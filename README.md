# Helm-Netflix-Clone
Step 1 : Create cloudformation stack to create vpc and  other resources <br>
         Use s3 url : https://amazon-eks.s3.us-west-2.amazonaws.com/cloudformation/2020-04-21/amazon-eks-vpc-private-subnets.yaml  <br>

Step 2 : Create IAM role <li> helm-eks-cluster-role </li> <li> helm-eks-worker-node-role -> AmazonEC2ContainerRegistryReadOnly, AmazonEKS_CNI_Policy, AmazonEKSWorkerNodePolicy </li>

Step 3: Update the config <pre>aws eks update-kubeconfig --region ap-south-1 --name CLUSTER_NAME --profile PROFILE_NAME </pre>

Step 4: Create namespace or else work default <pre> kubectl create ns helm </pre>

Step 5: Meanwhile, create helm chart <br>
 <pre>helm create netflix-clone</pre>

Step 6: Customize chart & values as per our requirement

Step 7: Package the chart 
<pre> helm package netflix-clone </pre>

Step 8: Once clutser and nodegroup get creates, install package helm chart
<pre> Install clone application - helm install my-netflix-clone ./netflix-clone-0.1.0.tgz
install for monitoring - helm install prometheus prometheus-community/kube-prometheus-stack -n helm </pre>

Step 9 : To check <pre> kubeclt get pods -n helm </pre>


Step 10: Check Ingress controller, with command 
<pre> kubectl get pods --all--namespaces -l app.kuberenets.io/name=ingress-ngnix</pre>
If not found
<pre> kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml </pre>

Step 11 : To verify 
<pre>Describe ingress -> kubectl describe ingress netflix-clone
Get service  ->  kubectl get svc
Curl NodePort -> curl http://<node-i>:<node-port>
Info to get node ip -> kubectl get nodes -o wide (use external-ip)
for nodeport -> kubectl get svc </pre>

Step 12: To verify the deployment, check command
<pre> kubectl get all -l app=my-netflix-clone </pre>
