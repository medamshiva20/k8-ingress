### Ingress Controller

Reference

-----
https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.6/deploy/installation/

After opened this document you can click on deployment option
-----

1.eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster eks-spot-cluster1 \
    --approve

2.curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.6.1/docs/install/iam_policy_us-gov.json

3.curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.6.1/docs/install/iam_policy_cn.json

4.curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.6.1/docs/install/iam_policy.json

5.aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json

6.eksctl create iamserviceaccount \
--cluster=eks-spot-cluster1 \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::637423251643:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region us-east-1 \
--approve

7.helm repo add eks https://aws.github.io/eks-charts

8.helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=eks-spot-cluster1 --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller