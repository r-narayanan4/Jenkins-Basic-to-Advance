# Working with Nodes in Jenkins

Nodes in Jenkins are servers where build jobs are executed. An agent manages the build execution on these nodes.

## Managing Nodes

Nodes can be managed via **Manage Jenkins > Manage Nodes and Clouds**:

- View, add, remove, control, and monitor nodes.
- Nodes are listed in the **Build Executor Status** block on the Jenkins dashboard and in the **Manage Nodes and Clouds** page.

### Creating a New Node

To create a new node:

1. Navigate to **New Node** in the left frame of **Manage Nodes and Clouds**.
2. Enter a name for the node and click **OK**.
3. Configure the node characteristics, including the number of executors and monitoring options.

### Configuring a Node

When configuring a node:

- **Name**: Identify the node on the dashboard.
- **Description**: Provide a description for human readability.
- **# of executors**: Set the number of executors based on workload requirements.
- **Remote root directory**: Define the filesystem directory for the controller on the agent machine.
- **Usage**: Select **Use this node as much as possible** to maximize resource utilization.
- **Launch method**: Choose **Launch agent via SSH** for secure agent communication.
- **Availability**: Opt for **Keep this agent online as much as possible** to maintain availability.

**Note:** Modify these settings cautiously, especially in production environments, to optimize node performance and resource utilization.

# Build Agents in Jenkins

Build agents in Jenkins are critical components that execute build jobs on nodes, providing necessary tools and environments for builds, such as JDK, Maven, Docker, etc.

## Using Agents

Agents can:

- Provide different environments for building and testing software.
- Execute builds within containers or Kubernetes pods.
- Be configured to match specific build requirements like JDK versions or OS environments.

### How Many Agents?

- Jenkins can support large clusters with 500+ nodes, limited mainly by system resources like memory and network bandwidth.
- Monitor Jenkins to ensure sufficient agents are configured to handle build queues effectively.

### Creating Agents

- Define agents with required tools and configurations for specific build operations.
- Use Docker containers or Kubernetes pods for agent templates.
- Attach agents to nodes where they will execute builds.

### Agent Architectures

Jenkins supports different agent architectures:

- **Dedicated Agents**: Restricted access for specialized hardware or software.
- **Fungible Agents**: Substitutable agents that provide basic build environments.
- **Cloud Agents**: Efficiently use resources by provisioning agents dynamically, e.g., with Docker containers or cloud platforms like AWS EC2.

### Controller-Agent Communication

- Bi-directional byte stream communication ensures no shared file system or network constraints between controller and agent.
- Access control ensures secure communication, customizable with allow/deny rules.

### Using Agents in Declarative Pipeline

- Pipelines define stages executed on agents.
- Use `agent any` to run on any available agent.
- Use `agent none` for stages not requiring an agent context.
- Labels can specify special-purpose agents in the Pipeline.

### Best Practices

- Never build on the Jenkins controller in production for security and scalability.
- Scale resources by distributing builds across agents.
- Restrict agent usage to specific projects when necessary.

## Further Reading

For more detailed information:

- [Distributed Builds Architecture](https://jenkins.io/doc/book/distributed/#distributed-builds)
- [Swarm Plugin](https://plugins.jenkins.io/swarm/)
- [Custom Tools Plugin](https://plugins.jenkins.io/custom-tools/)
- [Job Restrictions Plugin](https://plugins.jenkins.io/job-restrictions/)
- [Docker Slaves Plugin](https://plugins.jenkins.io/docker-slaves/)

Explore these resources to optimize and manage Jenkins agents effectively in your environment.
