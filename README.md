# Monitoring of K8s Clusters Using Prometheus and Grafana:

![image](https://user-images.githubusercontent.com/92631457/189537252-df12749a-9a91-4eba-9a62-a516864d73f8.png)


 ## What is Prometheus?
 - Prometheus is an open source monitoring tool
 - Provides out-of-the-box monitoring capabilities for the Kubernetes container orchestration platform. It can monitor servers and databases as well.
 - Collects and stores metrics as time-series data, recording information with a timestamp 
 - It is based on pull and collects metrics from targets by scraping metrics HTTP endpoints.
 - It makes use of PromQL which is a very powerful query language to pull the time series data.
 - It can be used with Grafana for visualization of time-series data produced by prometheus.
 
   ### Features:
   - Multi-dimensional Data Model
   - PromQL
   - Service recovery
   - Time series collection via pull model
   
 
 ## What is Grafana?
 
   - Grafana is an open source visualization and analytics software. 
   - It allows you to query, visualize, alert on, and explore your metrics no matter where they are stored.
   - It uses the data collected by tools like Prometheus or Loki!
   
   ### Features:
    
   - Visualize data
   - Dynamic Dashbaords
   - Explore Metrics and Logs: For logging, Loki can be used!
   - Alerting: Very important part of monitoring, ALerts can be sent via mail, slack etc. 
   - Mixed data sources

   ### Key components:
   
   1. Prometheus server - Processes and stores metrics data
   2. Alert Manager - Sends alerts to any systems/channels
   3. Grafana - Visualize scraped data in UI

  ### Installation Method:
  
  The are are many ways you can setup Prometheus and Grafana. You can install in following ways:

  1. Create all configuration files of both Prometheus and Grafana and execute them in right order.

  2. Prometheus Operator - to simplify and automate the configuration and management of the Prometheus monitoring stack running on a Kubernetes cluster

  3. Helm chart (Recommended) - Using helm to install Prometheus Operator including Grafana.
  
  ## Why to use Helm?
  Helm is a package manager for Kubernetes. Helm simplifies the installation of all components in one command. Install using Helm is recommended as you    will not be missing any configuration steps and very efficient. 
  
  ## Prerequisites:
  - Kubernetes cluster is setup already
  - Install Helm
  - Any other EC2 instance or local machine to access the kubernetes cluster. 
  
  ## Implementation Steps:
   - Add the helm stable repo, in your repo list. Execute the below command:
   
   ```sh
   helm repo add stable https://charts.helm.sh/stable
```
![image](https://user-images.githubusercontent.com/92631457/189537756-8e2079c3-caf2-4219-a21a-722afecdcf06.png)

   - Add the prometheus helm repo:

```sh
helm repo add prometheus-community https://prometheus-community.github.io/helm-chart
```

![image](https://user-images.githubusercontent.com/92631457/189537852-6077f5e2-c1c8-43d7-ac9a-c2ac7566f7d7.png)

  - Now search prometheus-community repo, from the list of repo
  
  ![image](https://user-images.githubusercontent.com/92631457/189538104-82bf4f98-ed4b-413e-8239-a9d6a7a0b263.png)
  
  - Create Prometheus namespace
```sh
kubectl create namespace prometheus
```
  - Use the below command to install kube-prometheus-stack. The helm repo kube-stack-prometheus (formerly prometheus-operator) comes with a grafana deployment embedded.

```sh
helm install stable prometheus-community/kube-prometheus-stack -n prometheus
```
![image](https://user-images.githubusercontent.com/92631457/189538250-ee9257bf-8008-403d-bdcb-9fdb557edbe1.png)

  - Lets check if prometheus and grafana pods are running already:
  
```sh
kubectl get pods -n prometheus
```
![image](https://user-images.githubusercontent.com/92631457/189538310-58a72905-e60d-44c9-b73d-8f13d06b324d.png)

  - Check the services deployed against the expose of deployments:
  
```sh
kubectl get svc -n prometheus
```
![image](https://user-images.githubusercontent.com/92631457/189538429-fd0a0e86-cd12-4748-bbfb-861d38ac7c8d.png)

  - To acccess the UI, expose the above services, which has been highlighted in the above screenshot to Loadbalancer
  
```sh
kubectl edit svc stable-kube-prometheus-sta-prometheus -n prometheus
```
![image](https://user-images.githubusercontent.com/92631457/189538997-a267ade2-b419-4357-8b40-7d02af87d2f4.png)

  - Edit grafana service:
  
```sh
kubectl edit svc stable-grafana -n prometheus
```
![image](https://user-images.githubusercontent.com/92631457/189539037-2077cb5a-a51d-4ce8-8bbf-c8d4fddb41d1.png)

  - Verify if service is changed to LoadBalancer and also to get the Load Balancer URL.
  
```sh
kubectl get svc -n prometheus
```
 ![image](https://user-images.githubusercontent.com/92631457/189539069-9648cf54-f4bc-497a-a28e-8bcc7fd732d3.png)

  - Access the grafana and prometheus UI using the above URLs:
  
  ![image](https://user-images.githubusercontent.com/92631457/189539199-af50cc2b-54ec-4b7a-9d85-dc3d52b59865.png)
  
```sh
Username: admin
Password: prom-operator
```
  - Create Dashboard in Grafana
    - In grafana we can make various, types of dashboards as per our need!
    - For creating the dashbpard click on the import button on the left panel.
    - Enter 12740 or 3119 [kubernetes monitoring], or 6417[Pod Monitoring]  dashboard id under Grafana.com Dashboard.
    - Click ‘Load’.
    - Select ‘Prometheus’ as the endpoint under prometheus data sources drop down.
    - Click ‘Import’.
  This will show monitoring dashboard for all cluster nodes
  
  ![image](https://user-images.githubusercontent.com/92631457/189539500-d96ed131-5304-4194-88e0-57ea63c46271.png)

![image](https://user-images.githubusercontent.com/92631457/189539638-60b4648a-3a87-4959-bcae-5c06b8f62610.png)

![image](https://user-images.githubusercontent.com/92631457/189539738-dd397e36-77e6-4042-8437-6f39e0fca57f.png)



