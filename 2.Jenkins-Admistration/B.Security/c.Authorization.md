# Authorization in Jenkins

## Overview

Authorization defines who can do what and always occurs in the context of authentication. It determines the actions that each user is allowed to perform.

Authorization grants access rights to:

- **Resource**: Task, object, and/or action to manipulate
- **Role**: A set of rights grouped together by commodity
- **Requester**: User or group that has roles assigned and wants to manipulate resources

## Security Matrices

All Jenkins authorization is defined using a security matrix. This matrix arranges permissions (grouped by resource) across the top and identifiers of people down the left side, allowing for a clear visualization of authorizations.

## Authorization Schemes

Different authorization schemes provided in Jenkins include:

1. **Matrix-based Security**: Assigns global privileges to users and groups.
2. **Project-based Matrix Authorization Strategy**: Assigns permissions to users and groups based on the projects they access.
3. **Role-Based Strategy**: Assigns permissions to specific roles such as administrator, developer, and tester.

### Important Note

The Role Strategy plugin is not covered in this documentation. Do not confuse the Role Strategy plugin with the proprietary Role-Based Access Control (RBAC) plugin included in CloudBees CI. Both plugins allow you to set authorization based on roles, but their implementations are different.

## Configuring Authorization Scheme

The default authorization scheme is `Logged-in users can do anything`, which is seldom a good idea. It is recommended to choose one of the matrix-based strategies for better security.



![CI/CD Lifecycle](../Image/image14.png)
![CI/CD Lifecycle](../Image/image1.png)
![CI/CD Lifecycle](../Image/image15.png)

![CI/CD Lifecycle](../Image/image16.png)

![CI/CD Lifecycle](../Image/image17.png)

![CI/CD Lifecycle](../Image/image18.png)

![CI/CD Lifecycle](../Image/image19.png)

![CI/CD Lifecycle](../Image/image20.png)
