# Source Code Management (SCM)

Source code management systems (SCMs), also known as version control systems (VCSs), are software systems that record all changes for a set of files over time. This allows you to share those changes and provide merging and tracking history of the recorded changes.

When working as a development team, using a modern SCM enables you to:

- Collaborate efficiently on a single codebase.
- Allow team members to comment on changes before they are merged to the codebase.
- Help resolve code conflicts.
- Make it easy to share contents.
- Track every change. SCM is considered the Single Source of Truth.
- Provide a complete modification history.
- Allow easy rollback to an earlier version.

## A Brief History of SCM

The first SCMs were developed in the 1970s. They only tracked modification history. Newer SCMs add many capabilities, such as:

- Multiple people can share a codebase and collaborate on modifications.
- Version databases can be distributed or hosted on a cloud-based web server.
- Support for multiple branches.
- Support for online text editor, visual tools, issue tracker, etc.

Jenkins can work with virtually any modern SCM.

## Advanced SCM

Advanced SCMs support the most powerful Jenkins capabilities, allowing a Pipeline run to be triggered by a code modification. Git technology supports this ability.

Git is the core technology used in many cloud-based SCM systems, including GitHub, Bitbucket, GitLab, Visual Studio Team Services, Gitea, Gogs, Assembla, Helix, and Deveo. Most of these cloud-based SCM systems are also available as on-premise solutions. Pull-based SCM is also supported by other SCMs such as Mercurial (in Bitbucket) and Perforce.

## SCM Repository

Code is stored in a repository (or "repo"). Creating separate repositories (one per product, for example) allows more granular control and is generally considered a best practice for Jenkins. You control who can access each repo and define whether they have read, write, or admin privileges. You can also enforce "good practices," such as requiring that each code modification is reviewed and approved by someone other than its author.

### Note the following about a repository structure:

- Each repository has a branch that contains the current official version of the code. Traditionally, this was called the main branch, although this terminology is expected to change soon.
- The Jenkinsfile that specifies how Jenkins runs the build cycle is a text file that should usually reside in the root directory of the repo that contains the code being built. Some software, such as the Blue Ocean pipeline editor, only works if the Jenkinsfile is in the root directory of the repo.

## Modifying the Code

The workflow to modify code that is stored in an SCM can be summarized as:

1. Clone (copy) the repo to your local computer.
2. Create a branch for the change you want to make. A branch should represent a small, discrete change, which is typically the amount of work you can do in a few hours or less.
3. Modify your local codebase and test your changes locally.
4. Create a commit that contains the modified codebase.
5. Push your commit to the remote, official repository.

## Updating the Repository

After you push your changes to the remote repository, create a Pull Request (PR) to request that it be merged into the main branch. Jenkins then builds and tests the contents of your changes against what is currently in main and flags any issues.

- Colleagues review, comment, and approve your PR.
- The contents of the PR can be modified on the remote site or you can push additional commits from your local clone.
- When ready, the PR can be merged to the main branch.

Git is very flexible and very powerful and you can do much more than is listed here. For example, you can:

- Fork a repo on the remote and then clone it.
- Combine several commits into a single PR.
- Push content between different forks and repos.

## SCM Terminology

This section defines some of the most common terms used when discussing Git and other SCMs.

- **diff**: A set of changed lines on a single file.
- **commit**: A set of diffs that have been explicitly validated. A commit is actually a new version of the codebase. It can exist only locally or remotely. The latest commit on the history is the Head.
- **branch**: A line of development.
- **Head**: The "latest" commit on the current branch.

To integrate a branch, you must merge it:

- **Pull Request (PR)**: Contains the commits you want to add to the official code. It is pushed to the central server, where it can be reviewed and modified before it is merged to the destination branch. A pull request ends by being closed or merged.

    
