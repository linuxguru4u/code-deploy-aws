Instructions
============
1.) When server is booted run the following commands as root. 
Update Install Validate

#Connect to instance using ssh 
 Authenticating with public key "imported-openssh-key" 
     SSH session to ec2-user@XXX.XXX.XXX.XXX                     
 
        __|  __|_  ) 
        _|  (     /   Amazon Linux AMI 
       ___|\___|___| 
 
 https://aws.amazon.com/amazon-linux-ami/2016.09-release-notes/ 
 5 package(s) needed for security, out of 9 available 
 Run "sudo yum update" to apply all updates. 
#SuperUser 
 [ec2-user@ip-172-31-2-192 ~]$ sudo su 
#Yum update system make it current
 [root@ip-172-31-2-192 ec2-user]# yum update -y 
 Loaded plugins: priorities, update-motd, upgrade-helper 
 Resolving Dependencies 
 --> Running transaction check 
 ---> ...
 --> Finished Dependency Resolution 
 
 Dependencies Resolved 
 
 ================================================================================ 
  Package            Arch       Version                   Repository        Size 
 ================================================================================ 
 Installing: 
  kernel             x86_64     4.4.39-34.54.amzn1        amzn-updates      16 M 
 Updating: 
  kernel-tools       x86_64     4.4.39-34.54.amzn1        amzn-updates      99 k 
  ntp                x86_64     4.2.6p5-43.33.amzn1       amzn-updates     658 k 
  ntpdate            x86_64     4.2.6p5-43.33.amzn1       amzn-updates      91 k 
  sudo               x86_64     1.8.6p3-25.23.amzn1       amzn-updates     929 k 
  vim-common         x86_64     2:8.0.0134-1.44.amzn1     amzn-updates     9.0 M 
  vim-enhanced       x86_64     2:8.0.0134-1.44.amzn1     amzn-updates     1.2 M 
  vim-filesystem     x86_64     2:8.0.0134-1.44.amzn1     amzn-updates      12 k 
  vim-minimal        x86_64     2:8.0.0134-1.44.amzn1     amzn-updates     495 k 
 
 Transaction Summary 
 ================================================================================ 
 Install  1 Package 
 Upgrade  8 Packages 
 
 Total download size: 28 M 
 Downloading packages: 
...
 -------------------------------------------------------------------------------- 
 Total                                               55 MB/s |  28 MB  00:00 
 Running transaction check 
 Running transaction test 
 Transaction test succeeded 
 Running transaction 
...
 
 Installed: 
   kernel.x86_64 0:4.4.39-34.54.amzn1 
 
 Updated: 
   kernel-tools.x86_64 0:4.4.39-34.54.amzn1 
   ntp.x86_64 0:4.2.6p5-43.33.amzn1 
   ntpdate.x86_64 0:4.2.6p5-43.33.amzn1 
   sudo.x86_64 0:1.8.6p3-25.23.amzn1 
   vim-common.x86_64 2:8.0.0134-1.44.amzn1 
   vim-enhanced.x86_64 2:8.0.0134-1.44.amzn1 
   vim-filesystem.x86_64 2:8.0.0134-1.44.amzn1 
   vim-minimal.x86_64 2:8.0.0134-1.44.amzn1 
   
#Check yum install -y aws-cli is installed (installed with aws linux)
 [root@ip-172-31-2-192 ec2-user]# yum install -y aws-cli
 Up to date nothing to do 
 
#Check current directory ensure /home/ec2-user
 [root@ip-172-31-2-192 ~]# pwd 
 /root 
 [root@ip-172-31-2-192 ~]# cd /home/ec2-user/ 
 [root@ip-172-31-2-192 ec2-user]# pwd 
 /home/ec2-user

#AWS-configure 
 [root@ip-172-31-2-192 ec2-user]# aws configure 
 AWS Access Key ID [****************PB5Q]: 
 AWS Secret Access Key [****************SlQi]: 
 Default region name [None]: us-east-1 
 Default output format [None]: json 

#Get the ./install for code deploy agent
 [root@ip-172-31-2-192 ec2-user]# aws s3 cp s3://aws-codedeploy-us-east-1/latest/install . --region us-east-1 
 download: s3://aws-codedeploy-us-east-1/latest/install to ./install 

#Add execute permission 
 [root@ip-172-31-2-192 ec2-user]# chmod +x ./install 

#Sed to speed up agent startup
 [root@ip-172-31-2-192 ec2-user]# sed -i "s/sleep(.*)/sleep(10)/" install 

#Install using defaults
 [root@ip-172-31-2-192 ec2-user]# ./install auto 
 I, [2017-01-06T19:57:15.363792 #28552]  INFO -- : Starting Ruby version check. 
 I, [2017-01-06T19:57:15.363981 #28552]  INFO -- : Starting update check. 
 I, [2017-01-06T19:57:15.364059 #28552]  INFO -- : Attempting to automatically detect supported package manager type for system... 
 I, [2017-01-06T19:57:15.370259 #28552]  INFO -- : Checking AWS_REGION environment variable for region information... 
 I, [2017-01-06T19:57:15.370311 #28552]  INFO -- : Checking EC2 metadata service for region information... 
 I, [2017-01-06T19:57:15.417410 #28552]  INFO -- : Downloading version file from bucket aws-codedeploy-us-west-2 and key latest/VERSION... 
 I, [2017-01-06T19:57:15.462492 #28552]  INFO -- : Downloading version file from bucket aws-codedeploy-us-west-2 and key latest/VERSION... 
 I, [2017-01-06T19:57:15.540355 #28552]  INFO -- : Downloading package from bucket aws-codedeploy-us-west-2 and key releases/codedeploy-agent-1.0-1.1067.noarch.rpm... 
 I, [2017-01-06T19:57:15.597423 #28552]  INFO -- : Executing `/usr/bin/yum -y localinstall /tmp/codedeploy-agent-1.0-1.1067.noarch.tmp-20170106-28552-1ouob6q.rpm`... 
 Loaded plugins: priorities, update-motd, upgrade-helper 
 Examining /tmp/codedeploy-agent-1.0-1.1067.noarch.tmp-20170106-28552-1ouob6q.rpm: codedeploy-agent-1.0-1.1067.noarch 
 Marking /tmp/codedeploy-agent-1.0-1.1067.noarch.tmp-20170106-28552-1ouob6q.rpm to be installed 
 Resolving Dependencies 
 amzn-main/latest                                         | 2.1 kB     00:00 
 amzn-updates/latest                                      | 2.3 kB     00:00 
 --> Running transaction check 
 ---> Package codedeploy-agent.noarch 0:1.0-1.1067 will be installed 
 --> Finished Dependency Resolution 
 
 Dependencies Resolved 
 
 ================================================================================ 
  Package          Arch   Version    Repository                             Size 
 ================================================================================ 
 Installing: 
  codedeploy-agent  noarch 1.0-1.1067 /codedeploy-agent-1.0-1.1067.noarch.tmp-20170106-28552-1ouob6q 
                                                                            11 M 
 
 Transaction Summary 
 ================================================================================ 
 Install  1 Package 
 
 Total size: 11 M 
 Installed size: 11 M 
 Downloading packages: 
 Running transaction check 
 Running transaction test 
 Transaction test succeeded 
 Running transaction 
   Installing : codedeploy-agent-1.0-1.1067.noarch                           1/1 
 Installing codedeploy-agent auto-update cron in '/etc/cron.d/codedeploy-agent-update'... 
 Installing codedeploy-agent auto-update cron in '/etc/cron.d/codedeploy-agent-update'...Complete 
   Verifying  : codedeploy-agent-1.0-1.1067.noarch                           1/1 
 
 Installed: 
   codedeploy-agent.noarch 0:1.0-1.1067 
 
 Complete! 
 I, [2017-01-06T19:57:19.482028 #28552]  INFO -- : Update check complete. 
 I, [2017-01-06T19:57:19.482110 #28552]  INFO -- : Stopping updater. 

#Check service is up 
 [root@ip-172-31-2-192 ec2-user]# service codedeploy-agent status 
 The AWS CodeDeploy agent is running as PID 28600 
 
# Logout

