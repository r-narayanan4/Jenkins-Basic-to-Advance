# About Pipelines

## What you will learn

- Continuous Delivery and Continuous Deployment
- What is Continuous Delivery/Deployment Flow?
- Advantages of Pipeline projects over Freestyle projects
- Scripted versus Declarative Pipeline
- Basic Pipeline structure
- How to use Blue Ocean and Classic Web UI tools on the Jenkins dashboard for creating and running Pipelines

## Continuous Delivery and Continuous Deployment

The "continuous" approach to software delivery requires that all the activities required to build, test, and deploy the software are straightforward and repeatable.

The continuous discipline encompasses two different strategies:

- "Continuous Delivery" is a software engineering approach in which teams produce software in short cycles, ensuring that the software can be reliably released at any time, although it does not actually deploy the software.

- "Continuous Deployment" is an extension of continuous delivery, but every change is automatically deployed into production.

Both disciplines aim at building, testing, and releasing software faster and more frequently. This approach helps reduce the cost, time and risk of delivering changes by allowing for more incremental updates to applications in production.

## Jenkins and continuous practice

Jenkins provides tools that implement the continuous practice by defining all the activities that must happen after the application is coded but before it can be deployed and ensuring that each build runs the same steps in the same order.

At a high level, the Jenkins workflow is:

- Teams produce software in short cycles, with code changes tested before they are merged into the official branch of the Source Code Management (SCM) system.

- Software can be reliably released at any time; applications in production can be updated incrementally.

- Continuous Delivery means that every change is ready to be deployed.

- With Continuous Deployment, every change is automatically deployed into production.

## Pipeline and continuous flow

The main requirement of Continuous Delivery is to set a "flow" that runs the whole process, starting from the SCM commit and going through building and testing, all the way until deployment.

The Pipeline builds and tests new code with the code that is already in SCM. One Pipeline can build the code for various platforms (such as JVM 8, JVM 11, and JVM 13) or for different operating systems (such as Linux, macOS and Windows) or different mobile operating systems (iOS and Android).

The flow usually defines several test stages. One Pipeline can run different levels of tests on different platforms. When the code passes all tests, it is ready to be deployed to production.

## Project types

This class teaches how to define and run a Declarative pipeline but you should know that Jenkins provides three different project types that can define your continuous project:

- Freestyle Projects

- Pipeline Projects

  - Declarative Pipeline

  - Scripted Pipeline

Freestyle ("chained") projects

Freestyle ("chained") projects (also called jobs) have been used to define the build flow almost from the day Jenkins was born. They use job orchestration tools such as the Job DSL plugin or Jenkins Job Builder.

Freestyle jobs are still supported and are useful in specific situations but are not recommended for most new developments.

Freestyle jobs only provide sequential steps although they can be chained with upstream/downstream. They are also not defined in code, only through the GUI, as was fashionable at the time, and do not provide centralized configuration.

Modern continuous delivery and deployment is often much more complex than what chained jobs can support. Even when the objectives can be achieved with Freestyle, the result is cumbersome, hard to maintain, and difficult to visualise. It is not possible — or difficult — Many continuous objectives cannot be solved or, at best, are very difficult to implement with Freestyle:

Requires (complex) conditional logic:

Continuous flows often require some conditional logic to be applied. In many cases, it is not enough to simply chain jobs in a linear fashion. Often, we do not want to simply say run job A, then run job B after Job A is finished, and follow it with job C. In real-world situations, things are more complicated than that. We want to run something (call it A), and, depending on the result, invoke B1 or B2, then run in parallel C1, C2 and C3, and, finally, execute job D only when C jobs are finished successfully.

Requires resource allocation and cleanup:

Resource allocation needs careful thought and is often more complicated than a simple decision to run a job on a predefined agent. There are cases when agent resources must be allocated dynamically; for example, workspace needs to be defined during runtime, or cleanup depends on the result of some action.

Involves human interaction for manual approval:

While continuous deployment process means that the whole flow ends with deployment to production, manual approval may needed at some point. For example, someone may need to confirm a step, or a failed process may need manual input about reasons for the failure and so on. Human interaction — allowing us to pause, inspect and resume the flow — must be supported as part of the project definition.

Should be resumable at some point on failure:

