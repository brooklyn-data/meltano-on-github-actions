# Meltano on Github Actions

This template uses [cookiecutter](https://github.com/cookiecutter/cookiecutter) to generate a [GitHub Actions](https://github.com/features/actions) orchestrated [Meltano](https://meltano.com/) project.

For a list of pre-made Singer taps and targets, see the [Meltano Hub](https://hub.meltano.com/singer/taps/).

## Benefits to using GitHub Actions

- Requires only a GitHub account to deploy a fully automated Meltano project with logging, alerting, and incremental state
- Configuration is version controlled and maintained as code
- Low barrier to entry, a working project can be deployed on a schedule in a couple of hours
- Easy management of secrets
- Low cost. See the free minutes for your plan [here](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions#included-storage-and-minutes), and the incremental cost outside of your free minutes [here](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions#per-minute-rates).

## Limitations of using GitHub Actions

- There is a [6 hour maximum run time](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#usage-limits) for any isolated individual job in a workflow. This often means that backfills need to be performed outside of GitHub actions.
- No way of exposing the Meltano UI

## Usage prerequisites:
- Python >= 3.7
- pipx
- meltano
- cookiecutter

Install pipx with:
```bash
pip install pipx
pipx ensurepath
```

Install Meltano with:
```bash
pipx install meltano
```

Install Cookiecutter with:
```bash
pipx install cookiecutter
```

## Instructions

1. In your terminal, navigate to the parent folder in which you'd like the project to be created.
2. Run `cookiecutter https://github.com/brooklyn-data/meltano-on-github-actions` and follow the prompts.
3. From inside the newly generated project, search for all 'TODO' strings, and complete any actions required.
4. Once ready to publish, initialize Git with `git init`.
5. Create an empty repository in GitHub.
6. Take the `.git` URL of the newly created remote repository, and run `git remote add origin <.git url>`.
7. Stage and commit the generated project files with `git add -A` and `git commit -m 'Initial commit'`.
8. Make sure the branch is named `main` by running `git branch -M main`.
9. Finally, push the created project to the remote repository with `git push -u origin main`.
10. Configure any required secrets in the GitHub repo.

## Slack alerts
Slack alerts on failure are enabled [using the official Slack GitHub action](https://github.com/slackapi/slack-github-action), using '[Technique 2: Slack App](https://github.com/slackapi/slack-github-action#technique-2-slack-app)'. To configure:

1. [Create a Slack App](https://api.slack.com/apps) for the workspace, with a suitable name (e.g. Meltano).
2. Add the [chat.write](https://api.slack.com/scopes/chat:write) bot scope under OAuth & Permissions.
3. Install the app to the workspace.
4. Copy the app's Bot Token from the OAuth & Permissions page and add it as a secret in the repo settings named `SLACK_BOT_TOKEN`.
5. Invite the bot user into the channel you wish to post messages to (/invite @bot_user_name).
6. Copy the Slack channel's Channel ID (from the channel's About section, accessed by clicking the drop down arrow next to the channel's name) into another repository secret named `SLACK_CHANNEL_ID`.

# About Brooklyn Data Co
We are a full-stack data team and analytics team as a service, focused on leadership, process improvement, implementation, and advanced analytics. [Read more about what we do](https://brooklyndata.co/solution) and [join us!](https://brooklyndata.co/careers)
