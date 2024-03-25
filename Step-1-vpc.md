Create custom VPC with CIDR 10.0.0.0/16 with following components.

1. Create IGW.
2. 3 public subnet- 10.0.1.0/24,10.0.2.0/24,10.0.3.0/24 in multiple az for fault tolerance.
3. 1 private subnet- 10.0.4.0/24.
4. Create 2 Route Table for public and private subnet.
5. here we will require 3 NAT GW for communicating with private subnet from three public subnet
6. Create 2 security group for public and private routing distinctly for inbound and outbound traffic rule.

Follow following steps to create and attach components for ingress and egress for public and private subnet.


1. **Create a VPC:**
   - Go to the VPC dashboard in the AWS Management Console.
   - Click on "Create VPC."
   - Enter the CIDR block `10.0.0.0/16`.
   - Create the VPC.

2. **Create Subnets:**
   - Create three public subnets with CIDR blocks `10.0.1.0/24`, `10.0.2.0/24`, and `10.0.3.0/24`.
   - Create one private subnet with the CIDR block `10.0.4.0/24`.

3. **Create Internet Gateway (IGW):**
   - Go to the Internet Gateways section in the VPC dashboard.
   - Create an IGW and attach it to your VPC.

4. **Create NAT Gateways:**
   - Go to the NAT Gateways section in the VPC dashboard.
   - Create three NAT Gateways, one for each public subnet.
   - Assign Elastic IPs to each NAT Gateway.
   - Update the route tables of the public subnets to route traffic destined for the internet (`0.0.0.0/0`) to their respective NAT Gateways.

5. **Route Tables:**
   - Create separate route tables for public and private subnets.
   - Associate the public subnets with the public route table.
   - Associate the private subnet with the private route table.
   - In the public route table, add a route for `0.0.0.0/0` pointing to the Internet Gateway.
   - In the private route table, add a route for `0.0.0.0/0` pointing to the respective NAT Gateway in each public subnet.


6. **Security Groups:**

To configure security groups for a WordPress application running in an ECS cluster in a public subnet and an RDS SQL database running in a private subnet within the same VPC, you'll need to set up the following:

1. **For WordPress ECS Cluster (Public Subnet):**
   - Create a security group for the ECS cluster instances.
   - Allow inbound traffic on port 80 (HTTP) and 443 (HTTPS) from anywhere (0.0.0.0/0) to allow access to the WordPress application from the internet.
   - Optionally, restrict SSH access (port 22) to a specific IP range or your own IP for management purposes.

2. **For RDS SQL Database (Private Subnet):**
   - Create a security group for the RDS instance.
   - Allow inbound traffic on the port used by your SQL database engine (e.g., 3306 for MySQL) from the security group associated with the ECS cluster. This allows the ECS instances to communicate with the RDS instance.
   - Optionally, restrict inbound access to the SQL port to only the ECS cluster's security group, enhancing security.

Here's a simplified example of how you might configure these security groups using AWS:

### Security Group for ECS Cluster (WordPress)
- **Name:** WordPressECS-SG
- **Inbound Rules:**
  - HTTP (Port 80) from anywhere (0.0.0.0/0)
  - HTTPS (Port 443) from anywhere (0.0.0.0/0)
  - SSH (Port 22) restricted to your IP or management range

### Security Group for RDS SQL Database
- **Name:** RDSSQL-SG
- **Inbound Rules:**
  - SQL Port (e.g., 3306 for MySQL) from WordPressECS-SG (the security group associated with the ECS cluster)

By configuring the security groups in this way, you ensure that your WordPress application running in the ECS cluster can communicate securely with the RDS SQL database while also allowing access to the WordPress site from the internet. Adjust the security group rules based on your specific requirements and best practices.

