## Geospatial Digital Special Collections - Installation Notes

### For a development machine  

You will need a minimum of:  

- python (see notes below)<sup>1</sup>  
- jupyter notebook<sup>1</sup>  
- [docker desktop](https://www.docker.com/products/personal/) (personal version)  
- git (command line preferred) and a github account<sup>2</sup>  

<sup>1</sup> both can be installed with either conda or [miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html) (preferred)  
<sup>2</sup> already on most windows machines and for macOS install the XCode command line tools (```$ xcode-select --install```)  

### Set up python environment  

On a mac (in terminal)  
```
$ conda create --name gdsc python=3.9
$ source activate gdsc
$ pip install kubernetes boxsdk "boxsdk[jwt]" python-dotenv pyyaml pandas openpyxl 
# postgis and the ipykernel are used in dev but not production
$ pip install psycopg2-binary postgis
$ python -m pip install ipykernel --user
$ python -m ipykernel install --user --name gdsc --display-name "gdsc"
```

On a pc (in powershell)  
```
> conda create --name gdsc python=3.9
> conda activate gdsc
> pip install kubernetes boxsdk "boxsdk[jwt]" python-dotenv pyyaml pandas openpyxl 
# postgis and the ipykernel are used in dev but not production
> pip install psycopg2-binary postgis
> python -m pip install ipykernel --user
> python -m ipykernel install --user --name gdsc --display-name "gdsc"
```

### Additional tools that make life easier:  

- [QGIS](https://qgis.org/en/site/forusers/download.html)  
- [PGAdmin](https://www.pgadmin.org/download/)  
- A good text editor like [sublime](https://www.sublimetext.com/3) or [notepad++](https://notepad-plus-plus.org/downloads/)  
- [postgres with postGIS extensions](https://postgis.net/documentation/getting_started/) (see the installing section)  

### Post-install configuration  

- Create a project directory and clone the following two repositories as sub-directories  
   - https://github.com/Geospatial-Digital-Special-Collections/tools   
   - https://github.com/Geospatial-Digital-Special-Collections/kubernetes  
- From the provided zipfile, get the set of credentials, encryption keys, and base disk images and place them in the following directories in the respective repositories as follows:  

```
.  
+-- tools  
|   +-- config  
|   |   +-- keys  
|   |   |   box.json
|   |   |   census.env  
|   |   +-- kubernetes  
|   |   |   gdsc-controller-token.yaml
|   |   |   postgis-secret.yaml  
|   |   +-- pki 
|   |   |   client_cert.pem  
|   |   |   client_key.pem  
|   |   |   server_cert.pem  
+-- kubernetes  
|   +-- _localdata  
|   |   data/  
|   |   solr/  
|   |   tileserv/  
|   +-- secrets  
|   |   docker-hub-secret.yaml  
|   |   gdsc-controller-token.yaml  
|   |   local-gdsc-controller-token.yaml  
|   |   postgis-secret.yaml
```  

### Testing the install  

- Start docker desktop  
- To start the kubernetes stack on your local machine, in shell (mac terminal or windows powershell) navigate to your kubernetes repository and type:  

On a mac (in terminal)  
```$ postgis.sh -l```  

On a pc (in powershell)  
```> postgis.ps1 -l```  

- To use the management and curation tools, start a second shell and navigate to the tools repository and type:  

On a mac (in terminal)  
```$ jupyter notebook```  

On a pc (in powershell)  
```> jupyter notebook```  

- In the notebook navigate to ```scripts/jupyter/kube.ipynb```, open and run all cells up until and including the cell with ```api = api_config()```. If this completes with no errors, your configuration is correct.  
- Continue to run all cells up until the md cell __main(): call from here__, then look below the header __working utils__ and run the cell with the following code:

```
# list all pods
pods = get_pods(api,pg)
print (json_dumps(pods, indent=2))
```

You should see something like:  

```
{
  "data": {},
  "service": {
    "flask": "flask-gdsc-97c65c94c-b98gk",
    "osgeo": "postgis-osgeo-5fcbd48b7f-jrhk7",
    "proxy": "postgis-proxy-5d57f77dc8-pcbjq",
    "tileserv": "postgis-tileserv-7b747d8cc4-gxtnh",
    "postgrest": "postgrest-5656d4c8b5-x7sl9",
    "solr": "solr-8687cbd46c-csx27"
  }
}
```

- Under __working utils__ find and run the cell with the code:  

```
meta = get_meta('Metadata',1)
fields = get_meta('MetadataFields',2)
```

If the code runs with no errors, you are now ready to start using the toolset (see [basic use](gdsc_use.md)).  