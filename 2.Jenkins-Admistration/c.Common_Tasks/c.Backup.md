# Running backups

Having good backups of your Jenkins instance is critically important. Backups are used for:

## Disaster recovery

- Recovering an older configuration (an accidental configuration change may not be discovered for some time).

- Recovering a file that is corrupted or was deleted accidentally.

## This lesson discusses the following

- How to create a backup

- Files that should be backed up

- How to validate a backup to ensure that it is usable

We will also discuss what needs to be backed up and how to validate a backup.

## Creating a backup

### Various schemes can be used to create backups. These are discussed in this section

- Filesystem snapshots

- Plugins for backup

1. Write a shell script that backs up the Jenkins instance

## Filesystem snapshots

Filesystem snapshots provide maximum consistency for backups. They also run faster than live backups, reducing the possibility of copying different data at different time points. They are supported by the Linux Logical Volume Manager (LVM), Solaris ZFS (which also supports incremental backups), and other file system architectures. Some separate storage devices also let you create snapshots at the storage level.

## Plugins for backup

Several plugins are available for backup. Go to Manage Jenkins  Manage Plugins  Available and search for backup. Note that none of the open source plugins are currently being maintained-- you can try these plugins but you may have problems with them.

2. Writing a shell script for backups

You can write your own shell script that copies the appropriate files and directories to a backup location Use cron to schedule when the backup script runs.

The shell script should create a directory such as /mnt/backup to which the backup will be written (be sure that you have write permissions to that directory). Consider creating /mnt/backup as a separate filesystem with its own mount point. An alternative method is to create a subdirectory in /var. (Note that if you use this method, you might need to use the sudo command to execute the restore operation.) Backing up to /tmp is not advised because /tmp may be cleaned on reboot.

3. Create a unique identifier for each backup (use a timestamp, for example) to ensure that today’s backup does not overwrite yesterday’s backup.

4. Writing files to a local file system is the fastest way to take the backup. Consider copying the completed backup to a remote backup server or device for long term storage.

## What files to back up

The number of files you back up affects both the time required to run the backup and the size of the resulting backup. It also impacts the complexity of restoring the system from the backup. Here we discuss why various files should be backed up and list some files that could safely be excluded from at least some backups.

`$JENKINS_HOME`

Backing up the entire $JENKINS_HOME directory preserves the entire Jenkins instance. To restore the system, just copy the entire backup to the new system.

Note, however, that JENKINS_HOME includes a number of files that do not really need to be backed up. Selecting specific directories and files to back up yields smaller backups but may require a greater effort to restore a system. One approach is to back up different directories on different schedules.

### Configuration files

Configuration files are stored directly in the $JENKINS_HOME directory. ./config.xml is the main Jenkins configuration file. Other configuration files also have the .xml suffix. Specify $JENKINS_HOME/*.xml to back up all configuration files.

Configuration files can also be stored in an SCM repository. This keeps copies of all previous versions of each file that can be retrieved using standard SCM facilities.

`./jobs Subdirectory`

The $JENKINS_HOME/jobs directory contains information related to all the jobs you create in Jenkins.

`./builds — Contains build records`

`./builds/archive — Contains archived artifacts`

Back this up if it is important to retain these artifacts long-term

These can be very large and may make your backups very large

`./workspace — Contains files checked out from the SCM`

It is usually not necessary to back up these files. You can perform a clean checkout after restoring the system.

`./plugins/*.hpi — Plugin packages with specific versions used on your system`

`./plugins/*.jpi — Plugin packages with specific versions used on your system`

### What does not need to be backed up

The following files and directories do not usually need to be backed up:

`./war — Exploded war file`

- To restore a system, download the latest war file.

`./cache — Downloaded tools`

- To restore a system, download the current version of the tools.

`./tools — Extracted tools`

- To restore a system, extract the tools again.

`./plugins/xxx — Subdirectories of installed plugins`

These will be automatically populated on the next restart.

Alternatively, you can make an infrequent backup of just these files if you want to be able to easily restore the same versions of the system and all downloaded tools.

### Validating a backup

Your backup strategy should include validation of each backup. You do not want to learn that your backup is no good when you need it!

A simple way to validate a full backup is to restore it to a temporary location. Create a directory for the test validation (such as /mnt/backup-test) and restore the backup to that directory.

Set `$JENKINS_HOME` to point to this directory, specifying a random HTTP port so you do not collide with the real Jenkins instance:

```bash
export JENKINS_HOME=/mnt/backup-test
Now execute the restored Jenkins instance:

java-jar jenkins.war ---httpPort=9999
```

### What did we learn

- Making backups is a Jenkins best practice.

- Backups are critical for disaster recovery.

- Always set up a backup policy that defines:

  - The configurations and records that need to be saved from the controller

  - How often backups should be taken

  - Where backups should be stored

- Validate your backups.

  - You should periodically check whether your backups are intact and can be used to meet your recovery objectives.

## Going further

Some recommended readings on this subject:

- Why Smart, Efficient Backup and Restore Techniques are Essential with Jenkins Production Server

- Backup Plugin

- thinBackup Plugin

## Jenkins Backup: Data and Configurations

For a comprehensive guide on how to backup Jenkins data and configurations, please refer to the following resource:

[Jenkins Backup: Data and Configurations](https://devopscube.com/jenkins-backup-data-configurations/)

## Overview

Backing up Jenkins data and configurations is essential to ensure the integrity and continuity of your CI/CD processes. This guide provides detailed steps on how to perform backups, ensuring that you can restore your Jenkins environment in case of failures or data loss.

## Key Components to Backup

- **JENKINS_HOME Directory**: This directory contains all the necessary configurations, job data, and plugin information. It's the most critical part of the Jenkins setup that needs to be backed up regularly.

## Backup Methods

### 1. Manual Backup

- **Step 1**: Stop the Jenkins server to ensure data consistency.
- **Step 2**: Copy the `JENKINS_HOME` directory to your backup location using standard file copy commands.
- **Step 3**: Restart the Jenkins server once the copy is complete.

### 2. Scripted Backup

Automate the backup process using scripts to minimize manual intervention.

Example Bash Script:

```bash

#!/bin/bash
# Define the backup directory and Jenkins home directory
BACKUP_DIR=/path/to/backup/dir
JENKINS_HOME=/var/lib/jenkins

# Create a timestamp
TIMESTAMP=$(date +%F_%T)

# Stop Jenkins service
sudo systemctl stop jenkins

# Copy Jenkins home to the backup directory
cp -r $JENKINS_HOME $BACKUP_DIR/jenkins_backup_$TIMESTAMP

# Restart Jenkins service
sudo systemctl start jenkins

# Optional: Remove old backups to save space
find $BACKUP_DIR -type d -name "jenkins_backup_*" -mtime +30 -exec rm -rf {} \;
```
