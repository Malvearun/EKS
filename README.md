# EKS

Amazon EKS:

•	It is certified kubernetes conformant with k8s plugin.
•	Provision and scale your cluster
•	Self-healing

Serverless option:

Yes, with AWS with high agenda. 

#### Running containers with tradition way.

•	Manage cluster with EC2 instance with control panel and nodes. Manage nodes groups with latest EKS-optimized Linux AMI.
•	Automated process of lifecycle management system.
•	Also, stateful application or long running application like web server.

#### Serverless option with Fargate.
Fargate is also used with ECS (Elastic container service).
Fargate can be run with ECS and EKS.

•	Run applications as serverless containers.
•	No need to provision or manage servers, need to specify and pay for services/resources per application.
•	It is helpful for short-lived stateless process. Application need to run the jobs. 
** Note **: This is not helpful for application requires persistence volumes or file system.

#### Terminologies:

•	**Deployment**: Create instance of the container, will monitor and restart the instance.
•	**Image**: Docker image or Windows image.

•	**Image repositories**: ECR, Docker Hub etc.,

•	**Pod**:  one or more containers. Container run in pods, pods run on nodes. They share IP spaces.

•	**Service**: Expose the service external can be Elastic load balancer.


##### Tools need to know before getting started:

•	**eksctl**: tool to create, delete, launch the control plane and build worker nodes group them together has managed group has pod of the cluster. 
•	**kubectl**: kubernetes command line utility, used to control kubernetes clusters. 

EKS is a certified kubernetes conformant environment, you can compute EC2 for stateful applications with Fargate for serverless. Tools to work for EKS are `eksctl and kubectl`.

#### EKS is integrated with below:

•	**CloudTrail**: logging of all users and API activity.

•	**CloudWatch**: Control panel logs and Monitoring.

•	**IAM**: Authentication, authorization, permissions.

•	**VPN**: Network isolation, security groups, ACLs.

•	**Auto Scaling**: Scale the infrastructure as required.

•	**ELB**:  to expose your application over internet.

#### EKS control plane:

•	Scalability:  It is supper scalable, it is run across three AZ’s. There is no single point failure.

•	Security: Automatically the latest patches are updated on go.

•	Single Tenant: It is not shared with any other EKS cluster.

It is quick and easy to get up and running with multi-node, multi-AZ, high available Kubernetes cluster in a single command. It operates and maintains the k8s control plane.

#### Kubernetes:

•	It supports declarative model, we create configuration files written in yaml format. 
•	To create the / apply follow below command line.
Example commands: 
1.	kubectl create -f deployment.yaml
2.	Kubectl apply -f deployment.yaml


#### Prerequisites:

•	AWS management console:  AWS free-tier account would work.
•	EC2 instance: using has admin work environment.
•	EKS

### Steps will be following for EKS build:

•	IAM user with admin permissions 

•	EC2 Instance configuration with command line tools 

•	Provision of EKS Cluster

•	Create Deployment on your EKS Cluster

•	Test the HA of EKS cluster.

##### Create an IAM User with Admin Permissions

AWS Console &rarr; Services &rarr; IAM &rarr; Users &rarr; Add User &rarr; EKS_user with Programmatic access &rarr; Administrator Access &rarr; Review &rarr; Create
1.	Copy and paste some where you can use it later.

##### Launch an EC2 Instance and Configure.

•	Select the correct region. 

•	AWS Console &rarr; Services &rarr; EC2 &rarr; Launch Instance > 
1.	Choose AMI ‘Linux’
2.	Choose the Instance type as `depending upon requirement` 
3.	Configuration Instance Details:
a.	Auto-assign Public IP: `Enable`
4.	Add Storage: default
5.	Tags: Default
6.	Review and launch.
7.	Create a new key pair – `my-keypair` and `Download key pair`
8.	It will take few minutes to initialize the instance.
•	Select the instance and click on connect and select connection method has `EC2 instance connect` and click `connect`.
•	To run all admin commands in this CLI.


#### AWS command line configuration:
1.	run  `aws --version`

If running older version please upgrade to next version of interface.

