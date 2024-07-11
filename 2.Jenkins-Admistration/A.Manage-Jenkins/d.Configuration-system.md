# Configure System in Jenkins

Use the **Manage Jenkins Configure System** page to configure various standard aspects of your Jenkins server, including:

## General System-wide Configurations

- **JDK installations**
- **Build tools** - Ant and Maven installations
- **Version control tools**
- **Email configuration**

### Home Directory

Display the Jenkins home directory. Modify the value of the JENKINS_HOME environment variable to change this location.

### System Message

Text displayed at the top of your Jenkins home page, useful for server name, purpose description, downtime announcements, and contact information. Supports HTML tags or links to Wiki pages.

### Executors on Controller

Configure the number of executors on the controller. Set to 0 to prevent builds from running on the controller.

### Quiet Period

Interval to wait before starting a build, useful for collapsing adjacent commits into one build.

### SCM Checkout Retry Count

Retry count for source code management checkout retries on network errors or build cancellations during initial SCM clone.

### Jenkins Location

Define:

- **Jenkins URL**: Access location for this Jenkins instance.
- **System Admin E-mail Address**: Email address for general Jenkins issues.

### Serve Resource Files from Another Domain

Specify a separate URL for hosting static files like build artifacts or user-generated resources, distinct from the Jenkins controller URL.

### Global Properties

Configure:

- **Disable Deferred Wipeout on This Node**: Asynchronous deletion of resources.
- **Environment Variables**: Global environment variables for resource location.
- **Tool Locations**: Override default or specify tool locations on a node.

### Pipeline Speed / Durability

Choose settings to affect running pipeline durability and performance:

- **None**: Uses pipeline default (MAX_SURVIVABILITY).
- **Performance-optimized**: Faster, restarts may lose step information.
- **Less Durability**: Faster, potential for build pipeline data loss.

### Folder

Configure health metrics for folders:

- **Child Item with Worst Health**: Health based on nested items.
- **Health of Primary Branch**: Report health for multi-branch project primary branch.

### Global Build Timeout

Set a global build timeout limit for long-running builds to manage stuck or frozen builds.

### Timestamper

Define timestamp display format.

## More about System Configuration

Jenkins is flexible. Start with default configurations and adjust as you learn about Jenkins and your project's needs. Fine-tune settings on this page to optimize Jenkins instance behavior.

When done, scroll to the bottom and click **Save** to apply configuration changes.
