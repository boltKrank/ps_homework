Intermediate Three: MCO Agents and Applications
A client has a build server that compiles their code automatically.  They wish to have the compiled code installed and running on all their test application servers.  Their requirement is that all servers must be updated at the same time, therefore MCollective will be used to do this task.
The build server is called Builder_5000 (runs on Linux) and has an API Client for both Linux and Windows, which is called builder_5000_client.  The command to update the code base via the client is as follows:
builder_5000_client <HOSTNAME_OF_BUILD_SERVER> <CODE_BASE_NAME> <CODE_VERSION> <LOCAL_DESTINATION_DIRECTORY>
CODE_BASE_NAME is the name of the software code (choose your own software name).
CODE_VERSION can be a number for a specific version, or `latest` for the latest compiled version.
The application on the servers is called app_6000 (package and service name) and this application installation and configuration is managed by Puppet via a class called app_6000.  The code base directory is /opt/app_6000/code_base or c:\Program Files\App_6000\Code Base.  The application must be stopped whenever the code base is modified.
Tasking
Write a MCO agent that will update the code base that uses the client API above.
Write a MCO application that can trigger the MCO agent.
The MCO agent and application must work on both Linux and Windows.
The MCO code on the build server must only be allowed to publish this code and no other.
Use Puppet where possible to perform the work.
Think about what needs to be done with this MCO agent: you will need to do more than just update the code base to satisfy the requirements!
As well as the coding, answer the following:
What is the difference between a MCO Agent and a MCO Application?
What MCO code would be on the build server; the Agent or the Application, or both?
How do you restrict which server will run the MCO agent?
How would you install the MCO agent/application?
What methods are there to install MCO agents and applications?
How would you restrict the MCO publisher/client on the build server from running any other MCO agents or applications?
Why would we not get Puppet 'runs' to do this task for us?
Remember, you are explaining these questions to a customer.
