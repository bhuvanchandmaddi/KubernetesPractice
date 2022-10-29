## Ingress demo lab activities
* These activities were performed in eks cluster
* Deployed nginx-ingress-controller using below cmds:
```
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```
### Activity 1
1. Created a sample deployment(simple-eg1-deploy.yaml) of nginx with 3 replicas
2. Created a ClusterIp sevice for that deployment using below cmd:
```
 kc expose deploy nginx-deploy-main --name nodeport-demo --port 80
```
3. Created a Ingress resource with tls enabled(simpl1-eg1-ingress-with-tls)

4. Create a route53 record for the hostname used in ingress and pint it to the ingress loadbalancer controller(This can be done before creating deployment,service as well but after deployment of ingress nginx controller because after ingress controller deployment only loadbalncer sevice gets created in ingress-nginx namespace which is responsible for launching aws application loadbalancer) 

5. Now you can access the nginx app using rour53 record url, but it will show not secure don't worry as it is used for internal testing only

### Activity 2
1. Created 3 deployments with 1 replicas each, all the 3 are nginx applications only but customized default landing page with custom msgs to diffrentiate.
2. Created services for all the 3 deployment using below cmd:
```
kc expose deploy nginx-deploy-main --name app1-service --port 80
kc expose deploy nginx-deploy-blue --name app2-service --port 80
kc expose deploy nginx-deploy-green --name app3-service --port 80
```
3. Created ingress resource using(ingress-rules-host-based-routing.yaml file).Since it is host based routing created 3 dns records all pointing to the same ingress loadbalancer.

4. After applying ingress resource, I was able to access the 3 apps using 3 records 

5. Created another ingress resource using (ingress-rules-path-based-routing). This time single record is sufficient and pointed that to the sane Ingress controller loadbalancer

5. After applying ingress resource, I was able to access all 3 applications at route53record,route53record/blue and route53record/green

### Activity 3
* Created a single yaml manifest file(complete-deployment.yaml) to create all 3 deployments and their respective services along with ingress
* Used path based routing, So just create 1 dns record and point that to ingress controller loadbalncer ip.