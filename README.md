##### Creating a Load Balancer Service with AWS EKS <br/><br/>
* Clone the repository and move to folder lab-04 <br/>
* Create a cluster with eksctl <br/>
  $ eksctl create cluster --name k8sLB --version 1.23 --region us-west-2 --nodegroup-name k8sLBnodes --node-type t3.medium --nodes 2 <br/>
* Switch context <br/>
  $ aws eks --region us-west-2 update-kubeconfig --name k8sLB <br/>
* Refer below commands for verification of contexts.<br/> 
  $ kubectl config view <br/>
  $ kubectl config current-context (output should be k8sdemo). <br/>
  $ kubectl config get contexts <br/>
  $ kubectl config use-context <<-context name->> <br/>
* Verify - Check nodes and pods in the EKS cluster. <br/>
  $ kubectl get nodes -o wide <br/>
  $ kubectl get pods <br/>
* There would not be any pods in system but the output for nodes would be like below. This signifies that the cluster is reachable from CLI <br/>
  ![image](https://user-images.githubusercontent.com/92582005/203014906-d76dc9d2-8a56-4b60-b857-450a7907c172.png) <br/>
* Create deployments <br/>
  $ kubectl apply -f ./k8s-deployments/ <br/>
* Create services <br/>
  $ kubectl apply -f ./k8s-services/ <br/>
* Check the pods. The worker pod will have a high restarts value because initially there were no connection between the pods (services were not created) <br/>
  ![image](https://user-images.githubusercontent.com/92582005/203015881-ee0fb50f-96e7-4d0e-974b-2d07ceee4ce7.png) <br/>
* Check services. Wait for 5-10 mins for the load balancers to be ready to serve traffic. <br/>
* Get the DNS names of the load balancers. Under column "EXTERNAL-IP" <br/>
  ![image](https://user-images.githubusercontent.com/92582005/203016326-5be9c264-91c0-4485-ac7b-9d1a27e80eca.png) <br/>
* Access the "vote" service and place your vote! <br/>
  http://<<-DNS Name from above->>:5000 <br/>
* Output of the "vote" service should be like below. <br/>
  ![image](https://user-images.githubusercontent.com/92582005/203017190-35953dae-9864-4e3e-888c-3fad0e9ff496.png) <br/>
* Access the "result" service and check the results: <br/>
  http://<<-DNS Name from above->>:5001 <br/>
* Output of the "result" service would be like below. <br/>
  ![image](https://user-images.githubusercontent.com/92582005/203017536-458ad56b-19b9-49de-b5ca-2da8d6f87a6a.png) <br/>
* Delete deployments and services. <br/>
  $ kubectl delete -f ./k8s-deployments/ <br/>
  $ kubectl delete -f ./k8s-services/ <br/>
  $ kubectl delete all --all <br/>
* Verify from the EC2 console that the load balaner (classic) is deleted. <br/>
* Clean up AWS enviornment <br/>
  $ eksctl delete cluster --name k8sLB <br/><br/>
![image](https://user-images.githubusercontent.com/92582005/203019675-03c94d76-f3c1-4622-bede-0ceaf44a74fe.png) <br/>
#### References<br/>
[How do I troubleshoot service load balancers for Amazon EKS?](https://aws.amazon.com/premiumsupport/knowledge-center/eks-load-balancers-troubleshooting/)<br/>
[How do I resolve HTTP 503 (Service unavailable) errors when I access a Kubernetes Service in an Amazon EKS cluster?](https://aws.amazon.com/premiumsupport/knowledge-center/eks-resolve-http-503-errors-kubernetes/)<br/>
  
  


