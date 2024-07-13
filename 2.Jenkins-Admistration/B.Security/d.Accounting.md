# Accounting in Jenkins

## Overview

Accounting in Jenkins occurs in the context of a user who is authenticated and authorized. It measures resources used or consumed by the user, such as the amount of data, compute resources, or system time. Accounting enforces limits when they are defined to protect the system, which is related to system measurement, capacity planning, and feedback loops.

## Logs

### Running Jenkins.war Manually

When running `jenkins.war` manually with `java -jar jenkins.war`, all logging information is output to `stdout` by default.

### Logs on Linux Systems

By default, logs are available in the `/var/log/jenkins/jenkins.log` directory. The log location can be customized:

- In `/etc/default/jenkins` (for `*.deb`)
- In `/etc/sysconfig/jenkins` (for `*.rpm`)

### Logs on Windows Systems

By default, logs are available in:

- `%JENKINS_HOME%/jenkins.out`
- `%JENKINS_HOME%/jenkins.err`

The log location can be customized in the `%JENKINS_HOME%/jenkins.xml` folder.

### Logs on Docker

If you run Jenkins inside Docker as a detached container, use the `docker logs containerId` command to collect Jenkins logs.

### Logs on the Jenkins GUI

Go to **Manage Jenkins > System Log** to view the Jenkins logs. You can create a custom log recorder that groups relevant logs together while filtering out the noise.

## Monitoring Server Load

Jenkins provides built-in monitoring of server activity on the **Manage Jenkins > Load Statistics** page. This displays a graph of the server load time for the built-in controller node. The graph keeps track of four metrics:

1. Number of online executors
2. Number of busy executors
3. Number of available executors
4. Queue length

## Monitoring Plugins

Jenkins includes open-source plugins for monitoring. Some of the popular ones are:

- **Monitoring Plugin**: Uses JavaMelody to monitor Jenkins. It provides charts of CPU, memory, system load average, and HTTP response time, as well as details about HTTP sessions, errors and logs, actions for GC, heap dump, and invalid session(s).
- **Disk Usage Plugin**: Shows project-wide details for all jobs and all workspaces, as well as the Disk Usage Trend.
- **Build Monitor Plugin**: Provides a detailed view of the status of selected Jenkins jobs. It also shows the names of people who might be responsible for "breaking the build."

Additional monitoring plugins and practices are documented in the [Monitoring Jenkins documentation](https://www.jenkins.io/doc/book/system-administration/monitoring/).

## Auditing

Many open-source plugins are available for auditing. Some of the popular ones are:

- **Audit Trail Plugin**: Keeps a log of users who performed particular Jenkins operations, such as configuring jobs. This plugin adds an Audit Trail section in the main Jenkins configuration page. Many configuration options are supported, including:
  - Save output audit logs in rolling files.
  - Send audit logs to a Syslog server.
  - Output audit logs in `stdout` or `stderr`, primarily intended for debugging purposes.

- **Job Configuration History Plugin**: Saves a copy of the `config.xml` configuration file of a job for each change made and of the system configuration. This provides an overview page of all changes that were made. The overview page only lists changes made to the system configuration for performance reasons. Use links to view either all job configuration histories or just the deleted jobs, or all kinds of configuration history entries together. Note that this may take some time to load, especially if your instance is running many jobs.
