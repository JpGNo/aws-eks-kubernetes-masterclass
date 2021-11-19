eksctl utils associate-iam-oidc-provider --region=us-east-1 --cluster=eks --approve

eksctl create iamserviceaccount \
    --name default \
    --namespace server \
    --cluster eks \
    --attach-policy-arn arn:aws:iam::180789647333:policy/nginx \
    --approve \
    --override-existing-serviceaccounts  --region us-east-1


eksctl create iamserviceaccount \
    --name alb-ingress-controller \
    --namespace kube-system \
    --cluster eks \
    --attach-policy-arn arn:aws:iam::180789647333:policy/ALBIngressControllerIAMPolicy \
    --approve \
    --override-existing-serviceaccounts  --region us-east-1


eksctl create iamserviceaccount \
    --name cluster-autoscaler \
    --namespace kube-system \
    --cluster eks \
    --attach-policy-arn arn:aws:iam::180789647333:policy/AutoScalerPolicy \
    --approve \
    --override-existing-serviceaccounts  --region us-east-1


eksctl  get iamserviceaccount --cluster eks


kubectl annotate serviceaccount -n server default eks.amazonaws.com/role-arn=arn:aws:iam::180789647333:role/eksctl-eks-addon-iamserviceaccount-Role
