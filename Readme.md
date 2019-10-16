#### K8s spike for service type external name on Minikube

This spike explains how to create a 'ExternalName' type kubernetes service on Minikube.

#### Overview
We will configure, 

1. `httpbin.org` behind a kubernetes service of externalname type. 
2. An ingress controller routing to the newly created service.
3. Map hostname of the ingress controller to minikube cluster ip.
4. Access the ingress to access `httpbin.org`

#### Steps

(1) Start Minikube

`minikube start`

(2) Enable Ingress Addon

`minikube addons enable ingress`

(3) Verify Ingress Addon is enabled, by listing the pods on `kube-system` namespace. You should see the `nginx-ingress-controller`.

`kubectl get pods -n kube-system | grep 'nginx-ingress'`

_Note:_ Nginx is used for creating ingress controllers on Minikube 

(4) Apply the service.yaml in this repo to create the kubernetes service and ingress

`kubectl apply -f service.yaml`

_Note_ : Here we provide the ingress with hostname `k8s-external-name-svc-spike.io`. 

(5) Get your cluster IP and update `/etc/hosts` to route requests to `k8s-external-name-svc-spike.io`. 
Add an entry similar to the one below.

`192.168.99.121  k8s-external-name-svc-spike.io`
  
_Note_: Your cluster IP could change. You can obtain your minikube cluster ip using `kubectl cluster-info`

(6) Access `k8s-external-name-svc-spike.io` and you should see httpbin.org landing page.

`curl k8s-external-name-svc-spike` (or) `open http://k8s-external-name-svc-spike.io`

#### Cleanup
1. Delete the entry from `/etc/hosts`
2. Delete your k8s resources
`kubectl delete -f service.yaml`

🏁 Happy accessing external DNS via your k8s cluster.

_Coming soon ... https_