## Elastic Beanstalk Using eb cli

### Step 1
$ Install or update the AWS CLI

```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip awscliv2.zip
$ sudo ./aws/install

```
### Step2 
Install the EB CLI
```
$ pip install awsebcli --upgrade --user

```
### Step3 
Configure AWS:
```
$ aws configure 
The next command prompt you have to select some option
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json

```

### Step 3
you can use an existing directory or you can create a dicretory. for create a directory use command bellow 
```
$ mkdir folder-name
$ cd folder-name

```
### Step 4
copy all the source code on your created directory

### Step 5
check your Souce code safely and on this folder initialize git using bellow command

```
$ git init
$ git add .
$ git commet -m "your commit message"

```

### Step 6
now you can run eb initialization 
```
$ eb init

Select a default region
1) us-east-1 : US East (N. Virginia)
2) us-west-1 : US West (N. California)
3) us-west-2 : US West (Oregon)
4) eu-west-1 : Europe (Ireland)
5) eu-central-1 : Europe (Frankfurt)
6) ap-south-1 : Asia Pacific (Mumbai)
7) ap-southeast-1 : Asia Pacific (Singapore)
...
(default is 3): 3

Select an application to use
1) HelloWorldApp
2) NewApp
3) [ Create new Application ]
(default is 3): 3

Enter Application Name
(default is "tmp"):
Application tmp has been created.

It appears you are using PHP. Is this correct?
(y/n): y

Select a platform branch.
1) PHP 7.2 running on 64bit Amazon Linux
2) PHP 7.1 running on 64bit Amazon Linux (Deprecated)
3) PHP 7.0 running on 64bit Amazon Linux (Deprecated)
4) PHP 5.6 running on 64bit Amazon Linux (Deprecated)
5) PHP 5.5 running on 64bit Amazon Linux (Deprecated)
6) PHP 5.4 running on 64bit Amazon Linux (Deprecated)
(default is 1): 1
Do you want to set up SSH for your instances?
(y/n): y

Select a keypair.
1) aws-eb
2) [ Create new KeyPair ]
(default is 2): 1

```
### Step 7
check eb status
Run eb status to see the current status of your environment. When the status is ready, the sample application is available at elasticbeanstalk.com and the environment is ready to be updated.
```
$ eb status

```
### Step 8
check eb health
Use the eb health command to view health information about the instances in your environment and the state of your environment overall. Use the --refresh option to view health in an interactive view that updates every 10 seconds.

```
$ eb health

```
### Step 9
Final stage
now you can deploy your project using 

```
$ eb deploy --staged

```
## For migrate database you have to configure few more things

create a folder on your source code folder .ebextensions and on that folder create one file "01-laravel.config"
add this four line
```
container_commands:
    
    01-config_clear:
        command: "sudo php artisan config:clear"
    02-view_clear:
        command: "sudo php artisan view:clear"
    03-route_cache:
        command: "sudo php artisan route:cache"
    04-view_cache:
        command: "sudo php artisan view:cache"
    05-migrate:
        command: "sudo php artisan migrate"

```
And create ".platfor directory"
on this directory create folder name "nginx" and on nginx folder create another folder configuration and create file there "laravel.conf"
add these lines here
```
location / {
 	try_files $uri $uri/ /index.php?$query_string;
 	gzip_static on;

}

```
and then again run 

```
$ git add .
$ git commit "your commit message"
$ eb deploy --staged

```
## Monitor your application
In this step, you will confirm the deployment of your sample application and monitor the health of your application using the Elastic Beanstalk CLI.

a.   Using the terminal, open your live application in a browser by entering the following command:
```
eb open

```
This page is for the php sample application. If you picked a different platform, your application may look different.

b.  To view the status of the application the Elastic Beanstalk CLI has created for you, enter the following command:
```
eb status

```
![alt text](https://github.com/anjanpaul/elastic-beanstalk/blob/main/output/Screenshot%202022-03-31%20at%202.20.09%20PM.png)

c.  To view the health of the computing resources the Elastic Beanstalk CLI has created for you, enter the following command:
```
eb health

```
![alt text](https://github.com/anjanpaul/elastic-beanstalk/blob/main/output/Screenshot%202022-03-30%20at%204.00.41%20PM.png)

## Terminate your application
In this step, you will terminate your sample application using the Elastic Beanstalk CLI. Terminating resources that are not actively being used reduces costs and is a best practice.
```
eb terminate --all

```

Then type in the name of your application. The Elastic Beanstalk CLI will now delete the running resources and application and output the events to the command line. When the termination is complete the Elastic Beanstalk CLI will output:

```
INFO: The application has been deleted successfully.

```
