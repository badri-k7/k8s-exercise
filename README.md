# Hyphen K8SExercise
This repository contains all the Codes and Information needed to create Kubernetes cluster and Kube manifests needed to complete the exercise
## Environment used for this exercise
1. AWS - Public cloud provider
2. K8S - Elastic Kubernetes Service ( V1.20 )
3. K6.io - Benchmarking tool
## Directory Structure
```text
├── global 
│   ├── iam # ( IAM Roles for cluster to other AWS Service )
│   │   └── cf-iam-roles-instance-profiles.yaml
│   └── vpc # ( VPC created to host the K8S Cluster )
│       └── cf-vpc-multi-az-hybrid.yaml
├── k8s-cluster 
│   ├── core-cluster-nodes # ( K8S Master node )
│   │   ├── cf-k8s-multi-node-cluster-controlplane.yaml
│   │   └── parameters
│   ├── core-security-group # ( Firewall rules for Master & Worker & Management node )
│   │   └── cf-k8s-controlplane-workernode-sg.yaml
│   ├── core-worker-nodes # ( K8S Worker Nodes )
│   │   ├── cf-k8s-cluster-worker-node.yaml
│   │   └── launch-template
│   │       └── cf-k8s-cluster-workernode-launch-template.yaml
│   └── mgmt-administration-node # ( K8S Management Node for Cluster activities )
│       └── cf-k8s-cluster-admin-node.yml
├── k8s-cluster-app-manifests 
│   ├── http-echo
│   │   ├── ingress.yaml
│   │   ├── pod.yaml
│   │   └── service.yaml
│   ├── nginx-ingress-controller
│   │   └── README.md
│   └── promethus # ( Moni
│       ├── cluster-role-and-role-binding.yaml
│       ├── configmap.yaml
│       ├── deployment.yaml
│       └── service.yaml
└── output
│   ├── promql-export
│   │   ├── avg_cpu_usage_30m.csv
│   │   ├── avg_http_req_30m.csv
│   │   └── avg_mem_usage_30m.csv
```
1. global - Contains the base infra artifacts needed for the K8S Cluster
2. k8s-cluster - Contains the infra artifacts needed to provision Master and Worker nodes for K8S.
3. k8s-cluster-app-manifests - Contains the K8S app manifests needed for creating Ingress controller, Prometheus and Terraform HTTP Echo service
4. output - Contains the promql csv export of the data requested from benchmarking load
## Usage Instructions
```bash
Step 1: Core - Foundation - VPC Creation:
aws cloudformation create-stack --stack-name cf-vpc-multiaz-hybrid --template-body file:///k8s-cluster-exercise/global/vpc/cf-vpc-multi-az-hybrid.yaml --on-failure DO_NOTHING 
Step 2: Core - Cluster - IAM Roles Creation
aws cloudformation create-stack --stack-name cf-iam-roles-instance-profile --template-body file:///k8s-cluster-exercise/global/iam/cf-iam-roles-instance-profiles.yaml --capabilities CAPABILITY_NAMED_IAM --on-failure DO_NOTHING 
Step 3: Core - Cluster - Security Group Creation
aws cloudformation create-stack --stack-name cf-security-groups --template-body file:///k8s-cluster-exercise/k8s-cluster/security-group/cf-k8s-controlplane-workernode-sg.yaml 
 
Step 4: Core - Cluster - K8S Cluster creation 
aws cloudformation create-stack --stack-name cf-k8s-cluster --template-body file:///k8s-cluster-exercise/k8s-cluster/core-cluster-nodes/cf-k8s-multi-node-cluster-controlplane.yaml  --capabilities CAPABILITY_NAMED_IAM
Step 5: Core - Cluster - Worker Node Launch Template Creation
aws cloudformation create-stack --stack-name cf-k8s-worker-node-launch-template --template-body file:///k8s-cluster-exercise/k8s-cluster/core-worker-nodes/launch-template/cf-k8s-cluster-workernode-launch-template.yaml 
Step 6: Core - Cluster - Managed worker node Autoscaling Group  
aws cloudformation create-stack --stack-name cf-k8s-managed-worker-nodes --template-body file:///k8s-cluster-exercise/k8s-cluster/core-worker-nodes/cf-k8s-cluster-worker-node.yaml
```
## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.
Please make sure to update tests as appropriate.
## License
[MIT](https://choosealicense.com/licenses/mit/)