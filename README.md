# platformfactorycapstone
export EKS_CLUSTER_NAME=my-cluster

export AWS_REGION=us-east-1

export PUBLIC_SUBNET_1=subnet-11111111

export PUBLIC_SUBNET_2=subnet-22222222

export PRIVATE_SUBNET_1=subnet-33333333

export PRIVATE_SUBNET_2=subnet-44444444

export KEY_PAIR_NAME=my-key-pair

export INSTANCE_PROFILE_NAME=my-instance-profile

# To run execute below
eksctl create cluster -f your-config-file.yaml


