            ***VPC with public-private Subnet in Production About the project***

This project demonstrates how to create a VPC that you can use for servers in a production environment.

To improve resiliency, you deploy the serevrs in 2 Availability Zones, by using group and Auto scaling group and an Application Load Balancer. For additional Security, I deployed the servers in private Subnets. The Servers receive requests through the load balancer. The servers can Connect to the internet NAT gateway. To improve resiliency, I deployed the NAT gateway in both Availability Zones.


Overview

- The vpc has public subnets and private Subnets in two Availability Zones.

- Each public Subnet contains and a NAT gateway and load balancer node.

- The severs run in the private subnets, are launched and terminated by using an Auto Scaling group, receive traffic from the load balancer.

- The Servers can connect to the internet by wing the NAT gateway.


Note:
1. The purpose of deploying applications in 2 Availability Zones is just for resiliency.

2. NAT gateways comes into handy when the applications need to access the content from external world using internet or API calls.

3. Auto Scaling Group is a concept, using which it creates replicas of actual server when the amount of  load is greater than the planned capacity.

4. Load Balancer, drive the load according to the rules configuration.

5. Bastion Host or Jump Server: As we want to keep our applications Safe, we deploy then in private subnet and donot allocate any public Ip address. To connect to them we create a bastion server through which create we connect to main servers.

Implemetation:
Firstly, Login to AWS console

1. Creation of VPC:
- In the search bar, type VPC and select it.
- Click on create VPC.
- Select VPC and more
- Name the project. Ex: app-deployment
- Select the required IPv4 CIDR block
    For this project going with the default is fine.
- IPv6 CIDR block - Not required
- Select 2 AZ's, Public and Private subnets, NAT gateways - 1 per AZ, VPC endpoints - None

2. Creation of Auto Scaling Groups
- Provide name. Ex: app-deployment
- For creation of Auto Scaling Groups, we need to create a Launch templet.
- I choose to work on ubuntu instance.
- Choose VPC as newly created VPC.

- Select this templet and deploy applications in both private subnets with desired, min. and max capacities.


3. Installing applications in instances
- First we need to create an ec2 instance, acting as bastion host in public subnet of VPC.
- Copy the key-pair of the servers to the bastion host using scp command.
- ssh to the  bastion host using .pem file
- Then ssh to both the servers using their private IP's.
- Create a simple index.html file
- Run the following command to start the server
        python3 -m http.server 8000

4. Creation of Load Balancer
- Create an application load balancer.
- configure the security group of load balancer to allow ssh traffic, http traffic and also to allow traffic on port 8000.
- We also need to create Listeners and routing. For that we need to create the target group.
- Hence create a target group for the type Instances to allow HTTP traffic on port 8000 in the newly     created VPC.
- Add the target group and final create Load balancer.

Copy the DNS name generated and browse it to view the results of the webpage.