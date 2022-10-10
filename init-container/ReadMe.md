## steps perfomrd
* demo1 is starit forward, just main container will wait for 60 sec(i.e init container completion)
* Demo2 is also straight forward, Just replaces main container's nginx default home page with "I am green" msg
* Apply the apply the yaml and create a service to test it
> kc expose deploy nginx-deploy-green --name init-demo --port 80
* I've created clusterIp servie to test, execed into other pod running in the cluster and access the application by hitting init-demo culsterIp service over port 80 using curl
> curl http://clusterIp:\<Service port>

Other ways of testing:
* we can create a NodePort service and access the application using any one of the worker nodes public ip and NodePort(which will be with 30-32k range)
* Create a clusterIP service and Ingress resource, ofcourse you need to have ingress controller in your cluster and then map the dns name to loadbalncer used by ingress controller loadbalancer

### References:
Justme and Opensource github repo: [here](https://github.com/justmeandopensource/kubernetes/blob/master/yamls/ingress-demo/nginx-deploy-green.yaml)