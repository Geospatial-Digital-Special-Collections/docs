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
- a good text editor like [sublime](https://www.sublimetext.com/3) or [notepad++](https://notepad-plus-plus.org/downloads/)
- [postgres with postGIS extensions](https://postgis.net/documentation/getting_started/) (see the installing section)