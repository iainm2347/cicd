# IaC Configuration

## Description

AWS was chosen as the Cloud platform provider due to it's suitable choices of resources to create a basic web server (apache) to serve content over port 80 consisting of two json templates comprising of VPC & EC2 resources.

### Network setup

The networking setup consists of the following components:

- VPC	-				Virtual network to host the required components
- Public subnets	-	Used for publicly accessible web server	
- Private subnets	-	Optional in this instance but could be used to also to host web server (will explain below)
- IGW				-	Allows communication between VPC & the internet (inbound access in our case)
- Route table(s)	-	Determines packet flow direction i.e IGW or NAT Gateway

VPC & public subnet values are also exported which can then be used in the compute setup stack.

### Network changes/improvements

For the purpose of this workflow, the ec2 instance will reside in a public subnet with a route to the internet gateway. However to increase security, I would also do the following:

- CloudFront distribution (can be used to offload SSL)
- Application load balancer as CloudFront origin (public subnet)
- Autoscaling group for high availability (private subnet)
- Traffic would exit the network via the NAT Gateway

The above would allow the website/application to be publicly accessible whilst keeping the ec2 instance private which could be managed by AWS Systems Manager. 



### Compute setup

The compute setup consists of the following components:

- Network ACL 		-	Used to control network inbound/outbound access at subnet level
- Security group	-	Used to control network inbound/outbound access at Network Interface level
- EC2 instance		-	EC2 instance resource creation which includes custom metadata used to bootstrap install Apache webserver & PHP as well as initiate services

For the purpose of this exercise, bootstrap installing apache/PHP to serve a single static PHP file is simple and effective.

To increase high availability, you could also create an autoscaling group with a launch template.



### Deployment commands

For the purpose of this exercise, the networking & compute infrastructure will be deployed via the CICD pipeline. However to create a stack(s), the following command could be used:

* aws cloudformation deploy --template-file /path_to_template/template.json --stack-name my-new-stack 

