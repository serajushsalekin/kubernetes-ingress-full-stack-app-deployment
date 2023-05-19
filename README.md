# kubernetes-ingress-full-stack-app-deployment [Minikube]

This repository contains the Kubernetes configuration files for deploying a notebook application with a backend server and a frontend client. The deployment utilizes various Kubernetes resources such as secrets, config maps, deployments, services, and ingresses to create a fully functional notebook application.

### Prerequisites

Before deploying the application, ensure that you have the following prerequisites set up:

- Kubernetes cluster
- `kubectl` command-line tool
- Docker
- Ingress addon

### Deployment Architecture

The deployment consists of the following components:

- **notebook-server**: The backend server for the notebook application.
- **notebook-framework**: The frontend client for the notebook application.
- **mongodb-configmap**: A ConfigMap holding the MongoDB database configuration.
- **mongodb-secret**: A Secret containing the MongoDB username and password.
- **appsettings-secret**: A Secret containing the application settings.
- **notebook-api-service**: A Kubernetes service exposing the notebook server.
- **notebook-client-service**: A Kubernetes service exposing the notebook frontend client.
- **notebook-ingress**: An Ingress resource for routing HTTP traffic to the appropriate services.

### Configuration Files

The repository includes the following configuration files:

- [appsettings-secret.yaml](appsettings-secret.yaml): Defines a Secret resource, `appsettings`, that holds the application settings.
- [express-backend.deployment.yaml](express-backend.deployment.yaml): Deploys the notebook server as a Deployment, using the `salekin/notebook-api` Docker image. It mounts the `appsettings` Secret as a volume for configuration.
- [mongo-configmap.yaml](mongo-configmap.yaml): Defines a ConfigMap, `mongodb-configmap`, with the MongoDB database URL and name.
- [mongo-secret.yaml](mongo-secret.yaml): Defines a Secret, `mongodb-secret`, containing the MongoDB username and password.
- [notebook-ingress.yaml](notebook-ingress.yaml): Creates an Ingress resource, `notebook-ingress`, for routing HTTP traffic to the notebook API and client.
- [notebook-namespace.yaml](notebook-namespace.yaml): Creates a Namespace, `notebook`, to encapsulate all resources related to the notebook application.
- [react-frontend.deployment.yaml](react-frontend.deployment.yaml): Deploys the notebook frontend client as a Deployment, using the `salekin/notebook-client` Docker image.

### Deployment Steps

To deploy the notebook application, follow these steps:

1. Apply the `notebook-namespace.yaml` file to create the `notebook` namespace:
```shell
kubectl apply -f notebook-namespace.yaml
```
2. Create the ConfigMap and Secret resources:
```shell
kubectl apply -f mongo-configmap.yaml
kubectl apply -f mongo-secret.yaml
kubectl apply -f appsettings-secret.yaml
```
3. Deploy the notebook server and frontend client:
```shell
kubectl apply -f express-backend.deployment.yaml
kubectl apply -f react-frontend.deployment.yaml
```

4. Create the Ingress resource for external access:
```shell
kubectl apply -f notebook-ingress.yaml
```
5. Verify the deployment:
Run `kubectl get all -n notebook` to check the status of all deployed resources.

### Accessing the Application

Once the deployment is successfully completed, you can access the notebook application using the configured Ingress. The notebook API will be available at `http://<minikube-ip>/api`, and the notebook client will be available at `http://<minikube-ip>/`.

Make sure to replace `<minikube-ip>` with the actual domain or IP address where your Kubernetes cluster is running.

### Cleanup

To remove the notebook application and associated resources, run the following command:
```shell
kubectl delete namespace notebook
```
This will delete the entire `notebook` namespace and all the resources within it.

**Note:** Please exercise caution when deleting namespaces, as it will delete all resources associated with the namespace.

## Conclusion

This repository provides the necessary Kubernetes configuration files to deploy a notebook application with a backend server and a frontend client. By following the deployment steps outlined in this guide, you can easily set up the application in your Kubernetes cluster.

Feel free to explore and modify the configuration files to suit your specific requirements. Happy deploying!
