# Continuous Workflow

The intent of the continuous workflow is to have the code in a valid and stable state at all times.

Think about the activities that might be required after the code has been completed, but before that modification can be released:

- Put the modified code into Source Code Management (SCM).
- Build code.
- Test the built code.
- Deploy the tested code.
- Freeze a copy of the deployed code.

In the real world, the process is more complicated. Each of these activities can be subdivided into multiple activities with inherent conditionals, such as:

- Only run tests if the build is successful.
- Only run long, complex tests if the simple tests pass.
- Only deploy the software if all tests have passed.

You may also want human intervention along the way; for example, before you deploy to production, you might require a manual approval.

We can represent the process with an illustration:

![image1](Image/image1.png)

In the not-so-good old days, the Development, Quality Assurance, and Packaging/Deployment groups were often siloed and the process was more like this:

- A large group of developers coded for months.
- Developers handed the "completed" code to Quality Assurance.
- Quality Assurance ran tests and handed bugs back to developers to fix.
- Developers fixed the bugs then handed the code back to Quality Assurance, who reran the tests.
- Quality Assurance handed the tested code to Packaging/Deployment.
- Packaging/Deployment released the product.

## Modern Development Philosophies

The philosophies of modern development emphasize collaboration between different teams, flexibility in planning and development, and shorter development cycles. Three major philosophies are interrelated:

- **Agile**: Emphasizes adaptive planning and evolutionary development. Work is planned and completed in "sprints" (usually 1-2 weeks of work), with frequent (usually daily) "scrums" where all team members report progress and plan their next steps.
- **DevOps**: Extends the Agile philosophy into operations and production by advocating for the automation and monitoring of all steps in the development cycle.
- **Continuous**: Implements Agile and DevOps philosophies with tools that standardize the steps in the process and thoroughly test each code modification before it is integrated into the official source.

### Summary: Get it done! Or Ship it!

- **Agile and continuous philosophies**: Agile mostly applies to the earliest steps of the process. Continuous applies to all stages through deployment.

![image2](image2)

## Continuous Philosophy

The continuous philosophy advocates that code be integrated often, at least daily, so that integration is a non-event. Builds are triggered automatically based on commit and merge actions and the success of upstream builds. In summary:

- Each integration is verified by an automated build (including tests).
- Automate the complete build-test-deploy cycle to ensure that activities always run in the same order.
- Build and test each code modification to find problems early, when they are easier to fix.
- Continuous Integration does not get rid of bugs, but it does make them dramatically easier to find and remove.

> Continuous Integration, Delivery, and Deployment
>
> - **Continuous Integration (CI)**: The frequent, automatic integration of code. All new and modified code is automatically tested with the master code.
> - **Continuous Delivery (CD)**: The natural extension of CI. It ensures that the code is always ready to be deployed, although manual approval is required to actually deploy the software to production.
> - **Continuous Deployment**: Automatically deploys all validated changes to production. Frequent feedback enables issues to be found and fixed quickly.

To successfully implement continuous delivery, it is essential to have a collaborative working relationship with everyone involved. You can then use Delivery Pipelines, which are automated implementations of your product’s lifecycle.

![image3][def]

## Jenkins Workflow

Jenkins automatically performs all the activities required to deliver your software. You specify how to build and test your software as well as when, where, and how to deploy it using these guidelines:

- Define a Jenkins Pipeline to run each activity in the same order every time.
- Pipeline is glue for the activities defined. Do not code build actions directly in the Pipeline! Instead, use shell scripts or a tool such as Apache Maven, Gradle, npm, Apache Ant, or make to define the specific actions required at each step and use the pipeline to define the execution order.
- The pipeline runs each time the code is modified.


[def]: /Image/image3.png