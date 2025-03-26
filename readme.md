The `gh_slack_reminder` GitHub Action is designed to send reminders to Slack channels about pending pull requests in your repository. This helps teams stay informed and ensures that pull requests receive timely reviews.

## Features

- **Automated Slack Notifications**: Sends messages to specified Slack channels about open pull requests.
- **Customizable Queries**: Tailor the GitHub search queries to filter pull requests based on labels, status, or other criteria.
- **Flexible Scheduling**: Integrate with GitHub Actions' scheduling to send reminders at desired intervals.

## Prerequisites

Before setting up the action, ensure you have:

- **Slack Incoming Webhook URL**: This is required to send messages to your Slack workspace.
- **GitHub Personal Access Token (PAT)**: Needed to authenticate and fetch pull request data.

## Setup Instructions

1. **Obtain Slack Incoming Webhook URL**:
   - Navigate to your Slack workspace and create an incoming webhook.
   - Note down the provided webhook URL; it will be used in the GitHub Action.

2. **Generate GitHub Personal Access Token**:
   - Go to your GitHub account's settings.
   - Under "Developer settings," select "Personal access tokens" and generate a new token with the necessary permissions.
   - Keep this token secure; it will be used to authenticate API requests.

3. **Store Secrets in GitHub Repository**:
   - In your repository, navigate to "Settings" > "Secrets and variables" > "Actions."
   - Add the following secrets:
     - `SLACK_WEBHOOK_URL`: Your Slack incoming webhook URL.
     - `GITHUB_TOKEN`: Your GitHub Personal Access Token.

4. **Configure the GitHub Action Workflow**:
   - Create or update the workflow YAML file (e.g., `.github/workflows/slack_reminder.yml`) in your repository with the following content:

     ```yaml
     name: Slack Pull Request Reminder

     on:
       schedule:
         - cron: '0 9 * * 1-5'  # Runs at 9 AM UTC, Monday to Friday
       workflow_dispatch:

     jobs:
       slack-reminder:
         runs-on: ubuntu-latest
         steps:
           - name: Checkout Repository
             uses: actions/checkout@v3

           - name: Send Slack Reminder
             uses: Sonichigo/gh_slack_reminder@main
             with:
               SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
               GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
               ORG_NAME: 'your-slack-org'
     ```

     **Notes**:
     - The `cron` schedule is set to run the action at 9 AM UTC from Monday to Friday. Adjust the timing as needed.
     - The `slack_channel` input specifies the Slack channel where reminders will be sent. Replace `'your-slack-org'` with your org name, where the bot is added.
     - The `github_query` input allows you to customize the search criteria for pull requests. The example above filters for open pull requests that require a review.

5. **Commit and Push the Workflow**:
   - Save the workflow file and push it to your repository's main branch.
   - The action will now run based on the defined schedule or can be triggered manually via the "Run workflow" button in the Actions tab.
