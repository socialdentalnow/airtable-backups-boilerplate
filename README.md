# Airtable Backup Boilerplate

> This project is a boilerplate meant to perform backus of Airtable Bases, at a regular interval.
> It is managed by AWS Lambda, used as crons. It stores the backups in an AWS S3 bucket.
>
> It is meant to be hosted on your own AWS Account, so you have complete ownership of the project and the backups.
> 
> In order to get started, fork this project and follow the install guide 

<!-- toc -->

- [Getting started](#getting-started)
  * [Install](#install)
  * [Configure your own Airtable settings](#configure-your-own-airtable-settings)
    + [Local development](#local-development)
    + [Start project locally](#start-project-locally)
    + [Push to production](#push-to-production)
  * [Deploy](#deploy)
  * [Error monitoring](#error-monitoring)
  * [Logs](#logs)
  * [Test](#test)
  * [Release](#release)

<!-- tocstop -->

## Getting started

### Install

```bash
nvm use # Select the same node version as the one that'll be used by AWS (see .nvmrc) (optional)
yarn install # Install node modules
yarn deploy # Deploy on staging environment
```

### Configure your own Airtable settings
#### Local development

Everything is already configured for a quick getting started in `mocks/test-event.json`. 
This JSON object is a mock of what's provided to the crons through the `input` object.

Explanations:

* **AIRTABLE_BASE** is the base you want to use
* **AIRTABLE_TABLES** are the tables name to backup, split by a `;`
* **S3_DIRECTORY** is the S3 bucket sub-directory to use

Fill free to change the S3 bucket's name in `serverless.yml`:
```yaml
custom:
  bucket: my-airtable-backups
```

#### Test project locally

```bash
yarn invoke:airtableBackupsBoilerplate
```

> This uses the `sls invoke local` feature under the hood.

If the output of serverless is :
```json
{
    "statusCode": 200,
    "body": "Successfully created backup"
}
```

Then it means the backup was successfully created. You should now check if your backup exists on your S3 bucket. 

---

## Deploy

> Before deploying, search for all `TODO` in the code source and resolve them. (serverless.yml)
>
> They're mostly there to highlight changes that you should perform before deploying this project on AWS.

```bash
NODE_ENV=production yarn deploy # Deploys to production
```

## Error monitoring

We use Epsagon in this boilerplate to monitor errors and lambda invoke. It's very handy for faster debugging.

You can set up your own credentials [in serverless.yml](./serverless.yml):
```yaml
custom:
  epsagon:
    token: '' # TODO Set your Epsagon token - Won't be applied if not provided
```

This is completely optional and opt-in. You're opt-out by default.

---

## Logs

- **airtableBackupsBoilerplate** funciton:
```bash
yarn logs:airtableBackupsBoilerplate
```

- **status** function:
```bash
yarn logs:status
```

Similar to reading the logs from the AWS Console

---

## Test

```
yarn test
yarn test:coverage
```

---

## Release

Will prompt version to release, run tests, commit/push commit + tag

```
yarn release
```


> Check the [./package.json](./package.json) file to see what other utility scripts are available