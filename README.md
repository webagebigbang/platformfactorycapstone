# platformfactorycapstone
export EKS_CLUSTER_NAME=my-cluster

export AWS_REGION=us-east-1

export VPC_ID=$(aws ec2 describe-vpcs --filters "Name=tag:Name,Values=your-vpc-name" --query 'Vpcs[0].VpcId' --output text)


export PUBLIC_SUBNET_1=subnet-11111111

export PUBLIC_SUBNET_2=subnet-22222222

export PRIVATE_SUBNET_1=subnet-33333333

export PRIVATE_SUBNET_2=subnet-44444444

export KEY_PAIR_NAME=my-key-pair

export INSTANCE_PROFILE_NAME=my-instance-profile

# Getting above values
aws ssm get-parameter --name "AWS_REGION" --query "Parameter.Value" --region us-east-1 --output text

aws ssm get-parameter --name "AWS_REGION" --query "Parameter.Value" --region us-west-2 --output text

aws ssm get-parameter --name "PUBLIC_SUBNET_1" --query "Parameter.Value" --region us-west-2 --output text

aws ssm get-parameter --name "PUBLIC_SUBNET_2" --query "Parameter.Value" --region us-west-2 --output text

aws ssm get-parameter --name "PRIVATE_SUBNET_1" --query "Parameter.Value" --region us-west-2 --output text

aws ssm get-parameter --name "PRIVATE_SUBNET_2" --query "Parameter.Value" --region us-west-2 --output text

aws ssm get-parameter --name "KEY_PAIR_NAME" --query "Parameter.Value" --region us-west-2 --output text

aws ssm get-parameter --name "INSTANCE_PROFILE_NAME" --query "Parameter.Value" --region us-west-2 --output text

export VPC_ID=$(aws ec2 describe-vpcs --filters "Name=tag:Name,Values=your-vpc-name" --query 'Vpcs[0].VpcId' --output text)


# Configuration file 

apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: ${EKS_CLUSTER_NAME}
  region: ${AWS_REGION}
  version: "1.26"
vpc:
  id: ${VPC_ID}
  subnets:
    public:
      ${PUBLIC_SUBNET_1}:
        name: public-subnet-1
      ${PUBLIC_SUBNET_2}:
        name: public-subnet-2
    private:
      ${PRIVATE_SUBNET_1}:
        name: private-subnet-1
      ${PRIVATE_SUBNET_2}:
        name: private-subnet-2

iam:
  withOIDC: true
addons:
  - name: aws-ebs-csi-driver
    version: v1.18.0-eksbuild.1
    attachPolicyARNs:
      - arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy
managedNodeGroups:
  - name: mng-1
    desiredCapacity: 4
    instanceType: m5.2xlarge
     ssh:
      allow: false
    iam:
      withAddonPolicies:
        albIngress: true
        autoScaler: true
        ebs: true


# To run execute below

echo $VPC_ID

eksctl create cluster -f your-config-file.yaml



