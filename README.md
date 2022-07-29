# eks-fargate-sample-1

EKS Fargate with ALB Ingress Controller Sample

## Reference  

[eks-alb-ingress-controller-fargate](https://aws.amazon.com/premiumsupport/knowledge-center/eks-alb-ingress-controller-fargate/)  
[using-alb-ingress-controller-with-amazon-eks-on-fargate](https://aws.amazon.com/blogs/containers/using-alb-ingress-controller-with-amazon-eks-on-fargate/)
[eksctl](https://github.com/weaveworks/eksctl)

## Optional cluster configuration (Bottlerocket)

conntrack configuration

By default kube-proxy will set the nf_conntrack_max kernel parameter to a default value that may differ from what Bottlerocket originally sets at boot. If you prefer to keep Bottlerocket's default setting, edit the kube-proxy configuration details with:

kubectl edit -n kube-system daemonset kube-proxy
Add --conntrack-max-per-core and --conntrack-min to the kube-proxy arguments like so (a setting of 0 implies no change):

      containers:
      - command:
        - kube-proxy
        - --v=2
        - --config=/var/lib/kube-proxy-config/config
        - --conntrack-max-per-core=0
        - --conntrack-min=0

## Commands

- To get the ALB policy
    `curl -o iam_policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.4.2/docs/install/iam_policy.json`
- To create iam policy via AWS CLI
    `aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json`
- To create cluster using config file
    `eksctl create cluster -f cluster-fargate.yaml`
