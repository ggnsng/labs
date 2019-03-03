# Designing a Highly Available Network with Custom VPC

### VPC Lab – Part 01 of 02

In this lab we are going to design the network for a highly available two tier web application. The web servers will be deployed in two public subnets across two availability zones having Internet connectivity and DB servers will be deployed in two private subnets across two availability zones. The DB servers will use network address translation (NAT) service for accessing Internet.

#### Activity 01 – Creating a VPC
Login to your AWS account and find VPC under Networking & Content Delivery category.  
Click on Your VPCs in the side bar and then click on Create VPC.  
_Did you notice that a VPC (default VPC) was already created? Find out what other resources were automatically created for you in VPC and why._ 
Now you need to give a name to your VPC and select a CIDR notation.
* Name tag: MyVPC
* IPv4 CIDR block: 10.0.0.0/16
* IPv4 CIDR block: No IPv6 CIDR Block
* Tenancy: default
* Click on Yes Create. 
You should now see your VPC created similar to below picture.

Select MyVPC and click on Action drop-down. 
Ensure that Edit DNS Resolution and Edit DNS Hostnames are set to Yes.

#### Activity 02 - Creating Subnets

We would now create one public and one private subnet in both availability zones for the high availability of our resources.
Click on Subnets in the sidebar of the VPC Dashboard, click on Create Subnet

* Name tag: MyPrivateSubnet01
  * VPC: MyVPC
  * Availability Zone: _choose the first one you see_
  * IPv4 CIDR block: 10.0.1.0/24
  
Click on Yes, Create. Your new subnet should have been created now and show up on the screen.  
Repeat the same steps to create 3 more Subnets with below configuration.

* Name tag: MyPrivateSubnet02
  * VPC: MyVPC
  * Availability Zone: _choose the second one you see_
  * IPv4 CIDR block: 10.0.2.0/24
* Name tag: MyPublicSubnet01
  * VPC: MyVPC
  * Availability Zone: _choose the first one you see_
  * IPv4 CIDR block: 10.0.3.0/24
* Name tag: MyPublicSubnet02
  * VPC: MyVPC
  * Availability Zone: _choose the second one you see_
  * IPv4 CIDR block: 10.0.4.0/24
  
Once all the subnets are created, select MyPublicSubnet01 and click on the Subnet Actions dropdown; go to Modify auto-assign IP settings and check Enable auto-assign public IPv4 address box.  
Click on Save. Repeat the same step for MyPublicsubnet02 as well. Your four new subnets should be visible to you in subnet section similar to the below picture.

_Why is the available number of IPs showing as 251, where are the rest 5 IPs used?_  
_Why have we created two private and public in different subnets? Should we not create both Public subnets in one AZ and both Private in another AZ?_

#### Activity 03 - Create Internet gateway
As you might have noticed, there were similar steps taken in creating the Public and Private subnets, what differentiates them?
A public subnet is the one that has a route to Internet Gateway in its routing table. So now let's create an Internet Gateway.
Click on Internet Gateways in the sidebar of VPC Dashboard and then click on Create Internet Gateway.
* Name tag: MyIGW
* Click on Yes, Create.

You will see the state of MyIGW as detached as it is not yet attached to a VPC.  
Select the MyIGW and click on Attach to VPC, select MyVPC from the drop-down in next pop up and click
on Yes, Attach.

_Why was the default VPC not showing in the dropdown._



#### Activity 04 - Create Route table (public) and assign to relevant Subnets

Click on Route tables in the side bar, you should see a Route Table already created for you, assigned to MyVPC.

* Click on Create Route Table
  * Name tag: MyPublicRoute
  * VPC: MyVPC
* Click on Yes, Create.

A new route table would have come up now.

While the MyPublicRoute selected, click on Routes tab in the lower half of the screen. You would see that it already has an entry for local traffic. We now should add the route entry meant for Internet.
Click on Edit and then on Add route. Fill in the below details in the new blank route table entry.

* Destination: 0.0.0.0/0
* Target: Internet Gateway (you would see the Internet gateway name in the drop down)

Click on Save routes.

This way we have added an entry to Internet in our public route table, now is the time to assign the route table to our public subnets.

* Click on the Subnet Associations tab right next to the Routes tab.

_You would see that all four subnets that you created are associated with the main route table, why?_

* Click on Edit subnet associations and select the two Public Subnets that you created. Save.


Let us now create three different 'Security Groups' for bastion hosts, application server, database and load balancer. We would leverage them in coming labs.
In the navigation pane find and click on 'Security Groups'

* Click on 'Create Security Group'
	* Security group name*: My-App-SG
	* Description*: This SG is to be used for application servers.
	* VPC: MyVPC
* Click on Create

Create two more security groups with following configurations --

* Security group name*: My-DB-SG
	* Description*: This SG is to be used for database servers.
	* VPC: MyVPC
* Security group name*: My-ALB-SG
	* Description*: This SG is to be used for application load balancers.
	* VPC: MyVPC
* Security group name*: My-BastionHost-SG
	* Description*: This SG is to be used for bastions hosts.
	* VPC: MyVPC

Select either of the Security Group now and click on 'Inbound Rules' tab.
Click on 'Edit Rules' and add rules for incoming traffic on the security groups like mentioned below.

