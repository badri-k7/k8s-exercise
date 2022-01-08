Foundation - VPC Creation:

    aws cloudformation create-stack --stack-name cf-vpc-multiaz-hybrid --template-body file:///home/kesavanb/study/k8s-cluster-exercise/global/vpc/cf-vpc-multi-az-hybrid.yaml --on-failure DO_NOTHING --parameters ParameterKey=ChildAccName,ParameterValue=hyphen-k8s-cluster-exercise \
    ParameterKey=VPCCidrBlock,ParameterValue=10.235.112.0/21 ParameterKey=PublicSubnetCIDR1,ParameterValue=10.235.112.0/24 ParameterKey=PublicSubnetCIDR2,ParameterValue=10.235.115.0/24 ParameterKey=PublicSubnetCIDR3,ParameterValue=10.235.118.0/24 ParameterKey=PrivateSubnetCIDR1,ParameterValue=10.235.113.0/24 ParameterKey=PrivateSubnetCIDR2,ParameterValue=10.235.116.0/24 ParameterKey=PrivateSubnetCIDR3,ParameterValue=10.235.119.0/24 --no-verify-ssl --profile badri-billing-account  


Foundation - IAM Roles Creation
    aws cloudformation create-stack --stack-name cf-iam-roles-instance-profile --template-body file:///home/kesavanb/study/k8s-cluster-exercise/global/iam/cf-iam-roles-instance-profiles.yaml --capabilities CAPABILITY_NAMED_IAM --on-failure DO_NOTHING --no-verify-ssl --profile badri-billing-account

Foundation - Security Group Creation

    aws cloudformation create-stack --stack-name cf-security-groups --template-body file:///home/kesavanb/study/k8s-cluster-exercise/k8s-cluster/security-group/cf-k8s-controlplane-workernode-sg.yaml --profile badri-billing-account --no-verify-ssl

Foundation - 