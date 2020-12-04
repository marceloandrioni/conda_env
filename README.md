# Download
Download [Anaconda](https://www.anaconda.com/products/individual#Downloads) and install (tested with `Anaconda3-2020.11`)

# Activate `base` environment

## Linux
Open a terminal and run `eval "$(/home/eani/anaconda3/bin/conda shell.bash hook)"`

## Windows
Just open `Anaconda Powershell Prompt (anaconda3)`

# Configure .condarc
Create the file with a text editor if it doesn't exist or just run `conda config` to create an empty file.

## Linux
`/home/<user>/.condarc`

## Windows
`C:\Users\<user>\.condarc`

## Content:
```
ssl_verify: false

channels:
  - defaults
  - conda-forge
  - fbriol   # FES Tide https://bitbucket.org/cnes_aviso/fes/src/master/

## Proxy settings (uncomment and configure the 3 lines bellow if behind a proxy)
# proxy_servers:
#     http: http://username:password@server:port
#     https: https://username:password@server:port
```

# New envs

It is best not to change the `base` environment (env) and always create a new env when trying new packages. The new env can be created "by hand", e.g.:

`conda create -n <env_name> python=3.7
spyder
xarray dask netcdf4 cfgrib eccodes
metpy cdsapi
unidecode humanize termcolor aniso8601 tenacity
cx_oracle flask
cartopy seaborn windrose plotly folium cmocean python-docx xlsxwriter xlrd openpyxl geopy alphashape`

or imported from a `.yml` file (itself created with `conda env export --no-build > environment.yml`), e.g.:

`conda env create -f environment.yml`

but remembering to edit the `name` and `prefix` variables at the first and last lines to the desired values.

After creation, the new env must be activated, e.g.:

`conda activate <env_name>`


## Notes:

### cx_oracle:
To connect to Oracle databases using the `cx_oracle` library, the user also needs the Oracle library files ([Linux](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html)/[Windows](https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html)).

#### Linux

Dowload and extract:
- instantclient-basic-linux.x64-19.9.0.0.0dbru.zip
- instantclient-sqlplus-linux.x64-19.9.0.0.0dbru.zip

Set the paths:
```
export LD_LIBRARY_PATH=/some/dir/instantclient_19_9:$LD_LIBRARY_PATH
export PATH=/some/dir/instantclient_19_9:$PATH
export ORACLE_HOME=/some/dir/instantclient_19_9
```

#### Windows
???


### xESMF

Following [this discussion](https://github.com/JiaweiZhuang/xESMF/issues/47), `xesmf` hast to be installed after `esmpy` to work it correctly.

`conda install esmpy`

`conda install xesmf`

### CentOS 6.5
In older systems (e.g. CentoOS 6.5), packages `spyder`, `jupyter` and `pyferret` cause errors like:
```
*** glibc detected *** python: double free or corruption (!prev)
...
(core dumped)
```
So if these packages are needed, it is best to install then in a dedicated env or install older versions.

### env to test CF Conventions
`conda create -n cfconv cfchecker compliance-checker`

### Others
List the available envs:

`conda env list`

Remove an env:

`conda remove --name <env_name> --all`

# pycodestyle
Ignore some errors and warnings in the code style:

## Linux 
`/home/<user>/.config/pycodestyle`

## Windows
???

## Content:
```
[pycodestyle]
ignore = E501, E722, W503, W605
```
