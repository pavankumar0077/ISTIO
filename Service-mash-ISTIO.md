
# SERVICE MASH
- In microservice architecture we have to do all the necessary requirements into each and every microservice
![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/2ce16ab4-57c0-4492-91aa-09a2c8b8c97a)

- Now are are re-placing it with a SINGLE PROXY with a SIDECAR CONATINER
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/97009963-a4e2-4d4c-917e-2dacfee1c120)

- These microservice communicate with each other using DATA PLANE and they communicate with SERVER SIDE COMPONENT CALLED CONTROL PLANE
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/caf3a7e8-6916-409e-a713-7e4451acf967)
- CONTROL PLANe : Manages all tth traffic into other services via PROXY. All the networking logic is abstracted from the business code
- THis approach is known as SERVICE MASH.

 ### What is Service Mesh 
 ```
It is a dedicated and configurable infrastructure layer that
handles the communication between services without
having to change the code in a microservice architecture.
```
### What is Service Mesh Responsible For ?
![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/26550b2a-f903-42a9-af25-8fedd5529eb9)

# ISTIO
- It is free and Opensource service mesh that provides an effecient way to secure connect and monitoring services
- ISTIO works with KUBERNETES
- ISTIO is supported and implemented by Google cloud, Ibm cloud, redhat adn vmware
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/55129590-75c3-4092-a0e2-164977a145c9)
- ISTIO implements the PROXY with open source high performance proxy known as ENVOY. The proxies talk with SERVER side component known as the CONTROL PLANE
- CONTROL PLALNE consists of 3 components
- 1. Citadel - Manages Certificate generation 
  2. Pilot - It helps with the service discovery
  3. Galley - It helps in the validating configuration files
- These 3 components are later combined into single daemon called ISTIOD
- Each service has a separate component in it along with ENVOY PROXY called ISTIO AGENT
- These ISTIO AGENT is responsibel for passing configuration secrets to ENVOY PROXY

### Install istio
- ``` https://istio.io/latest/docs/setup/getting-started/#download ```
- ``` https://istio.io/latest/docs/setup/install/istioctl/ ```
- ``` istioctl install --set profile=demo -y ``` To install
- ``` istioctl verify-install ``` To Verify

# Deploy a sample deployment provided by ISTIO
- FRIST WE HAVE TO ENABLE namespace for istio-injection this is for SIDECAR Containers
- ``` kubectl label namespace default istio-injection=enabled ``` value = enabled or disabled bsed on requirement
- Go to installed folder and install ``` kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml ```
-  Check the pods ``` kubectl get pods ``` To verify the deployment of pods
-  Analyze the istio ``` istioctl analyze ```

### KIALI
- Visualizing Service Mesh with KIALI
- it has WEB BASED GRAPHICALLY INTERFACE
- Visualizing and Managing SERVICE MESH
- Kiali is used for observing connecctios and microservices of itio services mesh, defining and valiating them define, validate and Observe
- It also have features like requrest routing, circut breakers, request rates, latency and more
- Common traffic and automatically generates ISTIO configurations

# INSTALL KIALI
- Go to samples folder that ISTIO given when installed
- ``` kubectl apply -f samples/addons ```
- ``` kubectl rollout status deployment/kiali -n istio-system ``` To check kiali is rollout
- ``` kubectl -n istio-system get svc kiali ``` To check kiali service is running or not in cluster
- ``` istioctl dashboard kiali ``` TO GET THE KIALI DASHBOARD

# CREATE TRAFFIC INTO OUR MESH
- ``` kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yam ``` GATEWAY TO ACCESS THE APPLICATION FROM OUTSIDE
- ``` istioctl analyze  ```
- ``` minikube ip````
- export INGRESS_HOST=$(minikube ip)
- echo $INGRESS_HOST
- ``` export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}') ``` EXPORT ingress gaterway service port
- ``` curl "http://$INGRESS_HOST:$INGRESS_PORT/productpage" ``` Now check the application
- ``` echo "http://$INGRESS_HOST:$INGRESS_PORT/productpage" ``` To get the URL to check in web
- ``` http://192.168.39.30:30238/productpage ```
- ``` while sleep 0.01;do curl -sS 'http://'"$INGRESS_HOST"':'"$INGRESS_PORT"'/productpage'\ &> /dev/null ; done ``` to generate random traffic
- Now check the KIALI DASHBOARD, GRAPH
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/891f8df0-9707-4476-a112-e2d59ed4f720)
- ``` KIALI is very helpful in IDENTIFING PROBLEMS IN SERVICE MESH, Here we gonna delete ```  kubectl delete deployments/productpage-v1 ```
- And check dashboard - ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/01f9c875-8ae1-40c7-9f52-b6d974ecc8e0)
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/6a874f40-4919-4d35-8be7-8503104260c8)

# 
