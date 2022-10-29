## Installing Prometheus and Grafana

### Steps to Install Prometheus:

1. helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
1. helm repo update
1. helm install prometheus prometheus-community/prometheus
1. ClusterIp service will be created. So create another service of type Nodeport to access the UI from outside cluster
```
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext
```
5. if you are using eks cluster, make sure to update security groups to allow traffic from every where
1. Now prometheus UI can be accessed using any node public ip and Nodeport


### Steps to install Grafana:

1. helm repo add grafana https://grafana.github.io/helm-charts
1. helm repo update
1. helm install grafana grafana/grafana
1. ClusterIp service will be created. So create another service of type Nodeport to access the UI from outside cluster
```
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext
```
5. if you are using eks cluster, make sure to update security groups to allow traffic from every where
1. Now prometheus UI can be accessed using any node public ip and Nodeport


1. To get user name and password in Grafana:
```
kubectl get secret --namespace default grafana -o yaml
echo "RkpwY21aTFNXRDVJN3Z4RWFFUjlibkV1SDBDbnFBendadmc0bmROZQ==" | openssl base64 -d ; echo

or 

kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
8. Use dashboard from grafana labs by importing(You can use just the id i.e 6417)



Grafana playlist: [Thetips4you](https://www.youtube.com/playlist?list=PLVx1qovxj-akOGKSoQ5sGc5ZRfH8Tpnow)

Prometheus Playlist: [Thetips4you](https://www.youtube.com/playlist?list=PLVx1qovxj-anCTn6um3BDsoHnIr0O2tz3)
