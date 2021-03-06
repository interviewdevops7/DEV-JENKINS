# DEV-JENKINS
JENKINS COMPLETE INTERVIEW

#### Complete Jenkins

Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.

Needed softwares:
1. Java
2. Maven
3. Git
4. Set up various software environment

Build Triggers Methods:
GIT ------- GIT HOOKS/ WEB HOOKS -------->Jenkins
GIT<-------POLL SCM ----------------------Jenkins
Pool SCM: Where Jenkins look for updates

[minutes] [hours] [day of the month] [month] [day of the week]
* * * * * command to be executed
- - - - -
| | | | |
| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
| | | ------- Month (1 - 12)
| | --------- Day of month (1 - 31)
| ----------- Hour (0 - 23)
------------- Minute (0 - 59)

-->The asterisk (*) : This operator specifies all possible values for a field. 
For example, an asterisk in the hour time field would be equivalent to every 
hour or an asterisk in the month field would be equivalent to every month.

-->The comma (,) : This operator specifies a list of values, for example: “1,5,10,15,20, 25”.

-->The dash (-) : This operator specifies a range of values, for example: “5-15” days , 
which is equivalent to typing “5,6,7,8,9,….,13,14,15” using the comma operator.

-->The separator (/) : This operator specifies a step value, for example: “0-23/” can be used in 
the hours field to specify command execution every other hour. Steps are also permitted after an asterisk, 
so if you want to say every two hours, just use */2.

(i) Access Control which includes authenticating users and giving them an appropriate set of permissions, which can be done in 2 ways.

Security Realm determines a user or a group of users with their passwords.
Authorization Strategy defines what should be accessible to which user. In this case, there might be different types of security based on the permissions granted to the user such as Quick and simple security with easy setup, Standard security setup, Apache front-end security, etc.
Matrix-based security:
Step 1. Go to Jenkins Dashboard -> Manage Jenkins -> Configure Global Security ->
Security realm : Select Allow users to sign up (under Jenkins own user database).
If account was locked and showing tAccess Denied <user> is missing the Overall/Read permission then
Open terminal and goto 
1.Edit config.xml file, e.g.
sudo vi /var/lib/jenkins/config.xml
2. Change useSecurity element's value to false, e.g.
<useSecurity>false</useSecurity>
3. Remove authorizationStrategy block
4. Restart Jenkins: /etc/init.d/jenkins restart.
(ii) Protecting Jenkins users from outside threats.

-> Upload Plugin file extension
            Two plugin formats 
			.hpi = hudson plugin interface
	        .jpi = jenkin plugin interface(new)
			
******************************* Adding Node to Jenkins ********************************
-> Jenkins Master & Node communication happens via SSH (Linux)
                                            via JNLP (Windows)
											
-> Jenkins Master to any other Linux Node
           Non Password communication(Key Base Authentication)

-> Jenkins Master we have to generate key on jenkins user.(Private & Public)
-> Copy that key in the nodes for communication (Public Key $ ssh-copy-id)		   
-> Jenkins Master to any Windows Machine we will download a Jenkins Slave .jar(From Global Security) and install on windows Node for accessing.

-> For Nodes to work with master we have to install Java on every node

Backup of Jenkins:
Two methods
1. CLI by just storing the back up of /var/lib/jenkins
2. Install ThinBackup plugin and conf the location

What are the ways to configure Jenkins node agent to communicate with Jenkins master?
Answer: There are 2 ways to start the node agent –

Browser – if Jenkins node agent is launched from a browser, a JNLP (Java Web Start) file is downloaded. This file launches a new process on the client machine to run jobs.
Command line – to start the node agent using the command line, the client needs the executable agent.jar file. When this file is run, it launches a process on the client to communicate with the Jenkins master to run build jobs.

Configure the default port of the Jenkins build server
The default port number can be changed in the config file at
$ sudo vim /etc/default/jenkins
Here you can set a different port number, e.g. HTTP_PORT=8082
Now you need to restart Jenkins with
$ service jenkins restart
or by adding /restart at the end of the Jenkins URL, e.g. https://yourjenkinsurl/ci/restart.

Declarative versus Scripted Pipeline syntax:

A Jenkinsfile can be written using two types of syntax - Declarative and Scripted.
Declarative and Scripted Pipelines are constructed fundamentally differently. Declarative Pipeline is a more recent feature of Jenkins Pipeline which:

