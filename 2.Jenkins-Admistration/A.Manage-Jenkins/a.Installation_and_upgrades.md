# Jenkins Installation and Planning Guide

## Requirements

### Supported Platforms
Jenkins runs on JVM and supports:
- Linux
- Windows
- macOS
- UNIX/BSD

### Java Version
Jenkins requires Java 8 or Java 11. Jenkins LTS supports Java 11 starting from Release 2.164.1. Java 9 and 10 are not supported.

### JVM Recommendation
For production, use JDK over JRE for troubleshooting tools.

## Disk Space Requirements

### Considerations
- Jenkins generates large artifacts, logs, and build files.
- Disk usage grows over time, especially with remote job hosting.
- Backups require significant space; separate filesystems are recommended for backup directories.

### Best Practices
- Offload artifacts, logs, and backups to external storage (e.g., Nexus, Artifactory).
- Use low-latency SSD drives for improved performance.
- Consider expandable volumes like LVM (Linux) or spanned volumes (Windows).
- Network mount disks using NFS or SAN.
- Jenkins home directory (`$JENKINS_HOME`) should have its own filesystem.

## Installing Jenkins

### Distribution Channels
Jenkins is available via:
- OS native packages (RPM, DEB)
- WAR file
- Docker image
- Cloud templates (AWS, Azure, etc.)

### Installing from Linux Packages
Use native packages for major distributions:

#### Red Hat Example
```bash
# Add Jenkins Yum Repository
wget -O /etc/yum.repos.d/jenkins.repo "http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo"
rpm --import "http://pkg.jenkins.io/redhat-stable/jenkins.io.key"
# Install Jenkins
yum install jenkins
# Start Jenkins
service jenkins start
```

# Jenkins Installation Guide

## Installation Methods

### Linux Native Packages

Linux packages are based on the standalone `jenkins.war` file:

- Creates a `jenkins` user.
- Sets up service scripts (`init.d`, `upstart`, or `systemd`).
- Follows native configuration file conventions.
- Provides built-in log rotation.

File Locations:
- Settings: `/etc/sysconfig/jenkins`
- `$JENKINS_HOME` defaults to: `/var/lib/jenkins`

### Installing on Microsoft Windows

Use one of the following methods:

- `setup.exe`
- `jenkins.msi` (requires .NET 2.0 runtime if not available)

Installs Jenkins as a Windows service with files in `%JENKINS_HOME%` folder.

### Running Jenkins as Standalone WAR

Run Jenkins with embedded Jetty server:

```bash
java ${JAVA_OPTS} -jar jenkins.war ${JENKINS_OPTS}
```

# Jenkins Startup Options and Deployment Methods

## Startup Options (`JENKINS_OPTS`)

Jenkins can be configured at startup using the following options:

- `--prefix $PREFIX`: Specifies URL prefix (default: `/`).
- `--httpPort $PORT`: Defines Jenkins port (default: `8080`).
- `--httpListenAddress $HTTP_HOST`: Binds Jenkins to specified IP address (default: `0.0.0.0`).
- `--logfile $LOGFILE`: Directs logs to specified file instead of stdout.

These options are passed as members of the `JENKINS_OPTS` variable when starting Jenkins.

For a complete list of startup flags, refer to the [Starting and Accessing Jenkins](https://www.jenkins.io/doc/book/installing/initial-settings/#configuring-http) documentation.

## Running Jenkins on an Application Server

Deploy the Jenkins WAR file to an existing application server:

This method may integrate better with existing infrastructures.

## Installing Jenkins in Containers

### Docker Container

Install Jenkins using the Official Docker image available on [DockerHub](https://hub.docker.com/r/jenkins/jenkins/). This provides native Docker infrastructure integration.

### Kubernetes Container

Deploy Jenkins using the Official Helm chart provided at [Jenkins Helm Charts](https://github.com/jenkinsci/helm-charts). Helm, the Kubernetes package manager, simplifies deployment and management.

[Jenkins X](https://jenkins-x.io) is another option for CI/CD on Kubernetes, providing an opinionated platform for Kubernetes-based Jenkins deployments.

## Post-Installation Wizard

After installation, complete the post-installation steps to configure Jenkins:

1. Access Jenkins at `http://<myServer>:8080`. Replace `<myServer>` with your server's name or IP address.
2. Follow the setup wizard to:
   - Unlock Jenkins.
   - Install essential plugins.
   - Create the initial admin user.

For detailed instructions, see the [Setup Wizard](https://www.jenkins.io/doc/book/installing/linux/#setup-wizard) documentation.

## `$JENKINS_HOME` Directory

`$JENKINS_HOME` refers to the Jenkins home directory, typically located at `/var/lib/jenkins` by default. This directory stores all essential data and builds for Jenkins.

The directory structure is depicted in the diagram below:

![CI/CD Lifecycle](../Image/image5.png)


## Jenkins upgrades

- Upgrade packages for Jenkins are published regularly. Long-term Support (LTS) packages are released every 12 weeks. The Manage Jenkins page tells you when upgrades are available:


- Upgrade to the latest LTS release to stay up-to-date. Always check the Release Notes and changelog before you upgrade.

- Interim packages are released weekly to make critical bug fixes and to make features available quickly to users who need them.

- Security releases are published as soon as possible and should be applied immediately to protect the integrity of your Jenkins instance.

## Install Jenkins upgrades

**To install a Jenkins upgrade**:

1. Right-click on changelog in the alert panel to open the changelog for this release in a new window or tab. Read this to be aware of changes this upgrade could make to your system.

2. Click download to download the upgrade software.

3. Use the Manage Jenkins > Prepare for Shutdown link to initiate a graceful shutdown of Jenkins.

4. Replace the jenkins.war file on your system with the new file.

    - On Linux systems, this is located in the /usr/share/jenkins directory by default.

5. Restart Jenkins to apply the upgrade.