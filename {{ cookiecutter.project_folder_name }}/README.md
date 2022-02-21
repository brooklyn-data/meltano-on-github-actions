# {{ cookiecutter.org_name }}'s Meltano project

This project has been generated using [Brooklyn Data Co](https://brooklyndata.co/)'s [Meltano on GitHub Actions template](https://github.com/brooklyn-data/meltano-on-github-actions).

## What's Meltano?

[Meltano](https://meltano.com/) is a configuration and orchestration manager for Singer taps and targets. A tap extracts data from a data source, and a target moves data into a destination.

## In this repo

This repo contains the configuration for a Meltano project and GitHub Action workflows which run Meltano jobs on schedules. The configured taps and targets can be seen within `meltano.yml`. Schedules are configured within `.github/workflows`.

## Local development

Prerequisites:
- Python >= 3.7
- `pipx`
- `meltano`

Install pipx with:
```bash
pip install pipx
pipx ensurepath
```

Install Meltano with:
```bash
pipx install meltano
```

### Local Meltano invocation

1. Create a `.env` file in this directory using `example.env` as a template and populate the blank values
2. Initialize the Meltano project:
```bash
meltano install
```
3. Test using Meltano:
```bash
# Test invocation:
meltano invoke tap-example --version
# Run a test `elt` pipeline:
meltano elt tap-example target-jsonl --job_id=test
```

The output streams from the tap will be written to `.jsonl` files within the `output` directory.

{%- if cookiecutter.add_target == "Snowflake" %}
## Snowflake configuration

We use [pipelinewise-target-snowflake](https://github.com/transferwise/pipelinewise-target-snowflake) to load data into Snowflake. See the pre-requirements for the target here: https://github.com/transferwise/pipelinewise-target-snowflake#pre-requirements

Use development credentials and database of `MELTANO_DEV` configured as default in `meltano.yml` for local testing.
{%- elif cookiecutter.add_target == "BigQuery" %}
## BigQuery configuration

We use [target-bigquery](https://github.com/adswerve/target-bigquery) to load data into BigQuery. See the pre-requirements for the target here: https://github.com/adswerve/target-bigquery#how-to-use-it

Use development credentials and project configured as default in `meltano.yml` for local testing.
{%- endif %}
## Deployment

This pipeline is deployed on GitHub Actions, and orchestrated by Meltano. Secrets are configured in GitHub actions per [this guide](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository). Note that configuration in `meltano.yml` is overridden by environment variables. The environment variables set are:

# TODO - add to or modify as needed
{%- if cookiecutter.add_target == "Snowflake" %}
- `TARGET_SNOWFLAKE_ACCOUNT`
- `TARGET_SNOWFLAKE_USERNAME`
- `TARGET_SNOWFLAKE_PASSWORD`
- `TARGET_SNOWFLAKE_DBNAME`
- `TARGET_SNOWFLAKE_WAREHOUSE`
- `TARGET_SNOWFLAKE_FILE_FORMAT`
{%- endif %}

[Artifacts](https://docs.github.com/en/actions/advanced-guides/storing-workflow-data-as-artifacts) are used to persist Meltano's SQLite database between runs. Doing so enables pipelines to run incrementally, i.e. only load new or updated data in each run.

GitHub Action configuration can be seen in `.github/workflows`. A new workflow is created for each pipeline, with independent state. Note that the `action-download-artifact` step requires that the workflow is not given a 'name' parameter.

## Slack alerts
Slack alerts on failure are enabled [using the official Slack GitHub action](https://github.com/slackapi/slack-github-action), using '[Technique 2: Slack App](https://github.com/slackapi/slack-github-action#technique-2-slack-app)'. Configuration:

1. [Create a Slack App](https://api.slack.com/apps) for the workspace, with a suitable name (e.g. Meltano).
2. Add the [chat.write](https://api.slack.com/scopes/chat:write) bot scope under OAuth & Permissions.
3. Install the app to the workspace.
4. Copy the app's Bot Token from the OAuth & Permissions page and add it as a secret in the repo settings named `SLACK_BOT_TOKEN`.
5. Invite the bot user into the channel you wish to post messages to (/invite @bot_user_name).
6. Copy the Slack channel's Channel ID (from the channel's About window, accessed by clicking the drop down arrow next to the channel's name) into another repository secret named `SLACK_CHANNEL_ID`.
