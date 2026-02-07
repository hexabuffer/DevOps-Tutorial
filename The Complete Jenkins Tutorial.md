The Complete Jenkins Tutorial You Will Ever Need
### Introduction

In the world of DevOps and continuous integration/continuous delivery (CI/CD), Jenkins is one of the most popular tools. It is an open-source automation server that helps automate various stages of software development, from building and testing to deployment. Jenkins supports multiple plugins, which extend its functionality, making it a powerful and flexible tool for any software development team. This tutorial will provide a comprehensive guide on how to use Jenkins, starting from installation and setup to creating and running pipelines with relevant code examples.

### Table of Contents

1. Introduction
2. Installing Jenkins
3. Setting Up Jenkins
4. Jenkins Overview
5. Configuring Jenkins
6. Jenkins Plugins
7. Creating Your First Jenkins Job
8. Jenkins Pipelines
9. Integrating Jenkins with Version Control Systems
10. Automating Tests with Jenkins
11. Deploying Applications with Jenkins
12. Jenkins Best Practices
13. Conclusion

### 1. Installing Jenkins

#### Prerequisites

Before installing Jenkins, ensure you have the following prerequisites:
- A machine with a supported operating system (Windows, Linux, macOS)
- Java Development Kit (JDK) installed (Jenkins requires Java 8 or Java 11)
- Internet connection to download Jenkins and necessary plugins

#### Installing Jenkins on Windows

