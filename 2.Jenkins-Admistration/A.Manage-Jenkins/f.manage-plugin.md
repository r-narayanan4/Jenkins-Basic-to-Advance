# Managing Plugins in Jenkins

Jenkins relies on plugins to provide much of its functionality, with over 1800 plugins available. Plugins allow for a modular architecture where you include only the features you need, avoiding unnecessary functionality in your installation.

## Functionality Provided by Plugins

Plugins implement various features in Jenkins, including:

- Source code management tools
- Build tools
- Reporting tools (code coverage, static code analysis, etc.)
- Online source code browsers
- Issue trackers
- Notification tools
- Views and UI customizations
- Distributed builds

## Plugin Internals

A plugin is a JAR file following specific conventions (e.g., no web.xml) with an `.hpi` or `.jpi` extension. These files are stored in the `${jenkins_home}/plugins` directory (or a specified location with `--pluginroot`).

Plugins can have dependencies, either mandatory or optional. They are versioned artifacts that can be upgraded, though downgrading is generally not recommended.

- **hpi**: Hudson PlugIn (historical)
- **jpi**: Jenkins PlugIn

If two plugins have the same name but different extensions, the `.jpi` version takes precedence.

## Configuring Build Notifications in Jenkins

Build notifications are crucial for providing feedback on Jenkins builds. They can be configured to deliver notifications through email, Slack, and other channels. Here's how to set them up:

## Preparing for Notifications

1. **Install Required Plugins**: Before configuring build notifications, ensure you have installed necessary plugins like Email Extension for email notifications or Slack Notification for Slack notifications.

2. **Configure System Settings**:
   - Navigate to **Manage Jenkins > Configure System**.
   - Configure the notification service (e.g., Email Extension or Slack).
   - Set up credentials, typically an API token or SMTP server details.
   - Specify other provider-specific options, such as rooms or team domains for Slack.

## Project-Specific Configuration

For each project, specify:

- **Trigger Conditions**: Define when to trigger a notification (e.g., on any status change, warnings, etc.).
- **Recipients**: Choose who receives the notifications based on triggers.
- **Message Content**: Customize the content of the notification message.

### Built-in Email Notifications

Jenkins includes a basic email notification feature:
- Configure the SMTP (sending) server with hostname, port, encryption, authentication, and SPAM configuration.
- Notifications include:
  - Email on every failed build.
  - Email on successful build after a failed (or unstable) build.
  - Email on unstable build after a successful build.

### Email-Ext Plugin

The Email Extension plugin enhances the built-in Jenkins Mailer:
- Provides finer triggers (e.g., second failure, aborted build, test improvement).
- Offers extensive customization with templates, variables, and script attachments.
- Allows changing recipients dynamically based on notification types.
- Configure globally via **Manage Jenkins > Configure System**, or per project under **Post Build Actions > Editable Email Notification**.

### Slack Notification Plugin

The Slack Notification plugin sends build notifications to Slack channels:
- Configure notifications for different build statuses (Start, Aborted, Failure, Unstable).
- Include custom message content using Jenkins environment variables, commit list, or authors.
- Notifications can be sent by bot users.
- Configure globally via **Manage Jenkins > Configure System**, or per project under **Post Build Actions > Slack Notifications**.

## Going further

Some recommended readings on this subject:

- Cleaning up and notifications

- Notification Plugin

- Email-ext Plugin

- Mailer Plugin

- Slack Plugin