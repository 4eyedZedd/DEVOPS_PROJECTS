
# IMPLEMENT A WEB SOLUTION BASED ON MERN STACK IN THE CLOUD 

In this project, we are tasked to implement a web solution based on MERN stack in AWS Cloud.
MERN Web stack consists of following components:

* MongoDB - MongoDB is a popular, open-source, NoSQL (Not Only SQL) document database. This basically means that MongoDB uses a flexible document model. Data is stored in documents, which are similar to JSON (JavaScript Object Notation) objects. Data is stored in documents, which are similar to JSON (JavaScript Object Notation) objects.

* ExpressJS - ExpressJS often simply called Express, is a popular and flexible Node.js server-side web application framework.

* ReactJS - ReactJS often simply referred to as React, is a very popular, open-source JavaScript library used for building user interfaces (UIs). 

* Node.js - Node.js is a powerful and versatile runtime environment that allows developers to execute JavaScript code on a machine outside of a web browser.

### Prerequisites...
As a prerequisite to the course; 

* As usual, we will create an EC2 instance in the cloud, (refer to Project 1 for a guide with setting one up). Ensure to set a tag 'name' for the instance to be able to differentiate each one.

* For Windows users, we will be using Windows terminal to carry out the task in place of Git bash as we have used in previous tasks. Lets follow the following steps to set up.
    * You would already expectedly have windows terminal installed in your system. If you don't, download the windows terminal from the windows store and follow the installation prompts or use this [URL](https://learn.microsoft.com/en-us/windows/terminal/install)
    * You would also expectedly have Vscode installed in your system. If you don't, download VsCode using this [URL](https://code.visualstudio.com/download) select the Windows option and follow the prompts install correctly. Also install vscode extension like (material icon theme, Git extension pack and Bracket pair colorizer )
* We will then install the Open SSH server by running a series of commands. 
    - At the time of scripting this file and starting with Windows Server 2025, OpenSSH is now installed by default. But it is not installed by default for us so we need to install it with this command: `Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*' ` This returned the result for me below; ![alt text](<Images/package status for  open ssh.png>). As seen the SSH.Seerver is not installed for me.
    - Run the command below to install Open SSH server `Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0`, then run `Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*' ` to confirm that it works as expected.
    - Run `ssh-keygen -t ecdsa` to generate a keypair. you will be prompted with the destination path of the key pair. For me it is `(C:\Users\DELL/.ssh/id_ecdsa):`








 