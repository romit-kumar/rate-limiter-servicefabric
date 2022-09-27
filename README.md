# rate-limiter-servicefabric
This is sample application which demonstrates how envoy can be used as a rate limiting service in service fabric managed cluester.
![image](https://user-images.githubusercontent.com/113981567/192590071-5a01ec66-95bb-450e-be87-ea960c3209d7.png)

## Build and push weatherservice container to ACR.

1. Clone the repo.
1. Open PowerShell.
1. Change the directory to cloned rate-limiter-servicefabric directory.
1. Login into container registry.
```
docker login {azurecontainerhost}.azurecr.io -u {azurecontainerhost} -p {authkey}
```
1. Docker build the weather api project
```
docker build -f ./weatherapi/Dockerfile -t weatherservice:1.0 .
```
![image](https://user-images.githubusercontent.com/113981567/192615867-a8b87ce3-9179-44ba-a1da-bd0dfbe92d7b.png)

1. Docker tag and push

```
docker tag weatherservice:1.0 {azurecontainerhost}.azurecr.io/samples/weatherservice:1.0
docker push {azurecontainerhost}.azurecr.io/samples/weatherservice:1.0
```
![image](https://user-images.githubusercontent.com/113981567/192616440-d09ab4cf-f61f-498e-902c-2d1d6e2b944e.png)

## Build and push envoyservice container to ACR.

1. Envoy config which should be used is copied to base container image which is specified in envoy/DockerFile.
![image](https://user-images.githubusercontent.com/113981567/192617452-b41d5b58-2d7e-48f1-8891-0fbd9c37e0bc.png)

 Any [Envoy Filters](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/http_filters) can be used depending upon the requirement.
 
 1. Docker build.
 ```
 docker build -f ./envoy/Dockerfile -t envoyservice:1.0 .
 ```
 
 1. Docker tag and push.
 ```
 docker tag envoyservice:1.0 {azurecontainerhost}.azurecr.io/samples/envoyservice:1.0
docker push {azurecontainerhost}.azurecr.io/samples/envoyservice:1.0

 ```
 
 ## Build and publish the service fabric application to service fabric managed cluster hosted on azure.
 
 1. Open ratelimiterservicefarib.sln file in Visual Studio
 ![image](https://user-images.githubusercontent.com/113981567/192621113-5ec12822-e665-4f51-ad38-312f32563321.png)
1. Update weatherservicePkg > ServiceManifest > ImageName with image path in ACR
![image](https://user-images.githubusercontent.com/113981567/192621675-71c886b8-f907-4a46-937d-ddb02de35f35.png)
1. Update envoyservicePkg > ServiceManifest > ImageName with image path in ACR
![image](https://user-images.githubusercontent.com/113981567/192621952-54297311-638d-4128-b25d-93a2b3a3eeb6.png)
1. Update ACR account name and password in Application Manifest > Repository Credentials
![image](https://user-images.githubusercontent.com/113981567/192622607-257e12aa-f6ef-4525-bc23-b991e3152160.png)
1. Build the ratelimitersf project.
1. Publish the ratelimitersf project to the service fabric managed cluster.
![image](https://user-images.githubusercontent.com/113981567/192624042-4343eb4e-0f7f-421f-8777-768c775f9d69.png)



 
