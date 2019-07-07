
# How to Configure a Jupyter Datascience Notebook Server on Amazon Web Services using Docker
 - Detailed outline and explanation of every step
  - Who does what, when, where and why

## Short Summary of Contents:
1. Use git bash to create SSH keys on local machine
2. Set up an EC2 machine on Amazon Web Services
3. Install and configure Docker on the EC2 machine
4. Install Jupyter notebook
5. Diagram of system
6. Pro forma budget to run this system for 3 months

***

## Expanded Summary of Contents:
### 1. Create SSH keys on local machine
 - Purpose: `ssh` (**s**ecure **sh**ell) will allow you to use your local machine to securely connect to other machines via the Web.
 - `ssh` key pair: `id_rsa` (a private key) and `id_rsa.pub` (a public key)

### 2. Set up a virtual machine on AWS (Amazon Web Services)
 - What it is: An AWS EC2 machine is a "virtual machine" in the cloud that you can connect to via the Internet and use via git bash in a browser on your local machine.
  - EC2 (i.e., ECC, stands for "elastic computing cloud")
 - Purpose: The cloud machine will allow you to:
  - Configure and use an operating system (e.g., Linux) that differs from your local machine
  - Access hardware that may perform better than and exceed what you have on your local machine
  - Install and use software without having to load it on your own machine
  - Keep a jupyter notebook running full-time on the VM even when you shut down your local machine
  - Configure security for the EC2 machine

### 3. Install Docker 
 - What it is: Docker provides "containers" that contain preconfigured operating systems and software. It is considered an improvement on older implementations of virtual machines. Docker allows you to run a trimmed down version of any OS in a "container" on a box of any type (Linux, MacOS, Windows). Using Docker you can do this locally, on your own machine, or on a cloud machine such as an EC2 instance on AWS.
 - Purpose: Docker will allow you to build applications on any system (hardware & OS) and have those applications run exactly the same way on any other system that runs Docker.

#### Docker Installation
#### Obtaining the correct Docker image
#### Running the correct Docker image as a container

### 4. Jupyter notebook 
#### Jupyter security concerns

### 5. Diagram: The Docker Client-Host Setup

### 6. Budget
 - Detailed budget of the costs of running a Jupyter Data Science Notebook Server for three months using different kinds of EC 2 instances.
 
***

## Step-by-step detailed instructions

### Begin on your local machine (Windows, MacOS, Linux):
1. Launch Git Bash (I use MinGW-W64 on a Windows 10 machine)
 - MinGW stands for **Min**imalist **G**NU for **W**indows.

### Next steps in git bash:

1. At the prompt, enter `pwd` to print the working directory:
```
<your_name>@<your_machine> MINGW64 ~
$ pwd
/c/Users/your_name
```

2. Enter `ls ~/.ssh` to check if an `.ssh` dir exists and, if it does, list its files:
```
<your_name>@<your_machine> MINGW64 ~
$ ls ~/.ssh
ls: cannot access '/c/Users/your_name/.ssh': No such file or directory
```  
 - The output above indicates that .ssh does not exist.
 - **IMPORTANT: If .ssh already exists, DO NOT create a new one** as this can cause problems.
  - If `.ssh` already exists, use one of the key pairs that already exist in it. 

3. If the .ssh directory does not already exist, create it:
```
<your_name>@<your_machine> MINGW64 ~
$ mkdir -p .ssh
```
 - `-p` stands for "parents," telling `mkdir` to create a directory and any parent directories that don't already exist

4. Enter `ssh-keygen` to generate a public/private rsa key pair:
```
<your_name>@<your_machine> MINGW64 ~
$ ssh-keygen
```
 - Press `Enter` to accept the default file in which to save the key
 - Press `Enter` to use no passphrase
 - Press `Enter` to confirm no passphrase
```
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/your_name/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/your_name/.ssh/id_rsa.
Your public key has been saved in /c/Users/your_name/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:bbxQVVPUObbspq3a5JsaZ5tOd6/JboKuA5OHVgUiDTY karls@DESKTOP-VAQRJL8
The key's randomart image is:
+---[RSA 2048]----+
|    Eo. ..  ..oo=|
|   . o..  ..   =.|
|         ..   o o|
|        .+     o |
|       +S +   .  |
|      * .o .   o |
|     . +  .o =+..|
|        . . Xo*oo|
|        .+.o+&B..|
+----[SHA256]-----+
```

5. Enter `ls -a` to list (a)ll files and folders in the home dir:
```
<your_name>@<your_machine> MINGW64 ~
$ ls -a
 .anaconda/
 .aws/
 .bash_history
 .conda/
 .ipython/
 .jupyter/
 .matplotlib/
 .ssh/          # we're confirming that **this** folder was created
 Desktop/
 Documents/
 github/
'My Documents'@
 R/
 xgboost_install_dir/
```

6. Enter `ls ~/.ssh` to list the files in the `.ssh` dir:
```
<your_name>@<your_machine> MINGW64 ~
$ ls ~/.ssh
id_rsa  id_rsa.pub          # we're confirming that these two files created
```

7. Enter `cat ~/.ssh/id_rsa.pub` to print the public key to screen:
```
<your_name>@<your_machine> MINGW64 ~
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC1SRG7l18hVaNRItzur0uIv9qdtpztHLa+b8vh/ESAEi2YJpCc26Ob3de1vH4/H0KTImE0fgDoRGHBH11L
dsubSi1ocABqdy0QxA1KFFRIOqa0G3n4ipapTzEyGj580zNz8avJuBgRKL+xWWrAXKQPbXwxZuUoOjW4pi/VhK3FvTC5WTKEq4OQ+6p6w6Xea4yys68BowRP
7aIncRtxodKS78f11EVHF2sjF7Aec9zSkHsiTBGsxt8m+M7g2xwSQMR3JyPmpfivZ24Xqy3STQOtd4w0vu24PNYWpLgn3Mx0OrCe/LIU8AKTk/BHzF4uPnF6
9jfyYACROuONnia7CUgj your_name@DESKTOP-VAQRJL8
```

8. Enter `cp ~/.ssh/id_rsa.pub ~/Desktop/` to make a copy of the key file and save it to the Desktop
 - Syntax: `<command> <home/path/filename_to_copy> <location_to_copy_to>`
```
<your_name>@<your_machine> MINGW64 ~
$ cp ~/.ssh/id_rsa.pub ~/Desktop/
```

9. Enter `ls ~/Desktop/` to check that the file was copied to the Desktop:  
```
<your_name>@<your_machine> MINGW64 ~
$ ls ~/Desktop/
/c/Users/your_name/Desktop/:
'!Journaling - Shortcut.lnk'*   desktop.ini   'Microsoft Edge.lnk'*  'Visual Studio 2015.lnk'*
 Calendar.lnk*                  id_rsa.pub     OneDrive.lnk*
'chapter 3 - Shortcut.lnk'*     Kindle.lnk*   'Steps Recorder.lnk'*
'Chapters - Shortcut.lnk'*      Magnify.lnk*   Store.lnk*
```
 - We can see that the `id_rsa.pub` file was copied as expected.

10. Select the text of id_rsa.pub from the output of `cat ~/.ssh/id_rsa.pub` (completed a few steps back) and copy it to the clipboard:
 - Be sure to select the entire output of the `cat ~/.ssh/id_rsa.pub` command above
  - Selection must include `"ssh-rsa "` with space at beginning and `" <your_name>@<your_machine>"` at end

## Set up the ssh keys on GitHub
### These steps are done on github.com:

1. Log in to your github.com account.
2. Press the down arrow on your user icon and select **Settings**.
3. Select **SSH and GPG keys**.
4. Press the **New SSH Key** button.
5. Paste the contents of the clipboard into the **Key** cell, then press the **Add SSH Key** button.

### These steps are done in git bash on your local machine:

1. Add github to the `.ssh` list of known hosts using the code below:
  - Type `yes <Enter>` to confirm that you want to continue connecting.

```
<your_name>@<your_machine> MINGW64 ~
$ ssh git@github.com
The authenticity of host 'github.com (192.30.253.113)' can't be established.
RSA key fingerprint is SHA256:1RLviKwIGOCspRomTxdCA6E5SY8nThbg6kXUpJWGl7E.
Are you sure you want to continue connecting (yes/no)? y
Please type 'yes' or 'no': yes
Warning: Permanently added 'github.com,192.30.253.113' (RSA) to the list of known hosts.
PTY allocation request failed on channel 0
Hi <name>! You've successfully authenticated, but GitHub c.
Connection to github.com closed.
```

2) Display a list of known hosts for `.ssh`
```
<your_name>@<your_machine> MINGW64 ~
$ less ~/.ssh/known_hosts
```
Ouptuput should include the host, GitHub, that you just added.

## Set up EC2 instance on AWS
### These steps are done on aws.com:

1. Log in to your account on aws.com.
2. Click on the **Services** tab and select **EC2**.
3. Press **Key Pair**, then press **Import Key Pair**.

#### Set up security protocols

4. From the EC2 Dashboard, select **Security Groups**, then press the **Create** button.
- You will configure the security for your machine.
5. Give it a **Name** and a **Description**; e.g., "UCLA_class".  
6. Create "rules" to configure the ports to be used for web traffic on your machine. Press the **Add Rule** button and create a rule for SSH:
 - Col 1: Select **SSH**
 - Port: Type **22**
 - Source: Select **Anywhere**
 - Press the **Create Rule** button
7. Follow the same process (**Add Rule**, configure, **Create Rule**) to specify a port for each of the following types of web traffic that you want to be able to use:
- HTTP: Port: **80**, Source: **Anywhere**
- HTTPS: Port: **443**, Source: **Anywhere**
 - You will run Jupyter on port 443.
   
- PostgreSQL: Port: **5432**, Source: **Anywhere**
- Custom TCP: Port: **27016**, Source: **Anywhere**
 - YOu can run the MongoDB server on 27016.
 - This will be machine-to-machine communication between jupyter and mongo.
 - Tip: This port number is one less than the typical 27017 port used for MongoDB. Setting it to 27016 may help reduce the number of malicious hits you experience on your server.
 - NOTE: Later, in the browser, you can specify what port to use by appending `:port_number` to the end of a URL
  - E.g., `github.com:80`
8. From the EC2 Dashboard, select **Instances** (may be **Running Instances**).
9. Press the **Launch Instance** button.
10. Choose **AMI** (Amazon Machine Image): _Ubuntu 16.04_.
 - A fully-configured server image.
11. Choose **Instance Type** (hardware): _t2.micro_ (1 CPU)
12. Configure **Instance**: _default_
13. Add **Storage** (hard drive): Size: _20_ GiB
14. Add **Tags**: _default_
15. Configure **Security Group**: Press the **Select and Existing** button, then select the checkbox of the security group named above.
16. Press ** Review and Launch**.
17. Review: If everything looks right, press **Launch**.
18. Select **Choose an existing key pair**, then select "id_rsa _xxx_", your id_rsa.
19. Press the **Acknowledge** button.
20. Press **Launch Instance**.
21. Press ** View Instances**.
22. Find the instance you just created and give it a name; e.g., UCLA_DS_class.
23. Select the checkbox at the beginning of the line, then scroll down the page and copy the instance's **public IPv4** address to the clipboard.

## CONNECT AWS MACHINE TO GIT BASH
### These steps are done in git bash:
1. At the prompt, type `ssh ubuntu@`_`<ip>`_, pasting in the IP address that you saved to the clipboard in place of _`<ip>`_:
```
<your_name>@<your_machine> MINGW64 ~
$ ssh ubuntu@12.345.67.890
```

 - Press `<Enter>`, type "yes", and press Enter at the _Are you sure_ prompt.

```
The authenticity of host '18.188.45.167 (18.188.45.167)' can't be established.
ECDSA key fingerprint is SHA256:cBxYWZrVw8vFYxHoPnpSfPCd+iNhJmfu92jwOf7JjO4.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '18.188.45.167' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.4 LTS (GNU/Linux 4.4.0-1061-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.     
```

Notice that we are now at a NEW bash prompt that looks something like **ubuntu@ip-172-31-25-201:~$**

## Install and Configure Docker 
### These steps are done in git bash:
 - **Note: Generally piping straight into `sh` as I do below is bad practice security-wise, especially if you do not know or trust the shell script. In this case, I trust Docker and am willing to use this shortcut, but you should decide for yourself whether you want to do this or find another more secure way to run the Docker script.**

1. Run the Docker shell script to **install the docker engine**:
```
curl -sSL https://get.docker.com | sh
```

 - The contents o `https://get.docker.com` is a shell script
 - The shell script is piped ("`|`") into the `sh`(ell) program so it will be run immediately
  - Before running the `curl` command you can view the contents of `get.docker.com` to confirm that it is a shell script. Do this by simply opening the URL in a browser
  - Its first line, `#!/bin/sh` (hash bang, `#!`, pronounced "shebang"), confirms it is a shell script

2. Add your Ubuntu server on the AWS EC2 machine to the Docker user group
```
sudo usermod -aG docker ubuntu
```

3. Disconnect from ubuntu:   
```
Press "Ctrl-D" or type "exit"
```