1. **Download Jenkins**: Go to the [official Jenkins website](https://www.jenkins.io/download/) and download the Windows installer.

2. **Run the Installer**: Double-click the downloaded `.msi` file to start the installation process. Follow the on-screen instructions to complete the installation.

3. **Start Jenkins**: After installation, Jenkins should start automatically. You can also start it manually by running the Jenkins service from the Services panel.

4. **Access Jenkins**: Open your web browser and navigate to `http://localhost:8080`. You should see the Jenkins setup wizard.

#### Installing Jenkins on Linux

1. **Add Jenkins Repository**: Open a terminal and run the following commands to add the Jenkins repository:

```sh
wget -q -O — https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c ‘echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list’
```

2. **Install Jenkins**: Update your package index and install Jenkins:

```sh
sudo apt-get update
sudo apt-get install jenkins
```

3. **Start Jenkins**: Start the Jenkins service:

```sh
sudo systemctl start jenkins
```

4. **Enable Jenkins**: Enable Jenkins to start at boot:

```sh
sudo systemctl enable jenkins
```

5. **Access Jenkins**: Open your web browser and navigate to `http://your_server_ip_or_domain:8080`.

#### Installing Jenkins on macOS

1. **Install Homebrew**: If you don’t have Homebrew installed, install it by running the following command in your terminal:

```sh
/bin/bash -c “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. **Install Jenkins**: Use Homebrew to install Jenkins:

```sh
brew install jenkins-lts
```

3. **Start Jenkins**: Start Jenkins by running:

```sh
brew services start jenkins-lts
```

4. **Access Jenkins**: Open your web browser and navigate to `http://localhost:8080`.

### 2. Setting Up Jenkins

#### Initial Setup

When you first access Jenkins, you will be prompted to unlock Jenkins using an initial admin password. Follow these steps:

1. **Locate the Initial Admin Password**: The password is stored in a file on your system. The location of this file is displayed on the setup screen. On Linux, it is usually found at `/var/lib/jenkins/secrets/initialAdminPassword`.

2. **Enter the Password**: Copy the password from the file and paste it into the setup wizard to unlock Jenkins.

3. **Install Suggested Plugins**: Jenkins will prompt you to install plugins. Choose the “Install suggested plugins” option. This will install a set of commonly used plugins.

4. **Create First Admin User**: After installing the plugins, you will be prompted to create the first admin user. Fill in the details and click “Save and Finish”.

5. **Jenkins is Ready**: Click “Start using Jenkins” to complete the setup.

### 3. Jenkins Overview

#### Jenkins Dashboard

The Jenkins dashboard is the main interface where you can manage jobs, configure settings, and monitor builds. It consists of the following sections:

- **New Item**: Create a new job or pipeline.
- **People**: View and manage users.
- **Build History**: View the history of builds.
- **Manage Jenkins**: Access configuration settings and manage plugins.
- **My Views**: Create custom views to organize jobs.

#### Jenkins Nodes

Jenkins can distribute build jobs across multiple machines, called nodes. This helps in managing and balancing the load, especially in large projects. Nodes can be added and configured from the “Manage Jenkins” -> “Manage Nodes and Clouds” section.

#### Jenkins Jobs

Jenkins jobs are the fundamental units of work in Jenkins. Each job can be configured to perform specific tasks such as building code, running tests, and deploying applications. Jobs can be created from the dashboard by clicking “New Item”.

### 4. Configuring Jenkins

#### Global Configuration

Global configuration settings can be accessed from “Manage Jenkins” -> “Configure System”. Here you can configure various settings such as:

- **Jenkins URL**: Set the URL for accessing Jenkins.
- **JDK**: Configure Java Development Kit installations.
- **Git**: Set up Git installations.
- **Email Notifications**: Configure email notifications for build statuses.

#### Security Configuration

Securing Jenkins is crucial to protect your build environment. Jenkins provides several security options under “Manage Jenkins” -> “Configure Global Security”:

- **Enable Security**: Enable Jenkins security.
- **Realm**: Choose a security realm (e.g., Jenkins’ own user database, LDAP).
- **Authorization**: Configure authorization strategies (e.g., Matrix-based security, Role-based strategy).

#### Tool Configuration

Jenkins can be integrated with various tools and services. Tool configurations can be done under “Manage Jenkins” -> “Global Tool Configuration”. Common tools include:

- **JDK**: Configure JDK installations.
- **Maven**: Configure Maven installations.
- **Git**: Configure Git installations.
- **Gradle**: Configure Gradle installations.

### 5. Jenkins Plugins

#### Managing Plugins

Jenkins has a vast ecosystem of plugins that extend its functionality. Plugins can be managed from “Manage Jenkins” -> “Manage Plugins”. Here you can:

- **Install Plugins**: Browse and install new plugins from the available plugins list.
- **Update Plugins**: Check for updates and update installed plugins.
- **Uninstall Plugins**: Uninstall unnecessary plugins.

#### Essential Plugins

Here are some essential plugins you should consider installing:

- **Git Plugin**: Integrates Jenkins with Git repositories.
- **Maven Integration Plugin**: Integrates Jenkins with Maven projects.
- **Pipeline Plugin**: Enables the creation of Jenkins pipelines.
- **Blue Ocean Plugin**: Provides a modern user interface for Jenkins.
- **Slack Notification Plugin**: Sends build notifications to Slack.

### 6. Creating Your First Jenkins Job

#### Creating a Freestyle Job

Become a member

1. **Create a New Job**: From the Jenkins dashboard, click “New Item”. Enter a name for your job and select “Freestyle project”. Click “OK”.

2. **Configure Source Code Management**: Under the “Source Code Management” section, select “Git”. Enter the repository URL and credentials if required.

```sh
Repository URL: https://github.com/your-repo/your-project.git
```

3. **Build Triggers**: Configure how the job should be triggered. For example, you can trigger the job to run periodically, or when changes are pushed to the repository.

4. **Build Environment**: Configure the build environment settings if needed.

5. **Build Steps**: Add build steps to define the tasks the job should perform. For example, to build a Maven project, add a “Invoke top-level Maven targets” build step.

```sh
Goals: clean install
```

6. **Post-build Actions**: Configure post-build actions such as sending notifications or archiving artifacts.

7. **Save and Build**: Click “Save” to save the job configuration. To run the job, click “Build Now”.

### 7. Jenkins Pipelines

Jenkins pipelines are used to define the steps involved in building, testing, and deploying applications. Pipelines can be defined using a domain-specific language (DSL) based on Groovy.

#### Creating a Simple Pipeline

1. **Create a New Pipeline Job**: From the Jenkins dashboard, click “New Item”. Enter a name for your job and select “Pipeline”. Click “OK”.

2. **Pipeline Definition**: In the “Pipeline” section, define your pipeline using the Pipeline DSL.

```groovy
pipeline {
agent any
stages {
stage(‘Build’) {
steps {
echo ‘Building…’
}
}
stage(‘Test’) {
steps {
echo ‘Testing…’
}
}
stage(‘Deploy’) {
steps {
echo ‘Deploying…’
}
}
}
}
```

3. **Save and Build**: Click “Save” to save the pipeline configuration. To run the pipeline, click “

Build Now”.

#### Declarative vs. Scripted Pipelines

Jenkins supports two types of pipelines: Declarative and Scripted.

- **Declarative Pipeline**: Uses a more structured and simpler syntax. Recommended for most use cases.

```groovy
pipeline {
agent any
stages {
stage(‘Build’) {
steps {
echo ‘Building…’
}
}
}
}
```

- **Scripted Pipeline**: Uses a more flexible and powerful syntax based on Groovy.

```groovy
node {
stage(‘Build’) {
echo ‘Building…’
}
}
```

### 8. Integrating Jenkins with Version Control Systems

#### Integrating with Git

1. **Install Git Plugin**: Ensure the Git plugin is installed.

2. **Configure Source Code Management**: When creating or configuring a job, select “Git” under the “Source Code Management” section. Enter the repository URL and credentials if required.

```sh
Repository URL: https://github.com/your-repo/your-project.git
```

3. **Specify Branches**: Specify the branches to build. For example, to build the master branch:

```sh
Branches to build: */master
```

#### Integrating with GitHub

1. **Install GitHub Plugin**: Ensure the GitHub plugin is installed.

2. **Configure GitHub Repository**: When creating or configuring a job, select “GitHub project” and enter the repository URL.

```sh
Project URL: https://github.com/your-repo/your-project
```

3. **Configure GitHub Hook**: Set up a GitHub webhook to trigger Jenkins jobs when changes are pushed to the repository. In your GitHub repository settings, add a webhook with the following URL:

```sh
Payload URL: http://your-jenkins-server/github-webhook/
```

### 9. Automating Tests with Jenkins

#### Running Unit Tests

1. **Add Build Step**: In your job configuration, add a build step to run unit tests. For example, for a Maven project, add a “Invoke top-level Maven targets” build step.

```sh
Goals: test
```

2. **Publish Test Results**: Add a post-build action to publish test results. For example, for JUnit test results, add a “Publish JUnit test result report” post-build action.

```sh
Test report XMLs: **/target/surefire-reports/*.xml
```

#### Running Integration Tests

1. **Add Build Step**: In your job configuration, add a build step to run integration tests. For example, for a Maven project, add a “Invoke top-level Maven targets” build step.

```sh
Goals: verify
```

2. **Publish Test Results**: Add a post-build action to publish test results. For example, for JUnit test results, add a “Publish JUnit test result report” post-build action.

```sh
Test report XMLs: **/target/failsafe-reports/*.xml
```

### 10. Deploying Applications with Jenkins

#### Deploying to a Local Server

1. **Add Build Step**: In your job configuration, add a build step to deploy the application to a local server. For example, for a Tomcat server, you can use the “Deploy war/ear to a container” build step.

```sh
WAR/EAR files: **/target/*.war
Containers: Tomcat 8.x
```

2. **Configure Container**: Configure the container details such as URL, credentials, and context path.

```sh
Tomcat URL: http://localhost:8080
```

#### Deploying to a Remote Server

1. **Add Build Step**: In your job configuration, add a build step to deploy the application to a remote server. You can use SSH or SCP for this purpose. For example, use the “Send build artifacts over SSH” build step.

```sh
Source files: **/target/*.war
Remove prefix: target
Remote directory: /path/to/deploy
```

2. **Configure SSH Server**: Configure the SSH server details such as hostname, port, and credentials.

```sh
Hostname: remote.server.com
Port: 22
```

### 11. Jenkins Best Practices

1. **Use Declarative Pipelines**: Prefer declarative pipelines for their simplicity and structure.

2. **Use Version Control for Pipeline Scripts**: Store pipeline scripts in version control to track changes and collaborate with team members.

3. **Use Parameterized Builds**: Use parameterized builds to pass variables to your jobs and pipelines.

4. **Monitor Jenkins Performance**: Regularly monitor Jenkins performance and optimize build times.

5. **Backup Jenkins Configuration**: Regularly backup Jenkins configuration and job data to prevent data loss.

6. **Use Security Best Practices**: Follow security best practices such as enabling security, using strong credentials, and limiting user permissions.

### 12. Conclusion

Jenkins is a powerful and flexible automation server that can significantly improve your software development workflow. By following this comprehensive tutorial, you should now have a solid understanding of how to install, configure, and use Jenkins to automate various stages of your software development lifecycle. Whether you are building simple projects or complex applications, Jenkins provides the tools and features to help you achieve continuous integration and continuous delivery with ease. Happy building!
