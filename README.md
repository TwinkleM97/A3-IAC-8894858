# Assignment 3 - AWS CloudFormation: EC2 and RDS Deployment

## Info
- **Name**: Twinkle Mishra
- **Student ID**: 8894858

## Objective
To deploy an AWS infrastructure using CloudFormation that includes:
- A VPC with public/private subnets
- An EC2 instance accessible via public IP
- An RDS MySQL database publicly accessible

## Folder Structure
```
A3-IAC-TM-8894858/
├── templates/
│   ├── networking-template.yaml
│   ├── ec2-template.yaml
│   ├── rds-template.yaml
├── screenshots/
└── assignment3-keypair.pem
```

## Steps Performed

### 1. Deploy Networking Infrastructure
```bash
aws cloudformation create-stack   --stack-name assignment3-networking-twinklemishra   --template-body file://networking-template.yaml   --region us-east-1   --capabilities CAPABILITY_NAMED_IAM
```
#### VPC and Subnets Created
![VPC](screenshots/13_vpc_console_view.png)
![Subnets](screenshots/14_subnets-public-private-networks.png)

#### Internet Gateway + Route Table
![Route Table](screenshots/15_public_route_table.png)

#### Stack Events and Outputs
![Networking Events](screenshots/11_networking_stack_events.png)
![Networking Outputs](screenshots/12_networking_stack_outputs.png)

---

### 2. Deploy EC2 Instance
```bash
aws cloudformation create-stack   --stack-name assignment3-ec2-twinklemishra   --template-body file://ec2-template.yaml   --parameters ParameterKey=AMIId,ParameterValue=ami-0c02fb55956c7d316                ParameterKey=InstanceType,ParameterValue=t2.micro                ParameterKey=KeyPairName,ParameterValue=assignment3-keypair   --region us-east-1   --capabilities CAPABILITY_NAMED_IAM
```

#### Stack Events and Outputs
![EC2 Events](screenshots/03_ec2_stack_events.png)
![EC2 Outputs](screenshots/05_ec2_stack_outputs.png)

#### Instance Running + Public Web Output
![EC2 Running](screenshots/07_ec2_instance_running.png)
![Web Page](screenshots/02_ec2_web_outpu.png)

#### CLI Verification
```bash
aws ec2 describe-instances --region us-east-1
```
![CLI EC2](screenshots/09_terminal_describe_ec2.png)

---

### 3. Deploy RDS Instance
```bash
aws cloudformation create-stack   --stack-name assignment3-rds-twinklemishra   --template-body file://rds-template.yaml   --region us-east-1   --capabilities CAPABILITY_NAMED_IAM
```

#### Stack Events and Outputs
![RDS Events](screenshots/04_rds_stack_events.png)
![RDS Outputs](screenshots/06_rds_stack_outputs.png)

#### RDS Running + CLI Verification
![RDS Console](screenshots/08_rds_instance_running.png)

```bash
aws cloudformation describe-stacks   --stack-name assignment3-rds-twinklemishra   --query "Stacks[0].Outputs[?OutputKey=='RDSEndpoint'].OutputValue"   --output text
```
![CLI RDS](screenshots/10_terminal_describe_rds.png)

---

### 4.Stack Overview (All Stacks)
![CloudFormation Status](screenshots/01_stacks_status.png)

---

## Notes
- Region: `us-east-1`
- KeyPair: `assignment3-keypair.pem`
- Security Groups allow SSH (EC2) and MySQL (RDS)