4. Reconnect:
```
ssh ubuntu@18.188.45.167
```
 - Disconnecting and reconnecting forces the changes you made to take effect.

5. Launch `tmux`.
 - `tmux` stands for "terminal multiplexer." It is a more stable shell than sh. In general you won't need to use `tmux`, but here you use it because sometimes `sh` dies silently when left sitting too long.
```
tmux
```

6. Have Docker **pull jupyter** image from the Docker Hub.
 - Jupyter staff maintain jupyter images on Docker Hub for each flavor of Docker (Linux, MacOS, etc.).
 - The Jupyter staff also maintain several versions of the Jupyter image. Here you will install the **datascience-notebook image**, a fairly hefty version, but there are others with fewer features (no numpy/pandas/scipy, etc.) and a few that include even more features (tensorflow, pyspark).
```
docker pull jupyter/datascience-notebook
```

7. Run the jupyter/datascience-notebook image to install and configure it:
```
docker run -d \
-p 443:8888 \
-v /home/ubuntu:/home/jovyan \
jupyter/datascience-notebook
```
 - `-d` **d**etached mode 
 - `-p` **p**ort configuration
 - `443:8888` This command attaches port 8888 from the docker container to port 443 on your EC2 host. 
 - `-v` means "mounting a **v**olume".
 - `/home/ubuntu:/home/jovyan` This command attaches `/home/ubuntu` on the EC2 host to `/home/jovyan` in the docker container. 

Additional notes:
 - docker ps means "list docker processes"
 - docker ps -a "list all docker processes", including stopped ones
 - docker stop jovyan XXX # kill
 - ls -la "list in long form"
 - docker logs _containerID_ (first few chars, enough to be unique)
 - docker exec _containerID_ # jupyter nb list

### Next steps in Web browser:

1. To configure the Jupyter page so you can use it:
 - From AWS account, copy the public ip address to clipboard as before. Append `:443` to the end; e.g., `192.88.78.68:443`
 - Paste into **Password or Token** textbox on jupyter page.
2. Test jupyter by creating a new notebook and saving it.
 - It should appear in the jupyter file list.
3. In bash, run `ls` to see the file there also.

### You're ready to use your EC2 machine!

### When you are finished using your EC2 machine, be sure to terminate the instance
If you don't you may run up unnecessary charges on your AWS bill! Even though you set up a free instance, if you set up too many instances and run them for too many hours you would use up the free hours and start to incur charges.
1. In AWS, terminate the machine instance.
 - Select **Instance**, **Actions** button, **Instance State**, **Terminate**.

### Obtaining the correct Docker image

See `http://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook`

#### Selecting an Image
Using one of the **Jupyter Docker Stacks** requires two choices:

1. Which Docker image you wish to use
2. How you wish to start Docker containers from that image

 - jupyter/base-notebook is a small image supporting the options common across all core stacks. It is the basis for all other stacks.
 - jupyter/minimal-notebook adds command line tools useful when working in Jupyter applications.
 - jupyter/r-notebook includes popular packages from the R ecosystem.
 - jupyter/scipy-notebook includes popular packages from the scientific Python. 
 - jupyter/tensorflow-notebook includes popular Python deep learning libraries. 
 - jupyter/datascience-notebook includes libraries for data analysis from the Julia, Python, and R communities.
 - jupyter/pyspark-notebook includes Python support for Apache Spark, optionally on Mesos.
 - jupyter/all-spark-notebook includes Python, R, and Scala support for Apache Spark, optionally on Mesos.

### Running the correct Docker image as a container
 - See above step by step instructions

## Jupyter notebook 

### Jupyter security concerns
TK

## Diagram: My Docker Client-Host Setup
![My Docker Client-Host Setup](./images/docker_system_diagram.jpg)

## Budget

The table below shows the costs of running a Jupyter Data Science Notebook Server for three months using several different kinds of EC2 instances. From this I would look for opportunities to use spot instances. It is much cheaper than on demand instances, although it might require more monitoring and restarts of running operations because it can be interupted if AWS needs the server capacity I am using.

![AWS Cost Analysis](./images/AWS_cost_analysis.JPG)

Further below is am expanded review of pricing for these various types of machines.

Different types of machines are available 

 - General Purpose - Current Generation (or Previous Gen cheaper)
 - Compute Optimized - Current Generation (or Previous Gen)
 - GPU Instances - Current Generation
 - Memory Optimized - Current Generation (or Previous Gen)
 
