# aws_IAM-RBAC_EKS
aws-auth
Refer to aws_auth.yaml file

apiVersion: v1
kind: ConfigMap
metadata:
  name: aws-auth
  namespace: kube-system
data:
  mapRoles: |
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::00000000000:role/eksctl-robin-personal-cluster-nod-NodeInstanceRole-TUKH4Z187ANC
      username: system:node:{{EC2PrivateDNSName}}
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::00000000000:role/eksctl-robin-personal-cluster-nod-NodeInstanceRole-2RY0KK33CGIG
      username: system:node:{{EC2PrivateDNSName}}
  mapUsers: |
    - userarn: arn:aws:iam::00000000000:user/eks-trainee
      username: eks-trainee
    - userarn: arn:aws:iam::00000000000:user/eks-developer
      username: eks-developer
    - userarn: arn:aws:iam::00000000000:user/eks-super-admin
      groups:
      - system:masters
      username: eks-super-admin
Role and Role Bindings
Refer to role_mappings folder

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: developer-clusterrole
rules:
  - apiGroups: [""]
    resources: ["nodes", "namespaces", "pods"]
    verbs: ["get", "list"]
  - apiGroups: ["apps"]
    resources: ["deployments" ,"daemonsets" ,"statefulsets" ,"replicasets"]
    verbs: ["get", "list", "create"]
  - apiGroups: [ "batch"]
    resources: ["jobs"]
    verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: developer-clusterrole-binging
subjects:
  - kind: User
    name: eks-developer
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: developer-clusterrole
  apiGroup: rbac.authorization.k8s.io