Some deployment flows run for a couple of hours or even days. If the hardware fails along the way, we need a mechanism to restart the deployment flow from a defined place rather than having to rerun it from the beginning.

Jenkins Pipeline

jenkins.io/doc/book/pipeline

Jenkins Pipeline is a tool for defining your Continuous Delivery/Deployment flow as code. It is a separate fundamental "type" in Jenkins that conceptualizes and models a Continuous Delivery process in code. Two syntaxes are supported:

- Scripted: sequential execution, using Groovy expressions for flow control

- Declarative: uses a framework to control execution

Both syntaxes use a customized DSL (Domain-Specific Language) that is based on Apache Groovy to programmatically manipulate Jenkins Objects. The Pipeline defines the entire continuous delivery process as code. Is not a job creation tool like the Job DSL plugin or Jenkins Job Builder.
	

Our first attempt to solve the deficiencies in Freestyle projects was the Build Flow plugin. We took the lessons from the Build FLow plugin and created the Workflow plugin, which was later renamed to Pipeline.

Some of the advantages of Pipeline over freestyle are:

Can be restarted

- All Declarative Pipelines can be restarted if necessary. CloudBees CI also supports checkpoints that allow the flow of a Scripted Pipeline to be restarted from defined points.
Advanced visualisation

- The idea, from the start, was to combine the flexibility and maintainability accomplished with DSL and Groovy with advanced visualisation. We define the flow through code and monitor the result through visualisation. Blue Ocean further enhances the visualization that is provided.
SCM friendly

- Pipelines are defined in the plain-text Jenkinsfile so can be stored in the same repository as the application code and can be treated like any other code. We can commit the Jenkinsfile to the repository, use pull requests, perform code reviews, and so on. With the Multibranch Pipeline feature, we can store the script in a Jenkinsfile and define different flows inside each branch.

We discuss Scripted and Declarative Pipeline syntax more below.

To summarize, Pipeline has the following advantages:

- Durable: The Jenkins controller can restart and the Pipeline continues to run

- Pausable: can stop and wait for human input or approval

- Versatile: supports complex real-world CD requirements (fork, join, loop, parallelize)

- Extensible: supports custom extensions to its "DSL" (Domain-specific Language)

- Reduces number of jobs

- Easier maintenance

- Decentralization of job configuration

- Easier specification through code

## Pipeline-as-code

A Pipeline is defined in a Jenkinsfile that uses a DSL based on Apache Groovy syntax. The deployment flow is expressed as code; it can express complex flows, conditionals and such.

The Jenkinsfile is stored in an SCM. Pipeline works with SCM conventions such as Branches and Git Pull Requests and applies versioning, testing and merging against your CD Pipeline definition. Classic Web UI configuration is possible.

Pipeline-as-code allows the entire team to collaborate on the continuous delivery process: from development, to QA, all the way through to operations and production.

## Jenkins vocabulary

- Controller: (previously called "master") is a computer, VM or container where Jenkins is installed and run. It serves requests and handles build tasks.

- Agent: (previously called "slave") is a computer, VM or container that connects to a Jenkins controller. It executes tasks when directed by the controller and has a number and scope of operations to perform.

- Node is sometimes used to refer to the computer, VM or container used for the controller or agent. Be careful because "Node" has another meaning for Scripted Pipeline.

- Executor: is a computational resource for running builds. It performs operations and can run on any controller or agent, although running builds on controllers is strongly discouraged because it can degrade performance and opens up serious security vulnerabilities. An executor can be parallelized on a specific controller or agent.

## Jenkins Pipeline sections

The Jenkinsfile that defines a Pipeline uses a DSL based on Apache Groovy syntax.

The basic structure of a Declarative Pipeline is simple and straightforward:

- It is structured in sections, called stages, each of which defines a chunk of work to be done.

- Each stage includes steps that execute the actual programs and scripts to be run.

- Some steps are built into Pipeline, and many others are provided in plugins.

- An agent statement defines the node where the programs and scripts execute. You can define one agent to run the entire Pipeline or you can specify different agents for different stages.

- The agent section must be coded differently for agents that run on Kubernetes.

We will say much more about these components later and, of course, a real-world Pipeline gets much more complex.

## Declarative and Scripted Pipelines

Declarative and Scripted Pipelines use different syntaxes, but both are defined in a Jenkinsfile under source code management (SCM) and both use the same pipeline subsystem.

