# Fortigate-Firewall-AWS-Deployment
<h2>Description</h2>
<br/> In this demo we will deploy and configure a Fortigate Firewall in AWS
<br />
<br/> <br/>
<img src="https://github.com/user-attachments/assets/267108dd-2024-462c-82a4-199ad1d36387"/>


<h2>Languages and Utilities Used</h2>

AWS Resources, Fortigate Firewall

<h2>Environments Used </h2>

- <b> AWS </b>

<h2>Project walk-through:</h2>
<br/>
<p align="center">


### **Prerequisites**  
- Have an [AWS account](https://aws.amazon.com/console/).   
- Acess to a managerial account to access the [AWS Marketplace for the Fortinet FortiGate Next-Generation Firewall](https://aws.amazon.com/marketplace/pp/prodview-wory773oau6wq?ref_=beagle&applicationId=AWSMPContessa#pdp-reviews).

## Step 1: Creating the network
<br/> Navigate to the AWS console and create a VPC. <br/>
<img src="https://github.com/user-attachments/assets/e3b9eb11-1e79-47b7-8674-828ae16faeeb"/>
<br/> Next create an Internet Gateway <br/>
<img src="https://github.com/user-attachments/assets/6de9321a-454f-4f51-b99e-196eb4c7d8fc"/>
<br/> Now attach the igw to the vpc <br/>
<img src="https://github.com/user-attachments/assets/c7eb038d-066a-4415-872b-abc95334b7c4"/>
<br/> Now create and configure the following subnets. 5 total across 3 different availablity zones  <br/>
<img src="https://github.com/user-attachments/assets/fed05e56-12a3-4abb-9e0f-c22d05b60b1c"/>
<img src="https://github.com/user-attachments/assets/561977b0-5910-4a07-bb4e-841c27502db3"/>
<img src="https://github.com/user-attachments/assets/2ac4c384-7106-4e5b-81a3-79022a7b0918"/>
<br/> The next 2 subnets will be in separate availability zones <br/>
<img src="https://github.com/user-attachments/assets/b59ada85-1f9c-4509-b053-e5bfda72f91d"/>
<img src="https://github.com/user-attachments/assets/a156e67c-fbce-4d03-882d-a59cf3d49819"/>
<img src="https://github.com/user-attachments/assets/5f13f71c-03fc-4b15-a638-b4d6e8244ba4"/>
<br/> Now for the route tables create both a public and private route table <br/>
<img src="https://github.com/user-attachments/assets/93299e55-c4e5-453d-a375-2c9a78c73a8d"/>
<br/> Associate LAN10 with the public route table. LAN10 will act as the public facing subnet. <br/>
<img src="https://github.com/user-attachments/assets/0d630958-028e-4be9-8713-9abdbf409f2e"/>
<br/> After creating the private route table associate the following subnets <br/>
<img src="https://github.com/user-attachments/assets/1a191418-1e82-4720-9e15-67ba3b2f327d"/>
<br/> Make the private route table the current main so that all new networks provisioned will automatically be associated with it. This does not pose a security risk of having networks exposed publicly. <br/>
<img src="https://github.com/user-attachments/assets/a6c25f91-b20f-42ba-80d3-a1e3ad28e758"/>
<br/> Next we must create a security group, this will define what inbound traffic will be permitted to the vpc. Add an all inbound rule for demo purposes only <br/>
<img src="https://github.com/user-attachments/assets/5acb83b2-6872-4d80-9053-e8f3dfaa5e3c"/>
<br/> Now let's create the network interfaces that will use that security group <br/>
<img src="https://github.com/user-attachments/assets/d3b5430e-d5e4-413b-bb62-424870ad719d"/>
<br/> Configure the vpc, subnet, security group, and select a custom ip and repeat for the remaining subnets to create the network interfaces <br/>
<img src="https://github.com/user-attachments/assets/048d5c60-a607-410a-8c0c-d335bc14f4e4"/>
<img src="https://github.com/user-attachments/assets/3ca7f3cb-4e1d-497c-b57f-526609c4988b"/>
<br/> Disable the "change source/destination check" setting for every interface <br/>
<img src="https://github.com/user-attachments/assets/3124005f-0741-4458-be83-dbbb58fde2d8"/>


## Step 2: Launch the fortigate instance

<br/> Navigate to EC2, click launch and name the instance. Then select the following image from the AWS Marketplace <br/>
<img src="https://github.com/user-attachments/assets/b296695e-e050-435d-943e-e2955794d7b8"/>
<br/> Create a key pair for the instance and select the following configurations. Make sure that the subnet chosen is LAN10. Also make sure the IP assign is disabled so the IP is static and remains after the firewall is refreshed <br/>
<img src="https://github.com/user-attachments/assets/5e58c708-2e5a-4600-a245-db8142ac97c2"/>

## Step 3: Configure the Elastic IP interface
<br/> Navigate to Elastic IP addresses and attach it to the Fortigate instance <br/>
<img src="https://github.com/user-attachments/assets/230fd58b-33d2-44a9-b85c-4e7b83fad46e"/>
<br/> Choose network interface and select the network interface that was created with the EC2 instance <br/>
<img src="https://github.com/user-attachments/assets/8e6c92ec-c6d2-49b2-b6aa-0d3004462ba9"/>
<br/> Now Fortigate has both the LAN interface IP as well as the public IP address in the public subnet <br/>
<img src="https://github.com/user-attachments/assets/1da79da5-75ab-460d-a842-0b2d1bfe9f6f"/>

## Step 4: Add an Internet Gateway to the Public Route Table
<br/> Add the previously created internet gateway route to the public route table <br/>
<img src="https://github.com/user-attachments/assets/074305ef-7677-4b5c-bd51-94dbfd573206"/>
<br/> However now to have all the traffic to traverse the gateway. Add a route to the private routing table and select the LAN20 network interface <br/>
<img src="https://github.com/user-attachments/assets/0a2975c3-dedc-4b18-b141-5220f100991a"/>

## Step 5: Connect to the Fortigate 

<br/> Click the public IP in the fortigate EC2 and select proceed in the browser to the privacy error message <br/>
<br/> Login: <br/>
<img src="https://github.com/user-attachments/assets/cebe8938-0696-4096-b02c-dbef080ed5b8"/>
<img src="https://github.com/user-attachments/assets/f06fea18-7185-4ae7-8e25-c9e6a588dda3"/>
<br/> Let's view our network interfaces <br/>
<img src="https://github.com/user-attachments/assets/8df22f48-2178-4b2f-abf4-cdbd90e65852"/>
<img src="https://github.com/user-attachments/assets/961c8ce7-6409-476c-bdb6-94e8f79e19f5"/>
<br/> There is only one visible so we must return the EC2 in the AWS console and attach the network interfaces created <br/>
<br/> Attach all of the created network interfaces <br/>
<img src="https://github.com/user-attachments/assets/6bf3844e-d9dc-49af-907d-d46d48d769c2"/>
<br/> Note: LAN40 will not be able to be attached because the Fortigate is not available as an option. It will also not work for LAN50 because we can only attach network interfaces that are in the same availability zone. There would need to be a Fortigate Firewall per availability zone. <br/>
<img src="https://github.com/user-attachments/assets/01bb396e-63fd-45fa-bca2-b53f6e4eb57f"/>

<br/> Navigate to VPCs and view the resource map to view the current network <br/>
<img src="https://github.com/user-attachments/assets/c55a1b35-39fe-4d50-8ef0-a0c2e6ef9d6e"/>
<br/> LAN10 is mapped to a public route table. LAN20, 30, 40, and 50 are mapped to a private route table. Both route tables belong to the implicit router wich belongs to the internet gateway. <br/>
<img src="https://github.com/user-attachments/assets/49a36207-445f-4839-87d9-342553c1f9af"/>
<img src="https://github.com/user-attachments/assets/188e7da5-0695-4c19-b866-7b9225d51683"/>

## Step 6: Configure the interfaces to Fortigate
<br/> To add the interfaces created in AWS to Fortigate click on them and select DHCP. It will pull the IP from AWS. <br/> 
<img src="https://github.com/user-attachments/assets/23951884-54ac-403c-aaf7-6f6e3026146d"/>
<br/> The three interfaces are now visible <br/>
<img src="https://github.com/user-attachments/assets/87278361-1411-4655-9eef-b3cb4b3bd968"/>
<br/> That is how you deploy and configure fortinet on an AWS network <br/>
