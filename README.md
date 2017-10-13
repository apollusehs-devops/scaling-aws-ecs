# scaling-aws-ecs
Learn how to build and deploy a fault tolerant, scalable and load balanced AP on AWS ECS.

Scaling Docker on AWS  
https://www.udemy.com/scaling-docker-on-aws

Course Resource
* [Getting Set up on AWS](https://github.com/smalltide/scaling-aws-ecs/blob/master/resource/3-getting-set-up-on-aws.pdf)  
* [Installing and Configuring the AWS CLI](https://github.com/smalltide/scaling-aws-ecs/blob/master/resource/3-installing-and-configuring-the-aws-cli.pdf)  
* [Creating an SSH Keypair](https://github.com/smalltide/scaling-aws-ecs/blob/master/resource/3-creating-an-ssh-keypair.pdf)  
* [Creating a Security Group](https://github.com/smalltide/scaling-aws-ecs/blob/master/resource/3-creating-a-security-group.pdf)  

Skills
1. AWS
2. AWS ECR
3. AWS ECS
4. Docker

#### AWS Scaling  
![alt text](https://github.com/smalltide/scaling-aws-ecs/blob/master/img/aws-scaling.png "aws-scaling")

#### AWS ECS Compare with Other Services  
![alt text](https://github.com/smalltide/scaling-aws-ecs/blob/master/img/ecs1.png "ecs1")
![alt text](https://github.com/smalltide/scaling-aws-ecs/blob/master/img/ecs2.png "ecs2")
![alt text](https://github.com/smalltide/scaling-aws-ecs/blob/master/img/ecs3.png "ecs3")

install docker, docker-machine, docker-compose on Linux
```
  > https://store.docker.com
  > https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
  > curl -sSL https://get.docker.com/ | sh (install latest docker using script)
  > docker version
  > sudo usermod -aG docker <linux user> (add user in docker group)
  > curl -L https://github.com/docker/machine/releases/download/v0.12.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine && chmod +x /tmp/docker-machine && sudo cp /tmp/docker-machine /usr/local/bin/docker-machine (install docker machine)
  > docker-machine version
  > sudo -i
  > sudo curl -L 
  https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose (install docker compose)
  > chmod +x /usr/local/bin/docker-compose
  > exit
  > docker-compose version  
```
Installing and Configuring the AWS CLI
```
  > sudo apt-get install curl
  > curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip" 
  > unzip awscli-bundle.zip
  > sudo ./awscli-bundle/install -i /usr/local/aws -b / /usr/local/bin/aws
  > rm -rf awscli-bundle.zip awscli-bundle
  > aws --version
  > aws configure
  > aws iam list-users
```
Creating an SSH Keypair
```
  > aws ec2 create-key-pair --key-name aws-ice --query 'KeyMaterial' --output text > ~/.ssh/aws-ice.pem
  > aws ec2 describe-key-pairs
  > aws ec2 describe-key-pairs --key-names aws-ice
  > aws ec2 delete-key-pair --key-names aws-ice (for delete)
```
Creating a Security Group (Security Group used for setting ec2 network rule)
```
  > aws ec2 create-security-group --group-name ice_sg_ap-northeast-1 --description "security group for ice on ap-northeast-1"
  > aws ec2 describe-security-groups --group-id sg-cxxxxxxx
  > aws ec2 authorize-security-group-ingress --group-id sg-cxxxxxxx --protocol tcp --port 22 --cidr 0.0.0.0/0
  > aws ec2 authorize-security-group-ingress --group-id sg-cxxxxxxx --protocol tcp --port 80 --cidr 0.0.0.0/0
  > aws ec2 authorize-security-group-ingress --group-id sg-cxxxxxxx --protocol tcp --port 5432 --cidr 0.0.0.0/0 --source-group sg-cxxxxxxx (for RDS)
  > aws ec2 authorize-security-group-ingress --group-id sg-cxxxxxxx --protocol tcp --port 6379 --cidr 0.0.0.0/0 --source-group sg-cxxxxxxx (for redis)
  > aws ec2 describe-security-groups --group-id sg-cxxxxxxx
  > aws ec2 delete-security-group --group-id sg-cxxxxxxx
```