yum install git -y
helm repo add eks https://aws.github.io/eks-charts
kubectl apply -k "github.com/aws/eks-charts/stable/aws-load-balancer-controller//crds?ref=master"
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=eks --set serviceAccount.create=false --set serviceAccount.name=default

helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=eks --set serviceAccount.create=true --set serviceAccount.name=aws-load-balancer-controller

#https://docs.aws.amazon.com/eks/latest/userguide/add-ons-images.html   alb Images
helm upgrade -i aws-load-balancer-controller eks/aws-load-balancer-controller \
                --set image.repository=602401143452.dkr.ecr.us-east-1.amazonaws.com/amazon/aws-load-balancer-controller \
  --set enableWaf=false --set enableWafv2=false --set enableShield=false \
 --set clusterName=eks \
   --set serviceAccount.create=false --set serviceAccount.name=default \
     --set region=us-east-1 \
   --set autoDiscoverAwsRegion=true \
   --set autoDiscoverAwsVpcID=true \
   #--set vpcId=vpc-0f06bab7c9b9e8c12 \
   -n kube-system
