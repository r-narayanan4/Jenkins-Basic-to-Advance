# Global Security Settings in Jenkins

## Overview

Global security settings close off intrusion paths for the Jenkins instance. By default, Jenkins is installed with restrictive settings to reduce the attack surface of the Jenkins cluster. You may need to relax these settings to run your jobs, but it is recommended to reduce protection only when necessary.

## Access Control for Builds

By default, all Pipelines and jobs run with administrator privileges, which means any Pipeline can reconfigure Jenkins, delete or modify files, etc. Set the Access Control for Builds to limit the permissions used for builds.

## Other Global Security Settings

The Global Security Settings screen includes fields to configure or enable various features that have security ramifications. By default, Jenkins is installed in "locked-down" mode, and features that can have security ramifications are turned off. You may need to enable some of these features to get the required functionality. Plugins and practices are available to reduce the security vulnerability of these features.

### CSRF (Cross-Site Request Forgery)

CSRF is an exploit that enables an unauthorized third party to perform requests against a web application by impersonating another, authenticated user. In a Jenkins environment, a CSRF attack could allow a malicious actor to delete projects, alter builds, or modify the system configuration of the Jenkins instance. Jenkins enables CSRF Protection by default, and it is strongly recommended to keep it enabled.

When CSRF Protection is enabled, Jenkins checks for a CSRF token (or "crumb") on any request that might change data in the Jenkins environment. This includes form submissions and calls to the remote API, including those that use "Basic" authentication.

Challenges with CSRF Protection:
- Accessing Jenkins through a poorly-configured reverse proxy may result in the CSRF HTTP header being stripped from requests, causing protected actions to fail. Enabling proxy compatibility may allow you to run your build without disabling CSRF protection.
- Outdated plugins that have not been tested with CSRF protection enabled may not function properly. In Jenkins 2.95 and earlier, CSRF protection made use of the remote API difficult. Since 2.96, using the remote API with username/API token authentication no longer requires a valid CSRF protection crumb, but you must use an API token rather than a password.

It may be necessary to disable CSRF protection if these issues interfere with your builds.

### Port for Inbound Agents

The port for inbound agents can be used to launch an application on a client desktop by using resources hosted on a remote web server. In older Jenkins releases, this was called the port for The JNLP (Java Network Launch Protocol), used primarily by Windows-based agents. Modern versions of Windows can use SSH, which is a better option in most cases.

Jenkins uses a TCP port to communicate with inbound agents. If you select Random, the port is chosen randomly, which helps to avoid collisions. However, firewalls may not be able to secure a random port, so choosing the Fixed option and defining a TCP port to use for JNLP is more secure. You can then configure your firewall accordingly. It is recommended to leave this port disabled unless explicitly needed.

### Markup Formatting

Jenkins allows user input in some text areas and configuration fields, such as job descriptions, user descriptions, view descriptions, and build descriptions. HTML formatting in these text areas can lead to users inadvertently or maliciously inserting unsafe HTML and/or JavaScript, which could be used for Cross-site Scripting (XSS) attacks.

Use the Markup Formatter field to control how HTML code is rendered on your Jenkins instance. By default, this is set to Plain text, which causes all HTML formatting to be ignored. This is the safest option, but users are deprived of the aesthetic advantages of formatted text. You can choose the Safe HTML option, which allows using a subset of HTML markup such as hyperlinks and bold and italic characters. Other plugins can be installed to support additional options for handling HTML markup. If such plugins are installed, they are included in the dropdown menu for this field.

### Content Security Policy (CSP)

The Content Security Policy (CSP) header is applied to static files served by Jenkins. It is set to a very restrictive default set of permissions to protect Jenkins users from malicious HTML and JavaScript files in workspaces, `/userContent`, or archived artifacts. You may need to relax the default rules for some popular, useful plugins. For more details, see Configuring Content Security Policy.

This course does not cover the details of defining Content Security Policy rules here, but you are encouraged to explore further if you have issues with HTML rendering that are not solved by setting the Markup Formatter field.