Each of these categories offers a variety of "sizes" of machines, depending on number/model of CPU, RAM, storage, etc.

We will present Linux pricing for the following machines (US East (Ohio)):

**General Purpose - Current Generation**
- These machines are good for XXX
 - t2.micro	
 - t2.small	
 - m4.large	
 - m5.large	
 - m5d.large	

**Compute Optimized - Current Generation**
- These machines are good for XXX
 - c5.large	
 - i3.metal	
 
**GPU Instances - Current Generation**
- These machines are good for XXX
 - g3.4xlarge	
 - p2.xlarge	
 - p3.16xlarge	
 
**Memory Optimized - Current Generation**
- These machines are good for XXX
 - x1.32xlarge	
 - r4.large	

There are four ways to pay for Amazon EC2 instances: On Demand, Reserved Instances, Spot Instances, and Dedicated Hosts.

##### On-Demand
 - Compute capacity by the hour (some instances are by the second).

 - **Advantages**:
  - No longer-term commitments, no upfront payments.
  - You can increase/decrease your compute capacity depending on the demands of your application.

 - **Good for**
  - Applications with short-term, spiky, or unpredictable workloads that cannot be interrupted.
  - Applications being developed or tested on Amazon EC2 for the first time.
  

On-demand pricing:

**General Purpose - Current Generation** (models t2, m3, m4, m5, m5d)

| Model | USD/hr |
| ---------- | --------: |
| t2.micro | 0.0116 |
| t2.small | 0.023 |
| m4.large | N/A |
| m5.large | 0.096 |
| m5d.large | 0.113 |

**Compute Optimized - Current Generation** (models: c5, c5d, c4, d2, h1, i2, i3)

| Model | USD/hr |
| ---------- | --------: |
| c5.large	| 0.085 |
| i3.metal	| N/A |
 
**GPU Instances - Current Generation** (models: g3, p2, p3)

| Model | USD/hr |
| ---------- | --------: |
| g3.4xlarge  | 1.14 |
| p2.xlarge	  | 0.90 |
| p3.16xlarge |	24.48 |
 
**Memory Optimized - Current Generation** (models: x1, r3, r4)

| Model | USD/hr |
| ---------- | --------: |
| x1.32xlarge |		13.338 |
| r4.large	| 0.133 |

##### Reserved Instances
 - Up to 75% discount.
 - Requires 1 or 3 year term.
 - Provide a capacity reservation, giving you additional confidence in your ability to launch instances when you need them.
 - Good for applications that have steady state or predictable usage.

Reserved instance pricing (based on up front payment for a standard 1-year term):

**General Purpose - Current Generation** (models t2, m3, m4, m5, m5d)


| Model | USD/hr |
| ---------- | --------: |
| t2.micro | 0.0067 |
| t2.small | 0.0135 |
| m4.large | 0.0579 |
| m5.large | 0.0572 |
| m5d.large | 0.0671 |

**Compute Optimized - Current Generation** (models: c5, c5d, c4, d2, h1, i2, i3)


| Model | USD/hr |
| ---------- | --------: |
| c5.large	| 0.0503 |
| i3.metal	| 3.1987 |
 
**GPU Instances - Current Generation** (models: g3, p2, p3)


| Model | USD/hr |
| ---------- | --------: |
| g3.4xlarge	| 0.7261 |
| p2.xlarge	| 0.5733 |
| p3.16xlarge	| 15.5937 |
 
**Memory Optimized - Current Generation** (models: x1, r3, r4)


| Model | USD/hr |
| ---------- | --------: |
| x1.32xlarge		| 7.6711 |
| r4.large	| 0.0782 |

##### Spot Instances
 - Allow you to request spare Amazon EC2 computing capacity for up to 90% off the On-Demand price. 

 - **Good for**
  - Applications w/ flexible start and end times
  - Applications that are only feasible at very low compute prices
  - Urgent computing needs for large amounts of additional capacity.
  
Spot Instances can be interrupted by EC2 with two minutes of notification when EC2 needs the capacity back. You can use Spot Instances for various fault-tolerant and flexible applications, such as big data, containerized workloads, high-performance computing (HPC), stateless web servers, rendering, CI/CD and other test & development workloads. 

Spot Fleet, which automates the management of Spot instances. You simply tell Spot Fleet how much capacity you need and Fleet does the rest.

PAUSE & RESUME
With the new Hibernate and Stop-Start features, Spot will automatically pause and resume your work around interruptions, so your applications can start right where they left off.

