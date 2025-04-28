## Geospatial Digital Special Collections - Basic Use

### kube.ipynb jupyter notebook

The python scripts in the kube.ipynb jupyter notebook in the [tools repository](https://github.com/Geospatial-Digital-Special-Collections/tools) provides basic functionality to spin up a data set into a postGIS kubernetes container and thus make several services available. Specifically: 

- connections to the database (for the ArcGIS data store and other applications that consume postgres connections)  
- a set of dynamic vector tiles  
- a limited set of direct apis to query the data  
- an indexed landing page for the dataset  
- [coming soon] OGC mapserver and feature server

### data ingest process

A reference for the basic flow for the extract-transform-load (ETL) process is in the [gdsc ingest process diagram](../diagrams/gdsc_ingest_process.drawio). This diagram is best opened in the online application [https://app.diagrams.net/](https://app.diagrams.net/).  

### other functionality

Other functionality in the tool set includes:

- spin down and delete the pod with a dataset
- see all running pods
- review metadata
- update all json metadata on disk

### typical workflow  

This is a basic set of steps for a common workflow with GDSC:  

- start local k8s cluster
   - start docker desktop (if installed) 
   - for kind cluster
      - open a bash terminal (mac) or powershell (windows) and navigate to the kubernetes directory. 
      - start colima ```colima start --cpu 4 --memory 4```
      - start kind cluster ```sed s+{{pwd}}+$(pwd)+g kind-config.yaml | kind create cluster --config```  
      - in another shell ```sudo cloud-provider-kind```
- open a bash terminal (mac) or powershell (windows) and navigate to the kubernetes directory.  
- type ```postgis.sh -l``` (mac) or ```postgis.ps1 -l``` (windows)  
   - use  ```postgis.sh -lk``` on kind cluster
- once the system comes up, navigate to the tools directory  
- type ```git pull origin main``` (to get the recent changes from the repository)
- type ```jupyter notebook``` and wait for the notebook to start in a web browser  
- in the web browser navigate to the ```jupyter/kube.ipynb``` notebook and double click  

At this point you are ready to do work and use the tools. When you are done follow these steps to leave a clean workspace.  

- in the jupyter notebook clear all output from the cells. This is in the __Edit__ menu under __Clear Outputs of All Cells__.
- wait for the notebook to autosave (usually about one minute)  
- from the file menu choose __Close and Shut Down Notebook__, this will close the browser tab with the notebook  
- find the browser tab with the jupyter file system interface and select __Shutdown__ from the __File__ menu. 
- return to the bash terminal (mac) or powershell (windows) and navigate to the kubernetes directory  
- type ```cleanup.sh -l``` (mac) or ```cleanup.ps1 -l``` (windows)  
- once the script has finished delete the k8s cluster
   - quit docker desktop if running
   - for kind cluster
      - ```kind delete cluster```
      - ```colima stop``` 
      - don't forget to kill the cloud-provider-kind process

### debugging the jupyter notebook on localhost

_NOTE: none of the notes below apply to the kmaster.idsc.miami.edu control plane._

As most of the code is still a work in progress the tools will sometimes fail. Most often the fail will take place when you run the __main()__ function to ETL data. What stage the ETL process fails will determine where to look for the failure.

A good first check for general level failure is to open a shel and type ```kubectl get pods -n gdsc```. This will list all running pods. If you do not see something that ends similar to the below, simply run the cleanup script in the kubernetes directory and start again ...

If you have run the __main()__ function and it failed at

__Running scripts for ...__

This means the ETL failed on the __osgeo__ container in the __postgis_osgeo__ pod.

If you have run the __main()__ function and it failed at

__Running SQL scripts for ...__

This means the ETL failed on the __postgis__ container in the __postgis_osgeo__ pod.
