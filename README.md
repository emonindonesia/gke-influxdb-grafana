## InfluxDB with Grafana on Google Kubernetes Engine by [![N|Solid](https://emonindonesia.com/images/logo.png)](https://emonindonesia.com)
In this tutorial we explain how to install InfluxDB and Grafana on the kubernetes engine of Google.

## Prerequisites
- Google Cloud Project linked to a billing account. If you are a first time user of Googles Cloud services you will get $300 credit on your new billing account.
### 1. Create a kubernetes cluster on GCP
- From the side menu choose Kubernetes Engine --> clusters
- Click on Create Cluster button
 ### ![image](/screenshots/cluster_button.png)
- Choose your first cluster option
- Modify the number of nodes to 1 and the machine type to 1 vCPU
 ### ![image](/screenshots/create_cluster.png)
- Click create
- Wait few minutes until your cluster is up and running 

### 2. Connect to the cluster using Cloud Shell
- Click on the connect button
 ### ![image](/screenshots/connect_button.png)
- Choose Run in Cloud shell
 ### ![image](/screenshots/connect_cloudShell.png)
- Wait until the cloud shell is connected and execute the already written command

### 3. Cloning the repository on the cloud shell
- Run the following commands
```bash
git clone "https://github.com/Balipeace/influxDB-grafana-gke"
cd influxDB-grafana-gke/configs/
```
### 4. Running the kubernetes config files
- Now we want to open the pods and services of influxDB and grafana
- Run the following commands
```bash
kubectl create -f influxdb.yaml
kubectl create -f grafana.yaml
kubectl create -f influxdb-internal-service.yaml
kubectl create -f influxdb-external-service.yaml
kubectl create -f grafana-service.yaml
```
- Open Workloads in the side menu to check that the status of grafana and influxDB in OK
 ### ![image](/screenshots/workloads.png)
- Open Storage in the side menu to check that the phase of the persistent volume claim is Bound
 ### ![image](/screenshots/storage.png)
- Open Services in the side menu refresh until all the services are OK

### 5. Create a database in influxDB
- SSH with influxDB container
```bash
# Get the name of the influxdb container
kubectl get pods
```
 ### ![image](/screenshots/get_pods.png)
```bash
# Replace <NAME OF THE CONTAINER>
kubectl exec -it <NAME OF THE CONTAINER> influx
```
- Create emon_db database
```bash
create database emon_db
```
- Exit the container 
```bash
exit
```

- Install influxdb module
  
  ```bash
  pip install influxdb
  ```

### 7. Adding a dashboard on Grafana for real-time analytics
- In Kubernetes Engine side menu click on Services
- Click on the url in grafana-external-service row under the endpoint column
  - Username: admin
  - Password: admin
- Click Add Datasource
 ### ![image](/screenshots/add_datasource.png)
  - Enter the url http://<CLUSTER IP>:8086
    - To get cluster ip
      - From the cloud shell run :
  
    ```bash
    kubectl get services influxdb-external-service
    ```
  - Enter a name
  - Enter Database : solar_db
   ### ![image](/screenshots/add_datasource1.png)
 - Click save and test
 - then click back
 - On the side menu Click Dashboards --> Manage
 - Click on import
  ### ![image](/screenshots/import_dashboard.png)
 - Now on the repository on your local machine go to the dashboard folder and copy the content of the json file
 - Paste it on Or paste json field
  ### ![image](/screenshots/paste_json.png)
 - Click load
  ### ![image](/screenshots/importing_dashboard.png)
 - Select solar and then click import 
 
## Congrats You should be finished
 ### ![image](/screenshots/Finished.png)