Spot instances are also available to run for a predefined duration – in hourly increments up to six hours in length – at a discount of up to 30-50% compared to On-Demand pricing.

Spot instance pricing:

**General Purpose - Current Generation** (models t2, m3, m4, m5, m5d)


| Model | USD/hr |
| ---------- | --------: |
| t2.micro | 0.0035 |
| t2.small | 0.0069 |
| m4.large | 0.0178 |
| m5.large | 0.0202 |
| m5d.large | 0.0186 |

**Compute Optimized - Current Generation** (models: c5, c5d, c4, d2, h1, i2, i3)


| Model | USD/hr |
| ---------- | --------: |
| c5.large	| 0.0194 |
| i3.metal	| 1.4976 |
 
**GPU Instances - Current Generation** (models: g3, p2, p3)


| Model | USD/hr |
| ---------- | --------: |
| g3.4xlarge	| 0.342 |
| p2.xlarge	| 0.27 |
| p3.16xlarge	| 7.344 |
 
**Memory Optimized - Current Generation** (models: x1, r3, r4)


| Model | USD/hr |
| ---------- | --------: |
| x1.32xlarge	| 4.0014	 |
| r4.large	| 0.0186 |

##### Dedicated Hosts
  - Physical servers dedicated for your use.
  - No additional charge for multiple instances.
  - Provisioning is extra.
  - Can help reduce costs by allowing you to use your existing server-bound software licenses
   - Including Windows Server, SQL Server, and SUSE Linux Enterprise Server (subject to your license terms)
   - Can be purchased On-Demand (hourly).
   - Can be purchased as a Reservation for up to 70% off the On-Demand price.
  
Dedicated host pricing:
 - Varies by instance family, region, and payment option. This is, of course, the most expensive option.
 
Dedicated host on-demand pricing:
 - There is no size associated with each model because you are free to run any number of instances at no additional charge

**General Purpose - Current Generation** (models t2, m3, m4, m5, m5d)


| Model | USD/hr |
| ---------- | --------: |
| t2 | N/A |
| m4 | 2.42 |
| m5 | 5.069 |
| m5d | 5.966 |

**Compute Optimized - Current Generation** (models: c5, c5d, c4, d2, h1, i2, i3)


| Model | USD/hr |
| ---------- | --------: |
| c5 |	3.366 |
| i3 |	5.491 | (Storage Optimized - Current Generation)
 
**GPU Instances - Current Generation** (models: g3, p2, p3)


| Model | USD/hr |
| ---------- | --------: |
| g3 |	5.016 |
| p2 |	15.84 |
| p3 |	26.928 |
 
**Memory Optimized - Current Generation** (models: x1, r3, r4)


| Model | USD/hr |
| ---------- | --------: |
| x1 |	14.672	 |
| r4 |	4.682 |

### Related Costs

Like data transfer, Elastic IP addresses, and EBS Optimized Instances, visit the On-Demand pricing page.

Data Transfer IN To Amazon EC2 From Internet
 - All data transfer in	0.00 per GB

Data Transfer OUT From Amazon EC2 To Internet
 - Up to 1 GB / Month	0.00 per GB  
 - Next 9.999 TB / Month	0.09 per GB  
 - Next 40 TB / Month	0.085 per GB  
 - Next 100 TB / Month	0.07 per GB  
 - Greater than 150 TB / Month	0.05 per GB

Elastic IP Addresses
 - You can have one Elastic IP (EIP) address associated with a running instance at no charge. If you associate additional EIPs with that instance, you will be charged $0.005 for each additional EIP associated with that instance per hour on a pro rata basis. 

T2 Unlimited Pricing
 - For T2 Unlimited instances, CPU Credits are charged at 0.05 per vCPU-Hour for Linux.

Additional details:

Instance types comprise varying combinations of CPU, memory, storage, and networking capacity

Each instance type includes one or more instance sizes, allowing you to scale your resources to the requirements of your target workload.

A Dedicated Host is configured to support one instance type at a time. The instance capacity, the number of simultaneous instances a machine can run, host is a function of the host and the size of the instances. For this reason, on each model (t3, m4, etc.) the instance capacity _decreases_ as the host size increases.

Example:
A c3.large Dedicated Host (two sockets and 20 physical cores) can support up to 16 c3.large instances. The c3.**x**large can support up to 8 c3.**x**large instances. The c3.**8x**large can support only 1 c3.**8x**large instances. 