2.	run `sudo curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o “awscliv2.zip”`
3.	run `unzip awscliv2.zip`
4.	run `which aws` to check where aws location.
5.	run ` sudo ./aws/install –bin-dir /usr/bin –install-dir /usr/bin/aws-cli --update`
6.	run `aws --version`

**Reference**: https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html

•	Steps to configure aws with access key and secret access key:

run `aws configure`
‘access key` it will ask to enter the key value.
`secret access key` it will ask to enter the key value.
region: `depending upon requirement` region us-east-1
output format: json

##### download the binary of kubectl: check the version for kunernetes.
Reference page: https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

•	Run the below commands to excute the binary and Copy the binary to a folder in your PATH

1.	run `curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.17.9/2020-08-04/bin/linux/amd64/kubectl`
2.	run `chmod +x ./kubectl` add execute permission to binary.
3.	run ` mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin` 
4.	run ` kubectl version --short –client` verify its version

##### Download and install ekstcl:

•	Run the below commands to excute the binary and Copy the binary to a folder in your PATH

1.	run ` curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp`
2.	run ` sudo mv /tmp/eksctl /usr/local/bin` Move the extracted binary to /usr/local/bin
3.	run ` eksctl version` check the version and this command works.
4.	run `eksctl help`

##### Create your Amazon EKS cluster and compute
run `eksctl create cluster --name dev-cluster --version 1.17 --region us-east-1 --nodegroup-name standard-workers --node-type t3.micro --nodes 3 --nodes-min 1 --nodes-max 4 –managed`

•	check the above command and can modify depending upon the requirement.
1.	Should monitor the CloudFormation &rarr; Stacks &rarr; to see all the Event timestamp getting ready and look for status. At times you will see an error coming up. So don’t worry nothing wrong has done from your side. It is the infrastructure capacity. It will revert back and rollback and can create new with same command again.

Note: Duration of cluster creation might take more than 10 minutes.

2.	Services &rarr; Containers &rarr; EKS &rarr; Cluster will be created.

a.	Compute: find all the nodes created, select the group name kubernetes version 1.17, Instance type: T3.micro, status shows as active.
b.	Networking: Should see the VPC, Subnets, Security groups, networking is created.
c.	Logging: Manage the Control plane using kubectl.

•	Move to EC2 instance, should see four instance running dev-cluster instance names.

•	Connect back to admin instance and follow below commands.

1.	run `eksctl get cluster` get the information about the cluster.

2.	run `aws eks update-kubeconfig –name dev-cluster –region us-east-1`

##### Install our application:

1.	run `sudo yum install -y git` to install git

2.	run `git clone http://

3.	cd “folder”

4.	run `ls` to view the files.


•	In order see environment variable which will be running container will started, we create the service first.

1.	run `kubectl apply -f ./nginx-svc.yaml`

2.	run `kubectl apply -f ./deployment.yaml`

3.	run `kubectl get service` should see the AWS LoadBalancer will be create and should see DNS of LoadBalancer.

4.	To access the application through loadbalncer DNS, copy and paste in browser.

5.	run `curl "aa4a691577643433bbe713ff71d5797a-85005419.us-east-1.elb.amazonaws.com"`

#### Exploring High Availability of EKS cluster:

•	Move to EC2 instance, should see three or four instance running dev-cluster instance names. Select and `stop` them all.

•	Connect back to admin instance and follow below commands, to see the change in environment.

•	`kubectl get nodes` Firstly, should see the `nodes` are `NotReady` status has the instance were stopped.

•	`kubectl get pods` should be in `termination` status

•	Wait for 2-3 minutes and run back the commands `kubectl get nodes` and `kubectl get pods` should see they are up and running.

To check the environment is back:

1.	run `kubectl get service` grab the name of the loadbalance domain name and paste in browser to check if it works.

2.	Also, can check through run `curl "aa4a691577643433bbe713ff71d5797a-85005419.us-east-1.elb.amazonaws.com"`


This concludes that the EKS cluster infrastructure is High Available.

**Please excuse me for any typo errors.**
