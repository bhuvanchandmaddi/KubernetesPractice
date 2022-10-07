## Sequence of steps:
1. Create k8 cluster(I've created EKS cluster using ek)
> eksctl create cluster --name testcluster --version 1.22 --region ap-south-1 --nodegroup-name standard-workers --node-type t2.small --nodes 3 --nodes-min 3 --nodes-max 3

1. Deploy Ingress Nginx controller([Docs here](https://kubernetes.github.io/ingress-nginx/deploy/#quick-start))
> helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace

## Deployment without TLS certs
1.Deploy sample-app.yaml file(Creates deployment with 3 replicas,ClusterIP service and Ingress rule)
>kc apply -f sample-app.yaml
1. Map the Route53 DNS record **test.bhuvandevops.xyz** to Loadbalancer of Ingress Controller
1.Application will be accessible but when you verify the certificate by acessing over https, you will observe that is fale certificate provided by nginx ingress controller.Even browser will prompt the msg not secure

[!Click here for image](https://blogger.googleusercontent.com/img/a/AVvXsEgegYZS4vZnIAMSzE-o-vQ0929R98d1DhUObBTd-aCDEUvNBmw2R_iCoqIoOdiyaWelEA0qkBMgMFlGTxBca-bpwkbe7s6uORH5Rbgr6WNjCq8aiNYNTfA6LW3j2hgmbq88rz36m39Jraqws8rc4gQZi21G8DymttLIwR83Xh6g3pPjD1_e0aVbSwWPoA=w945-h600-p-k-no-nu)
## Deployment with TLS certs provided by cert-manager

* Install cert-manger([Docs here](https://cert-manager.io/docs/installation/helm/#steps))

>helm repo add jetstack https://charts.jetstack.io <br />
helm repo update <br />
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.crds.yaml <br />
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.9.1 

* Apply cert-issuer.yaml file(Update the namespace)
* Apply cetificate.yaml file.This will create secret file, the tls.crt(ssl certificate )and tls.key(private key)

[Image here](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKabh7z-KGdqDhWqjSfmNdHMVphR8Ti8MBYoWCw_0q6nX3oVlR4dCwT7MI51OVfoCn11kMZeYWLLzSQZFOH5aAGY3cGWeueeDnbETu0B4PZR4Jb2liIO7hnJdZXgusH-e1yxDNlCLRXWzIJDuGRj8zHul7Oq2_fsIvia7MNaUarPkski1U1pJACaB3Bw/s320/Capture.JPG)
* Delete and re-create ingress using ing-with-tls.yaml file
* Now you should be able to access application using **https://test.bhuvandevops.xyz** and if you verify the cetificate it should be issued by letencrypt with a little lock which says your application is secured with TLS encyption

[Image here](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiTwnWzkYgpf0SJdXax31PDMw5zASW_Pr6FOAbd5BlXAP3Fc6GSPHDAwiX3o74YU7QRMKT83JW7uNo--ocBXKT-wc_kQY9eCBtidEemuqWXFV7tozSnr3Uf0JpDVoS-u8ODlDQmZ6jw1sjwb40Jj2sZF7PYlmV5V8zr8jXfpXvRxatCLCdsefvzgV74Zg/s1234/Capture.JPG)

**Notes:**

* When you want do more testing activities, it is good to choose staging over production in letsencrpt because production has rate limits([Doc here](https://letsencrypt.org/docs/rate-limits/))
* When you want to create deployments in other namespace then just copy the secret and rename namespace accordingly
* In the above exampl, we used **http01** in ClusterIssuer, which means we used it for a single domain.If you have multiple domains use  dnsNames([refer this site](https://towardsdatascience.com/ssl-tls-for-your-kubernetes-cluster-with-cert-manager-3db24338f17)) 

## References:
* video: [here](https://www.youtube.com/watch?v=deLW2h1RGz0&t=1639s)
* Gitlab repo: [here](https://gitlab.com/cloud-versity/rancher-k3s-first-steps/-/tree/main/Certificate%20Manager%20(TLS%20Demo))
* Blogs: [Blog1](https://towardsdatascience.com/ssl-tls-for-your-kubernetes-cluster-with-cert-manager-3db24338f17)

