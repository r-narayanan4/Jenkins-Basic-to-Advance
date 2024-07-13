# Managing Credentials in Jenkins

## Overview

Credentials in Jenkins are used to get trusted access to other resources without sharing the actual passwords with all users. Jenkins itself uses credentials to secure internal communication, and administrators can define credentials that Pipelines use to access secured resources.

For detailed information, refer to the [Credentials API Plugin documentation](https://www.jenkins.io/doc/developer/security/secrets/).

## Why Use Credentials

Using credentials has the following advantages:
- Provides centralized credentials management
- Treats credentials as Jenkins Resources
- Provides an API consumable by plugins and other Jenkins Resources

Plugins may add additional credential types, such as the GitHub/Google/Bitbucket OAuth plugins and the SSH agents plugin.

## How Credentials Relate to Resources

Credentials are implemented through plugins for external resources. Each plugin defines the credential types it supports and may not support all credential types. For example, the Git Plugin can only use username/password and private key credentials.

## How Credentials are Used in a Pipeline

Pipelines can use credentials to access external resources, such as Nexus, Artifactory, or Elasticsearch. Administrators can define a credential that knows the username and password for the external resource. Credentials are defined under **Manage Jenkins > Global Security > Configure Credentials** on the Jenkins dashboard. Pipelines can use the `environment` or `withCredentials` directive to supply the username and password.

## Lowest Level Principle

Limiting the number of projects that can access a credential enhances its security. Credentials should be defined at the lowest possible level. Credentials defined for the controller are available to all Pipelines run by that controller. Credentials defined for a Folder are only available to Pipelines run from that Folder.

## How Credentials are Stored

Credentials are stored in an encrypted form on the Jenkins controller. They are encrypted using a key derived from the master key. Keys are tied to and encrypted by the instance ID, which means they cannot be migrated or copied to a new instance.

## Protecting Secrets

Keys to decrypt secrets are stored in the `$JENKINS_HOME/secrets/` directory, which has restrictive Linux file system permissions and should be excluded from backups. Keys should never be stored in SCM where their contents could be breached.

## Adding Credentials

1. Open **Manage Jenkins > Credentials** on the Jenkins dashboard.
2. Select **(global)** under Domains in the Stores scoped to Jenkins section.
3. Fill in the form to define the credential:

### Fields to Define Credentials

- **Kind**: Select the type of credentials to define the behavior.
  - **Secret file type**: `MYVARNAME` contains the path of the file that contains the Secret Text.
  - **Standard username and password type**:
    - `MYVARNAME` is set to `<username>:<password>`
    - `MYVARNAME_USR` is set to just the username
    - `MYVARNAME_PSW` is set to just the password
- **Scope**: Defines where this credential can be used.
  - **System**: For the Jenkins instance itself.
  - **Global**: For a Pipeline project or item.
- **ID**: Assign an ID that can be used to access the credential. IDs should be 40 characters or less and use alpha-numerical characters plus the separator characters `-`, `_`, `.`.

## Example Declarative Pipeline Code for Credentials

```groovy
stage('Deploy Reports') {
  steps {
    withCredentials(bindings: [string(credentialsId: 'elastic-access-key', variable: 'ELASTIC_ACCESS_KEY')]) {
      // Environment Variable available in the remote shell
      sh 'env | grep ELASTIC_ACCESS_KEY'
      sh 'echo ${ELASTIC_ACCESS_KEY} > secret-file.txt'

      // Groovy Variable
      sh "echo ${ELASTIC_ACCESS_KEY} > secret-file.txt"
    }
  }
}
```


- This snippet uses the withCredentials() method to implement credentials. It uses the credential that is defined with the ID of elastic-access-key. withCredentials binds that to a local Pipeline variable called ELASTIC_ACCESS_KEY.

- The deploy-token step uses the ELASTIC_ACCESS_KEY local Pipeline variable to establish the credentials that it needs. Anyone who can run this Pipeline gets the necessary credentials to run deploy-token.

- Note the use of single quotes and double quotes in this example. Pipeline only expands variable references that are enclosed in double quotes; it does not expand variable references in single quotes. To limit access to the external resource (Elasticsearch in this case), restrict permissions to run this Pipeline.

For further reading

- [Using Credentials](https://www.jenkins.io/doc/book/using/using-credentials/)

- [Storing Secrets](https://www.jenkins.io/doc/developer/security/secrets/)



