
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

# TRAFFIC MANAGEMENT
![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/e0e1f0b9-d563-4218-99a1-930f27da29a5)
### GATEWAYs in ISTIO
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/172e9d81-dc38-4b91-b46b-4d786b547daf)
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/e977f918-222a-4e94-a613-17d6bc6a0d3c)
- This kubernetes ingress routed to the product page service and users can access by external world
- Gateways are load balancer that sits at the edge, they manages the inbound and outbound traffic to service mesh
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/32b2c995-d75c-4ed3-b847-1894e89d414b)
- This is the RECOMMEDED approach such as compare to KUBERNETES INGRESS, ISTIO GATEWAY
- iSTIO GATWAY CONTROLLERS ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/2858414f-2a87-469f-97f0-33572bff6fd2)
- INGRESS -- INBOUD SERVIES AND EGRESS -- OUTBOUND SERVIVES
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/a5a31e59-e161-42e5-89fc-8ac5813db1fc)
- kUBERNETS USES CONTROLLERS LIKE NGINX
- ISTIO DEPLOY INGRESS GATWAYS USING ENVOY PROXIES
- wE CAN ALSO HAVE OWN CUSTOM INGRESS GATEWAYS ALSO
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/dba981d7-b07a-46e1-8e6c-48d027eedbc4)
- Here we have created ingress gatway to access the traffic and route to the bookinfo-gateway and ot the applcaiton
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/69b7ab44-31e6-4608-9d21-905558d3e327)
- If we have more ingress controllers like custom then use SELECTOR lable
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/aed467a8-8f36-461a-933b-9938afaf957a)
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/256d463d-0cea-418a-8da7-13c2b2a0abd0)
- ``` kubectl apply -f bookinfo-gateway.yaml ``` To book info gateway
- ``` kubectl get gateway ``` Check
- ``` kubectl describe gateway bookinfo-gateway```

# VIRTUAL SERVICES IN ISTIO
- How to know if the END-USER enter the url to which page should be shown.
- All Routing rules are configured through VIRTUAL SERVICES. It defines a set of routing rules for traffic comming from the ingress gateway to the SERVICE MESH
- When Virtual servies is created ISTIO CONTROL PLANE applies the new configuration to all the sidecar proxies
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/b10b67c7-b6f3-4461-9898-e8ef42a8f846)

 ### create virtual services
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/fbc145d8-ca4e-497c-a595-9a285d0244f1)
- **WITH KUBERNETES**
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/3d43f7fa-615b-4cd6-aa03-030f0caddfa7)
- Here we have created 3 deployments with reviews-v1,v2 and v3 --> AND 1 with 3 replica and rest 2 with 1 - 1
- But we want to 3 replicas for reviews-v3 now, Becoz users found it is useful and users wants to use reviews-v3 now
- So we dicided to ROUTE ALL TRAFFIC TO reviews-v3 now
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/8e33c9ea-b1f9-4780-ba08-c3ed171a8bb1)
- THIS PROCESS IS CALLED AB TEST. without any downtime or distrution to the service. (Identifing the traffic and making it scale)
- THIS WHOLE PROCESS IS KUBERNETES NATIVE WAY OF PERFORMING traffic
- This is not possible it uses like 75% and 25% ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/a15c12f3-c639-45a8-b5a0-471debd212df)
- with istio and virtual services we can CREATE VIRTUAL SERVICE INSTEAD FO SERVICE AND WE CAN SET WEIGHT IN THIS VIRUTAL SERVICES LIKE 99% AND 1%
- ![image](https://github.com/pavankumar0077/ISTIO/assets/40380941/30aec163-1ec1-4a2b-9306-6a2cdf5d99de)

# DESTINATION RUlES
- Subsets are defined in destination rules
-


