# Distributed Builds in Jenkins

Before delving into Jenkins administration specifics, it's crucial to understand the terminology related to distributed builds architecture, which is recommended for all Jenkins production systems.

**Distributed Builds** are builds that run on nodes other than the built-in controller node. The Jenkins controller serves HTTP requests and stores all critical build-related information. Agents on nodes manage task execution on behalf of the Jenkins controller, providing most of the computing power needed for building and testing software.

In a distributed Jenkins instance, nodes are separate physical or virtual machines configured to allow the Jenkins controller to delegate actual build work. Different nodes can run different operating systems and tools, enabling a single Pipeline to build and test software across various platforms.

## Benefits of Distributed Builds

### Scalability

Running builds on agents enhances Jenkins scalability. Adding more agents increases build capacity without needing a new controller. A single controller can manage thousands of builds daily by distributing the load efficiently.

### Security

Jobs running on a controller have full permissions to all Jenkins resources on that node, posing security risks. Running jobs on separate, isolated machines mitigates this risk. Ephemeral cloud agents further enhance security by creating and destroying agents for each build, preventing contamination across builds.

### Specialized Nodes

Specialized nodes are tailored for different build and test stages, supporting diverse operating systems, CPU architectures, and tool versions. Each node is configured with specific tools required for its tasks, whether directly installed or within Docker or Kubernetes containers. Pipelines access these nodes using labels configured for each node.

## Distributed Jenkins Components

A distributed Jenkins setup includes:

- **Jenkins Controller**: The core Jenkins service, acting as a web server and decision-maker for task execution.
- **Node**: Servers where Jenkins runs build jobs using executors. The controller also runs on a node.
- **Executor**: A task execution slot, effectively a thread within an agent.
- **Agent**: Manages task execution on behalf of the Jenkins controller, utilizing executors. Agents are Java client processes that connect to the controller and are assumed unreliable but versatile in operating system support.

## Executors Configuration

Determining the number of executors per build node depends on available resources and workload requirements:

- One executor per node is the safest configuration.
- One executor per CPU core may suit small tasks.
- Monitor CPU, memory, I/O performance, and network activity when configuring multiple executors.

### Executors on the Controller

In production systems, it's advisable to configure the Jenkins controller node with 0 executors to prevent builds from running on it. Running builds on the controller grants full access to Jenkins home directory permissions, posing security risks.

Administrative tasks like backups can run on the controller temporarily with an executor defined, ensuring it's not used for build jobs afterward. For production deployments, separate remote agents are recommended.

**Note:** Running builds on the controller is suitable for testing or demonstration purposes but not for production use due to security implications.

This setup ensures Jenkins operates efficiently and securely, scaling as needed while protecting sensitive information and maintaining operational integrity.