Both rely on the Pipeline DSL, which is based on Apache Groovy syntax and both support shared libraries.

### Scripted Pipeline

Scripted pipeline was the original syntax. It uses the Pipeline DSL, which is based on Apache Groovy, to define build steps and accomplish in a single script flow what would require many freestyle jobs chained together.

The Pipeline DSL provides a simple way to define common tasks like accessing an SCM repository, defining the node on which a task should run, parallel execution and so on. More complicated tasks can be defined using Groovy, so the pipeline can use conditionals, loops, variables and so on. Groovy is an integral part of Jenkins, so we can also use it to access almost any existing Jenkins plugin or even to operate on Jenkins core features.

#### Summary of Scripted Pipeline:

- Executed serially, from top down
- Relies on Groovy expressions for flow control
  - Requires extensive knowledge of Groovy syntax
- Very flexible and extensible
  - Limitations are mostly Groovy limitations

Scripted pipeline works well for power users with complex requirements but novice users can easily mess up the whole system and most users do not need all the flexibility it accords.

### Declarative Pipeline

The declarative pipeline syntax provides a defined set of capabilities that let you define a pipeline without learning Groovy. It offers a stricter, pre-defined structure with the following advantages:

- Execution can always be resumed after an interruption, no matter what caused the interruption
- Requires only limited knowledge of Groovy syntax
  - Using Blue Ocean simplifies the Pipeline creation process even more
- Encourages a declarative programming model
- Reduces the risk of syntax errors
- Use the script step to include bits of scripted code within a Declarative Pipeline only when you have needs that are beyond the capabilities of Declarative syntax


This is illustrated in the following simple declarative Jenkinsfile:

```bash

pipeline {
  agent { label 'linux' }
  stages {
    stage('MyBuild') {
      steps {
        sh './jenkins/build.sh'
      }
    }
    stage('MySmalltest') {
      steps {
        sh './jenkins/smalltest.sh'
      }
    }
  }
}

```
## Course Focus and Tools for Working with Pipelines

This course is focused on Declarative Pipeline; scripted is an advanced topic that is touched on only briefly.

### Tools for Working with Pipeline

Pipelines can be developed and maintained using different tools:

- Graphical Blue Ocean Visual Editor with the embedded Code Editor
- Jenkins dashboard with the inline editor that is provided
- Jenkins dashboard with a text editor and standard SCM tools

All tools result in the same Jenkinsfile that defines the Pipeline as code so the tools can be used interchangeably on the same Pipeline.

### Classic Web UI

Both scripted and declarative pipelines can be created and modified using the web UI. You code the Jenkinsfile using the text editor of your choice then use the Jenkins dashboard to run and configure the Pipeline.

Tools are provided to simplify the process: especially the Declarative Directive Generator for declarative pipelines and the Snippet Generator for scripted pipelines. Each of these can generate a line of code based on the task that is required and information you input to an appropriate form. All features for declarative and scripted pipelines can be implemented this way.

### Blue Ocean Graphical Editor

The Blue Ocean Graphical Editor greatly simplifies the tasks of creating and running declarative pipelines. It is a visual editor you can use to easily create, modify, and run multibranch declarative pipelines. It provides a visual editor that makes it very easy for novices to create a new Pipeline, Add/remove configuration, stages and steps. It includes a Code Editor that can be used to implement Pipeline features not supported by the GUI or to do edits that are easier to do with an editor.

- Round-trip to the Jenkinsfile file that is the Pipeline
- Supports Git, GitHub, GitHub Enterprise and Bitbucket Server and Cloud
- Supports most features that the Classic Web UI supports
- Provides visualization and analysis for the Pipeline run
- Generates a Jenkinsfile and stores it in the source code repository

### Blue Ocean Editor Limitations

- The Blue Ocean Visual Editor does not support all Pipeline capabilities.
- Use the Blue Ocean Code Editor or another text editor to incorporate such features into your Pipeline.
- The Declarative Directive Generator can help with the syntax when using the Code Editor or a text editor.
- Blue Ocean can run a Pipeline that includes these features:
  - Specify environment variables within a block
  - `post` section that defines actions that run at the end of a Pipeline run or an individual stage
  - Apply options to the Pipeline
  - Use the `when` directive
  - Define and access credentials