![](https://github.com/ashydv/aws-labs/blob/master/images/sg1.png)

These rules are not perfect but will suffice our requirement as of now. We will be changing them in sometime. In ideal scenario these rules should look like the picture below. Can you identify the difference?

![](https://github.com/ashydv/aws-labs/blob/master/images/sg2.png)

For now, our VPC configuration is complete. The instances launched in our public subnets should have access to Internet and the instances in our private subnet should not. We would verify the same in the next section.

VPC Lab -- Part 02 of 02
-----------------------

#### Activity 05 - Creating EC2 instances

Let us switch to EC2 Dashboard now and click on Launch Instance.

* Amazon Machine Image: "Microsoft Windows Server 2016 Base" (free tier eligible)
* Instance Type: t2.micro
* Configure Instance Details: select the below mentioned points and leave everything else as default.
	* Network: MyVPC
	* Subnet: MyPublicSubnet01
* Add Storage: Leave defaults (Your instance will come with a root volume of 30 GB as you can see in this screen. We can add additional EBS volumes if need be)
* Add Tags
	* Key: Name
	* Value: MyAppServer
* Configure Security Group: Select existing -> My-App-SG
* Click on Review and Launch.

On the next page ensure that your AMI is free tier eligible and Instance Type is showing as t2.micro.

* Click on Launch.

On the next window, create a new Key Pair, Key pair name: mykey and then Download Key pair.

Finally click on Launch Instance

We have just created an EC2 instance in our public subnet, now we would create an EC2 instance in private subnet following similar steps.
Go back to EC2 Dashboard now and click on Launch Instance.

* Amazon Machine Image: "Microsoft Windows Server 2016 Base" (free tier eligible)
* Instance Type: t2.micro
* Configure Instance Details: select the below mentioned points and leave everything else as default.
	* Network: MyVPC
	* Subnet: MyPrivateSubnet01
* Add Storage: Leave defaults (Your instance will come with a root volume of 30 GB as you can see in this screen. We can add additional EBS volumes if need be)
* Add Tags
	* Key: Name
	* Value: MyDBServer
* Configure Security Group: Select existing -> My-DB-SG
* Click on Review and Launch.

On the next page check that your AMI is free tier eligible and Instance Type is showing as t2.micro.

* Click on Launch.

On the next window, select to use existing Key Pair 'mykey'.

* Click on Launch Instance

Go back to your EC2 instance page. You should see your two instances.

_Did you notice that your MyAppServer has got a public IP and public DNS while MyDBServer has not, why?_
_Why are both are running in the same AZ?_

#### Activity 06 - Verifying the connectivity

We now have created two EC2 instances one in each public and private subnet. We would now verify whether our network configuration is working as desired.

Let us RDP to the MyAppServer.

* Select the MyAppServer in the dashboard and click on 'Connect'
* You now have to mention the path of your 'key pair' and decrypt the windows password.
* Get the login credentials and login to the instance.

Once you are logged into your MyAppServer EC2 instance, you may verify if the machine can reach Internet.

In the above step you connected to your EC2 instance in public subnet and verified that it had Internet connectivity. Our next step would be login to the EC2 instance launched in the private subnet and verify that it should not have Internet connectivity.
We will use our public EC2 instance as a bastion host (jump box) to login to the instance in the private subnet. In ideal scenario we should have created another EC2 instance as bastion host, we are using the 'MyAppServer' as jump server just to minimize cost and to be in free tier limits as long as possible in the lab exercise.

* Retrieve the password for the private instance using the AWS management console and initiate an RDP session from within the public instance.

Let us quickly try to check to see if this EC2 instance can reach Internet. The easiest way is by pinging google.com
In all possibilities, the connection should not work. This proves that your MyDBServer in MyPrivatesubnet01 does not have direct access to Internet. Keep the session opened. In the next steps, we would enable NATing service for private subnets to give one-way Internet access.

* Go back to your VPC dashboard.
* Click on NAT Gateways in the sidebar of VPC Dashboard and then click on Create NAT Gateway.
* Select the following configurations on the next page.
 * Subnet: MyPublicSubnet01 (select from the dropdown)
 * Elastic IP Allocation ID: Create New EIP
* Click on Create a NAT Gateway.

_Why have you created this NAT Gateway in a public subnet?_
_Why did you select MyPublicSubnet01 and not MyPublicSubnet02?_

Now as your NAT Gateway has been created, we will add this in the private route table.

* Go to Route Tables, create a new route table "MyPrivateRoute" and assign it to the two Private Subnets (Follow the similar process as you did in activity 3.  
As expected, you would see that it has an entry for local traffic. We now should add the route entry meant for Internet.
* Click on Edit and then on Add Another Route. Fill in the below details in the new blank route table entry.
 * Destination: 0.0.0.0/0
 * Target: NAT Gateway (Select the one you created)
* Click on save.

So, you have now created a NAT Gateway which is a managed service by Amazon and assigned it to the route table which is assigned to your private subnets. This way the EC2 instances created in private subnets would get the outbound access to Internet for downloading updates/patches etc.
Let us verify the same by going back and accessing Internet from the instance in private subnet. It should work now if you have followed the steps carefully.

#### Activity 07 - Clean up

Let's clean up. Follow the order or you will get dependency errors.

* Terminate both the instances.
* Delete NAT Gateway (deleting NAT Gateway might take a minute or two, keep refreshing the page till the time you see it is deleted)
* Release the Elastic IP that was created for your NAT Gateway (it will not be released until the NAT gateway is deleted).

***The NAT Gateway is a chargeable resource and typically it is ~USD 0.05/INR 5 to 6 per hour, ensure you delete it within an hour of creation. You would need to pay this amount once AWS sends you the bill at the end of month. Elastic IPs are chargeable resources if they are lying unused, there will be no fee as long as you delete it post NAT Gateway deletion.***

Leave the other resources. They are free and we will leverage them in subsequent labs.

**Lab Complete.**
