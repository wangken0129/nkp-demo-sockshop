# GitOps DKP SockShop demo

This tutorial is based on Waveworks Microservice Demo <https://microservices-demo.github.io/>.

## Prerequirements

* Access to Kommander/Konvoy connected environment
  * Kommander URL
  * Administrator User ID
  * Administrator Password
* `kubectl` configured on local desktop or some other host to access the cluster
* Access to the "dkp-demo-sockshop" GitHub Repository (this one)
  * <https://github.com/kisahm/dkp-demo-sockshop/>

## Log In to  Kommander with Provided Credentials at Provided URL

Kommander delivers visibility and management of any Kubernetes cluster, whether on-prem or cloud, regardless of distribution being used. Organizations can gain better control over existing deployments without service interruptions and create standardization around how new clusters are configured and used.

1. In a supported browser, enter the provided URL for your Kommander Cluster
2. Using the authentication mechanism specified in your instructions, log into the Kommander Cluster.
3. This will bring you to the "Home Dashboard" screen in the "Global" Workspace

> For more information on Kommander, please see the following link:
> <https://docs.d2iq.com/dkp/kommander/latest/introducing-kommander/>

## Navigate to _Default Workspace_

Workspaces give you the flexibility to represent your organization in a way that makes sense for your team and configuration.  For example, you can create separate workspaces for each department, product, or business function. Each workspace manages their own clusters and role-based permissions, while retaining an overall organization-level view of all clusters in operation.

1. In the upper-left hand corner of the screen (just to the right of the _Kommander_ label) click the pull-down menu and select _Default Workspace_

> For more information on Workspaces in Kommander, please see the following link:<br>
> <https://docs.d2iq.com/dkp/kommander/latest/workspaces/>

## Create a Project for your Workload

A Project, in this context, refers to a central location (Kommander) that hosts and pushes common configurations to all Kubernetes clusters, or a pre-defined subset group under Kommander management. That pre-defined subset group of Kubernetes clusters is called a Project.

1. On the left-hand "I-Frame" click _Projects_.  This will take you to the Projects for the selected Workspace.  
2. In the middle of the screen, click _Create Project_ to create a new project.
3. In the _Create Project_ screen, complete the following entries.

* Project Name: `sockshop`
* ID/Namespace: `sockshop` (this field will be revealed once you have entered the "Project Name" text box above.)
* Description: `whatever you want`
* In the _Clusters_ selection of the page, click _Manually Select Clusters_ and place your cursor in the _Select Cluster_ text box
* Select `local-cluster` to add this cluster to the below list of selected clusters.
* Click the _Create_ button in the upper right corner of the screen to create the project.

If everything is done properly, you will see a popup menu with "Success" and a green checkmark.  

* Click _Continue to Project_ in the popup window.

> For more information on Projects in Kommander, please see the following link:<br>
> <https://docs.d2iq.com/dkp/kommander/latest/projects/>

## Create Microservice "Project Deployment"

Kommander Projects can be configured with GitOps based Continuous Deployments for federation of your Applications to associated clusters of the project.

1. In the _Projects > sockshop_ page click the _Continuous Deployment (CD)_ tab

2. Click the _Add GitOps Source_ button in the middle of the screen.

3. Fill in the _Create GitOps Source_ fields as shown below:

* ID (name): `sockshop-microservices`
* Repository URL: `https://github.com/kisahm/dkp-demo-sockshop/`
* Branch/Tag: `master`
* Path:
* Primary Git Secret: `None`

4. Click `Save` in the upper right hand of the page to save the GitOps Source

5. Watch/ensure that your pod deployments come up by entering the following kubectl command:

```
watch kubectl get pods -n sockshop
```

When you see a screen that looks like the following, your application has successfully been deployed:
```
kubectl get pods -n sockshop
NAME                            READY   STATUS    RESTARTS   AGE
carts-b4d4ffb5c-t8nxj           1/1     Running   0          4m41s
carts-db-6c6c68b747-dnrx2       1/1     Running   0          4m41s
catalogue-759cc6b86-wlq8n       1/1     Running   0          4m41s
catalogue-db-96f6f6b4c-kblrp    1/1     Running   0          4m41s
front-end-5c89db9f57-x56nk      1/1     Running   0          4m41s
orders-7664c64d75-pqvw8         1/1     Running   0          4m41s
orders-db-659949975f-5z5dn      1/1     Running   0          4m41s
payment-7bcdbf45c9-9kc65        1/1     Running   0          4m41s
queue-master-5f6d6d4796-gfvdw   1/1     Running   0          4m41s
rabbitmq-5bcbb547d7-f6pr9       2/2     Running   0          4m40s
session-db-7cf97f8d4f-8n797     1/1     Running   0          4m40s
shipping-7f7999ffb7-285bx       1/1     Running   0          4m40s
user-68df64db9c-hs7jl           1/1     Running   0          4m39s
user-db-6df7444fc-6sjd9         1/1     Running   0          4m39s
```

## Open sockshop Application UI

1. To determine the public facing endpoint for the sockshop frontend application, execute the following command:

```
kubectl get svc front-end -n sockshop
```

You will see a response similar to the following:
```
NAME        TYPE           CLUSTER-IP     EXTERNAL-IP                                                              PORT(S)        AGE
front-end   LoadBalancer   10.104.61.65   a6fefc30017ef4eb3bcd3d620f37621d-533056230.us-east-1.elb.amazonaws.com   80:31285/TCP   5m39s
```

2. Copy the External IP (similar to ``a6fefc30017ef4eb3bcd3d620f37621d-533056230.us-east-1.elb.amazonaws.com), paste it into a new browser window and prepend it with `http://` if the page does not automatically load.
