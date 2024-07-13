# Jenkins Security Best Practices

## Introduction

It is critically important to keep your Jenkins instance secure, both to protect your information and to avoid executing malicious code from your Jenkins instance.

Your organization has information that is used to create value. Information is valuable and you must ensure the confidentiality, integrity, and availability of that information. Good security practices protect your information.

Your Jenkins environment is also a fully distributed build system. Each network connection is a potential point of entry. A bad actor could access your environment to launch a DDoS attack or a bot or other mischief. Remember that the code that runs your builds can be perverted to run anything! A user could inadvertently download malware from a web site that is not protected, and this malware could then infect Jenkins.

## Security Concepts

Security is the set of practices and tools to fight and prevent threats. The major principles for security are:

- **Least privilege:** Give people the privileges required to do their jobs but do not give everyone permission to do everything and do not open ports that are not required for your work.
- **Know the system:** The more you understand about how your system works, the more you are prepared to protect the integrity of your system.
- **Defense in Depth:** Systems are layered. Put security on all layers.
- **Prevention is good. Detection is better:** Monitor your Jenkins installation constantly to detect signs of a security breach quickly.
- **Keep your system current:** Always apply Security Updates as soon as possible! Keeping the Jenkins software and all plugins current also ensures that your system is secured.

## Principle of Least Privilege

Handling the "Least privilege" concept helps you manage the 'AAA' concepts:

- **Authentication:** Control who can access the system.
- **Authorization:** Control what each user can do on the system.
- **Accounting:** Monitor your system to ensure that only valid processes are executing.

## Know the System: How Jenkins Executes a Pipeline

A simple overview of how Jenkins executes a Pipeline helps us understand the security considerations.

By default, the Pipeline executes with the full privileges of the Jenkins administrator, although you can configure Jenkins to execute Pipelines with fewer privileges. All of the Pipeline logic, the Groovy conditionals, loops, and so on, execute on the controller.

When a Pipeline runs:
- For each build that runs, Jenkins creates a workspace where files for that build are stored.
- The Pipeline calls a series of steps, each of which is a script or command that does the real work and mostly executes using an executor on an agent.
- The agent:
  - Writes some files to the local node.
  - Sends data back to the controller.
  - Can also request information from the controller.

## How Jenkins Can Do Mischief

Jenkins and the Pipelines it runs must be able to do almost anything. This means that a malicious Pipeline could reconfigure the Jenkins instance, delete files, or launch malware attacks. A trusted user could visit an infected web site and accidentally introduce malicious code into the Jenkins instance.

## Agents and Security

An agent that executes on the Jenkins controller might be able to access Jenkins configuration files and the workspaces of other builds. An agent could also request information that belongs to other teams or organizations that share the controller. This may not be a concern for small Jenkins environments where all agents are trusted to the same degree that the controller is trusted and all users have the same level of access to all configured processes.

An agent can also write malicious code to the local disk so that the node is tainted. For maximum security, run all builds on ephemeral agents in the cloud that are destroyed at the end of the build job.

This is why you should never run builds on the Jenkins controller in production environments. A build job that uses an agent running on the controller node has access to Jenkins controller files and configuration, which poses a security risk.

A job that performs administrative tasks such as backup may run on the controller, but be sure to label the executor and only allow it to be used by jobs that use that label.

## Use Credentials to Secure Access to External Resources

Pipelines may need trusted access to external resources such as a Nexus, Artifactory, or Elasticsearch database.

To avoid having to code usernames and passwords in each Pipeline and share the information with everyone each time you change them, you can set up credentials that are defined by the administrator and called from the Pipeline code when accessing the external resource. The administrator can easily change the password frequently to reduce the time that any "stolen" credential information can be used. Pipelines that use that credential do not require modification when the password is changed.

## What We Will Learn

In the next sections, we will look at the Jenkins security provisions and learn how to use them effectively. This includes:

- How to authenticate users to ensure that they should have access to the Jenkins instance.
- How to use the authorization facilities to ensure that authenticated users can do their jobs but do not have permissions that they do not need.
- How to use the Jenkins accounting facilities to monitor your Jenkins instance for signs of a security breach.
- Understand the Jenkins global security settings and how to administer them.
- Know how to create and maintain credentials to secure access to external resources.
- Understand security updates.
