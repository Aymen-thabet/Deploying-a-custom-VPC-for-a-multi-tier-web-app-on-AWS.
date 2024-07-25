This mini project is made using only aws interface.

Description : I created a custom VPC with 4 subnets, 2 public and 2 private, and deployed a load balancer for my public subnets (automatically creating a node in each one) to assure high availability and fault tolerance, which receives public requests from the Internet and distributes the workload between both instances which are located in the private subnets. Finally, I created an auto-scaling group (in/out) assuring elasticity of the infrastracture in case of failure of an instance or heavy inbound traffic .


1.	I created VPC with 2 public and 2 private subnets. with the corresponding Internet Gateway and Route Tables settings. The private subnets do not have routes to the IGW, and the public subnets must have routes to the IGW

2.	![image](https://github.com/user-attachments/assets/462b954e-5e08-43c4-a83a-af9f13732984)

3.	I created a security group for the web/app instances named WebSG, that allows inbound traffic only from the application load balancer security group as a source and allows all outbound traffic. & I created another security group for the load balancer (ALBSG) that allows outbound HTTP to the web/app security group (WebSG) and allows inbound traffic from the internet on port HTTP.

4.	![image](https://github.com/user-attachments/assets/79b1a2b9-4f37-4785-9446-fada5af6f209)

![image](https://github.com/user-attachments/assets/3222d24d-494e-4cfe-9f27-63c4cd03aecb)

I launched 2 EC2 instances in the private subnets with no public IP, and attached a user data script to download httpd to execute:

echo " This is server *1* in AWS Region US-EAST-1 in AZ US-EAST-1A " > /var/www/html/index.html for server1

echo " This is server *2* in AWS Region US-EAST-1 in AZ US-EAST-1B " > /var/www/html/index.html for server2

![image](https://github.com/user-attachments/assets/904dbed9-bfc8-4b68-9647-17939668a7ac)


I created a target group and registered the EC2 instances.

![image](https://github.com/user-attachments/assets/6b799677-d593-47e9-88a1-246d2392b7fb)


I created an Application Load Balancer assigned to the 2 public subnets in 2 different AZ


![image](https://github.com/user-attachments/assets/27bae72a-e6f3-4903-b54f-cc2d603f18fb)


Created Launch template and auto-scaling group to ensure high availability & fault tolerance 


![image](https://github.com/user-attachments/assets/77684eb4-e0c4-4284-bb51-d27e303e00a5)


Then testing the fonctionning of the AS group 

![image](https://github.com/user-attachments/assets/e90cbf23-168e-4866-a1a6-b5087b8873ed)

"creation of a new instance " (working)

FINALLY : connecting to the ALB from its public address and reloading , Notice that the server changing between the 2 AZ !. 

SERVER1
![Capture d'écran 2024-07-14 160023](https://github.com/user-attachments/assets/09cbc628-e1bb-4766-8ba5-23bdaa78907a)

SERVER2
![Capture d'écran 2024-07-14 160032](https://github.com/user-attachments/assets/2320fd9e-1c76-46b3-a313-211efee6b208)