Pipeline : A Pipeline is a user-defined model of a CD pipeline. A Pipeline’s code defines your entire build process, which typically includes stages for building an application, testing it and then delivering it.
Also, a pipeline block is a key part of Declarative Pipeline syntax.

Node: A node is a machine which is part of the Jenkins environment and is capable of executing a Pipeline.
Also, a node block is a key part of Scripted Pipeline syntax.

Stage: A stage block defines a conceptually distinct subset of tasks performed through the entire Pipeline (e.g. "Build", "Test" and "Deploy" stages), which is used by many plugins to visualize or present Jenkins Pipeline status/progress. [6]

Step: A single task. Fundamentally, a step tells Jenkins what to do at a particular point in time (or "step" in the process). For example, to execute the shell command make use the sh step: sh 'make'. When a plugin extends the Pipeline DSL, [1] that typically means the plugin has implemented a new step.

pipeline{
    agent any
    
    stages{
        
        stage('Checkout'){
            steps{
                
                git 'https://github.com/devops-trainer/DevOpsClassCodes.git'
            }
        }
       
        stage('Compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('Test'){
            steps{
                sh 'mvn test'
            }
             post{
              always{
                  
                  junit 'target/surefire-reports/*.xml'
              }
             }
        }
        stage('Package'){
            steps{
                sh 'mvn package'
				sh 'mvn pmd:pmd'
          }
        }
    }
	}

Difference between Continuous Integration, Continuous Delivery, and Continuous Deployment:
CI
CodeBuild --AUTO--> Unit Testing --AUTO--> Staging/Integration  --AUTO--> Acceptance Testing	
latest copy of the source code at a commonly shared hub where all the developers can check to fetch out the latest change in order to avoid conflict.

CD Delivery
CodeBuild --AUTO--> Unit Testing --AUTO--> Staging/Integration  --AUTO--> Acceptance Testing  --MANUAL-->  Production
Manual Deployment to Production. It does not involve every change to be deployed

Continuous Deployment:
CodeBuild --AUTO--> Unit Testing --AUTO--> Staging/Integration  --AUTO--> Acceptance Testing  --AUTO-->  Production
Automated Deployment to Production. Involves every change to be deployed automatically

Continuous Delivery Workflow
Git Clone --> Compile --> Unit Test --> Packaging (war) --> Deploy to PROD

Define the process of Jenkins.
First, a developer commits the code to the source code repository. Meanwhile, the Jenkins server checks the repository at regular intervals for changes.
Soon after a commit occurs, the Jenkins server detects the changes that have occurred in the source code repository. Jenkins will pull those changes and will start preparing a new build.
If the build fails, then the concerned team will be notified.
If the build is successful, then Jenkins deploys the build in the test server.
After testing, Jenkins generates feedback and then notifies the developers about the build and test results.
It will continue to check the source code repository for changes made in the source code and the whole process keeps on repeating.

Most popular environment variables
$JOB_NAME: Name of the project of this build. This is the name you gave your job.
$BUILD_NUMBER: The current build number
$WORKSPACE: The absolute path of the workspace.
$NODE_NAME
$BUILD_URL

Move a file from one server to the other:
Simply copy the job directory and paste it in the other server. Before that ssh keygen copy and sshd config set passwords authen true 
Goto var/lib/jenkins/jobs
$ scp -r <jobfile>/ ec2@<machine ip>:<dest loc>
$ scp -r raj1/ ec2@123.17.125:/root/jenkins/jobs/

Builds failing with OutOfMemoryErrors:
1.java.lang.OutOfMemoryError: Heap space 
When you see this, you need to increase the maximum heap space. You can do this by adding JVM arguments -Xmx200m where you replace the number 200 with the new heap size in megabytes.
2. java.lang.OutOfMemoryError: PermGen space.
you need to increase the maximum Permanent Generation space, do this by adding the following to your JVM arguments -XX:MaxPermSize=128m where you replace the number 128 with the new PermGen size in megabytes.
3. Maven2/3 project type, you can set the -Xmx or -XX:MaxPermSize in your Jenkins global configuration.Maven2/3 project type, you can set the -Xmx or -XX:MaxPermSize in your Jenkins global configuration.
Name: MAVEN_OPTS
Value = -Xmx500m -XX:MaxPermSize=500m

Did you automate anything in your project? Please explain

What is input directive?
The input directive on a stage allows you to prompt for input, using the input step.If the input is approved, the stage will then continue. Any parameters provided as part of the input submission will be available in the environment for the rest of the stage.






























 
