# Global Tool Configuration in Jenkins

Navigate to **Manage Jenkins > Global Tool Configuration** to configure tools essential for Pipeline development.

## Default Tools

Configure the following tools based on plugin availability:

- **JDK and other languages**
- **Build tools**: Maven, Gradle, Ant, and others
- **Source Code Management**: Git, Mercurial, and others
- **Containers**: Docker and Kubernetes (requires installation)

## Adding Tools

To add a tool, click **Add** and Jenkins provides fields to supply necessary information for installation. Many tools support configuration of multiple versions and some offer auto-installation options.

### JDK

Define JDK versions required for your projects. Configure multiple versions as needed, with options for automatic installation from various sources.

### Maven

Automatically install Maven using the Maven section settings.

### Gradle

Provide a name and use the **Add Installer** option to install Gradle from the project's website.

### Ant

Provide a name and use the **Add Installer** option to install Ant from the Apache project website.
