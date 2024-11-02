# CloudFormation Ansible Server Builder

This project automates the process of launching and configuring EC2 instances using Ansible. It includes CloudFormation templates to set up the necessary infrastructure and Ansible playbooks to manage the EC2 instances.

## Prerequisites

- AWS CLI configured with appropriate permissions
- An existing EC2 Key Pair in AWS

## Setup

### 1. Clone the Repository

```sh
git clone https://github.com/jordanholtdev/stack-automator.git
cd stack-automator
```
### 3. Configure AWS CLI

Ensure your AWS CLI is configured with the necessary permissions:
```sh
aws configure
```
### 4. Create `parameters.json` file
Create a `parameters.json` file and include the necessary parameters for your environment.

### 5. Deploy the CloudFormation Stack

Deploy the CloudFormation stack to set up the infrastructure:

```sh
aws cloudformation create-stack \
--stack-name ansible-control-node \
--template-body file://ansible_control_node.yaml \ 
--parameters file://parameters.json \ 
--capabilities CAPABILITY_IAM
```

### 6. Copy the Private Key to the Bastion host
```sh
nano /home/ec2-user/your-key.pem

# Set correct permissions on the private key file

chmod 600 /home/ec2-user/your-key.pem
```

### 7. SSH into Ansible Control Node and add Key

```sh
ssh -i /home/ec2-user/your-key.pem ec2-user@<PrivateInstancePrivateIP>

# copy the private key to the control node 
sudo scp -i /home/ec2-user/your-key.pem /home/ec2-user/your-key.pem ec2-user@<PrivateInstancePrivateIP>:/home/ec2-user

# set correct permissions
chmod 600 /home/ec2-user/your-key.pem
```
## Usage
### Launch EC2 Instances
Run the launch_ec2.yml playbook to launch and configure EC2 instances:

```sh
ansible-playbook playbooks/launch_ec2.yml -e "key_name=your-key instance_type=t2.micro ami_id=ami-0abcdef1234567890 region=us-east-1"
```
### Terminate EC2 Instances
Run the terminate_ec2.yml playbook to terminate the EC2 instances:
```sh
ansible-playbook playbooks/terminate_ec2.yml -e "region=us-east-1"
```
