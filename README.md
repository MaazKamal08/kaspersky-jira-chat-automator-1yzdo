# kaspersky-jira-chat-automator-1yzdo

## Overview

This n8n workflow monitors a specific Gmail inbox for Kaspersky security alerts from 'any email'. It filters emails by subject keywords indicating critical issues (e.g., license expired, protection disabled). The workflow then robustly parses the email body to extract 'status of device' alerts, consolidates these into a single summary, creates a Jira Task ticket in 'Keenu ITSM' with this summary, and finally posts the Jira ticket link to a designated Google Chat space.

## Features

- Monitors Gmail for specific Kaspersky alert emails.
- Filters emails based on critical subject keywords.
- Robust parsing of email content, including HTML and plain text, to extract device status alerts.
- Normalizes extracted text data, removing HTML tags and extra whitespace.
- Combines multiple individual alerts from an email into a single, formatted Jira description.
- Automates the creation of Jira 'Task' issues.
- Notifies a Google Chat space with the link to the newly created Jira ticket.

## Services Used

- Gmail
- Jira Software Cloud
- Google Chat

## Trigger

The workflow is triggered every minute by the 'Gmail Trigger' node, checking for new emails from 'any email' that match specific subject keywords.

## Prerequisites

- An active n8n instance.
- A Gmail account (connected via OAuth2) that receives Kaspersky alerts from 'any email'.
- A Jira Software Cloud instance with a project named 'Keenu ITSM' (ID 10026) and 'Task' issue type (ID 10002) configured.
- A Google Chat space where notifications will be posted.

## Credentials

- Gmail OAuth2 account
- Jira Software Cloud API account
- Google Chat OAuth2 API account

## Configuration

1. Ensure the 'Gmail Trigger' node is connected to the correct Gmail account and monitoring the intended inbox.
2. Verify the 'Gmail Trigger' filter for sender 'any email' matches your incoming alert emails.
3. In the 'Code in JavaScript2' (email filter) node, adjust subject keywords if your Kaspersky alerts use different phrasing for critical issues.
4. In the 'Create an issue' node, confirm that the 'Project' (Keenu ITSM, ID 10026) and 'Issue Type' (Task, ID 10002) correspond to your Jira setup.
5. In the 'Send message and wait for response' (Google Chat) node, update the 'Space ID' to your target Google Chat room.

## Usage

1. Activate the workflow in n8n.
2. Ensure your configured Gmail account receives Kaspersky alerts from 'any email'.
3. The workflow will automatically detect new emails matching the sender and subject filters every minute.
4. Upon detection, it will parse the alert details, create a Jira ticket, and send a notification to Google Chat.

## Troubleshooting

- **No workflow trigger / No emails processed**: Verify the workflow is active. Check Gmail Trigger's execution history. Confirm the sender filter ('any email') is correct.
- **Emails not filtered correctly**: Review the subject keywords in the 'Code in JavaScript2' node to ensure they match your Kaspersky alert subjects.
- **Jira ticket content is incorrect/empty**: Inspect the input to 'Code in JavaScript' and 'Code in JavaScript1'. The parsing logic in 'Code in JavaScript' might need adjustments if Kaspersky email formats change.
- **Jira ticket creation fails**: Check Jira credentials. Ensure the Jira Project ID (10026) and Issue Type ID (10002) are valid for your Jira instance.
- **Google Chat notification fails**: Verify Google Chat credentials and ensure the 'Space ID' is correct and accessible by the configured Google Chat account.

## Security Notes

- **Credential Management**: Ensure all n8n credentials (Gmail, Jira, Google Chat) are securely stored and managed within n8n.
- **Jira Permissions**: The Jira credential should have the minimum necessary permissions to create issues in the specified project.
- **Google Chat Permissions**: The Google Chat credential should have permissions only to post messages in the designated space.
- **Email Sender Spoofing**: The workflow relies on the email sender for filtering. If high security is required, consider additional checks (e.g., DMARC verification, if available via email processing nodes) as sender addresses can sometimes be spoofed.
- **Data Exposure**: Be mindful that email content is processed. Ensure sensitive information is handled appropriately and not exposed in logs or other systems beyond what is necessary for Jira ticket creation.
