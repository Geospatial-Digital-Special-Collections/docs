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

- start docker desktop  
- open a bash terminal (mac) or powershell (windows) and navigate to the kubernetes directory.  
- type ```postgis.sh -l``` (mac) or ```postgis.ps1 -l``` (windows)   
- once the system comes up, navigate to the tools directory  
- type ```jupyter notebook``` and wait for the notebook to start in a web browser  
- in the web browser navigate to the ```jupyter/kube.ipynb``` notebook and double click  

At this point you are ready to do work and use the tools. When you are done follow these steps to leave a clean workspace.  

- in the jupyter notebook clear all output from the cells. This is usually in the cells menu or the edit menu (depending on the jupyter version)  
- wait for the notebook to autosave (usually about one minute)  
- from the file menu choose __close and halt__, this will close the browser tab with the notebook  
- find the browser tab with the jupyter file system interface and find __logout__ or __shutdown__ (again depending on the jupyter version)  
- return to the bash terminal (mac) or powershell (windows) and navigate to the kubernetes directory  
- type ```cleanup.sh -l``` (mac) or ```cleanup.ps1 -l``` (windows)  
- once the script has finished quit docker desktop  

