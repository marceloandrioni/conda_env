# conda environment

in `~/.condarc`
```
ssl_verify: false

channels:
  - defaults
  - conda-forge
  - fbriol     # FES Tide https://bitbucket.org/cnes_aviso/fes/src/master/

# Proxy settings (if behind a proxy): http://[username]:[password]@[server]:[port] 
# proxy_servers:
#     http: http://usr:psw@proxyserver.com:8080
#     https: https://usr:psw@proxyserver.com:8080
```

## main work env
```
conda create -n work python=3.7 \
    spyder \
    xarray dask netcdf4 cfgrib eccodes \
    metpy cdsapi esmpy \
    unidecode humanize termcolor aniso8601 tenacity \
    cx_oracle flask \
    cartopy seaborn windrose plotly folium cmocean \
    python-docx xlsxwriter xlrd openpyxl \
    geopy alphashape
```

`conda activate work`

or create the env from the environment file:

`conda env create -f environment.yml`

but remembering to edit the `prefix` variable at the last line to reflect the actual user path.

### Notes:

#### CentOS 6.5
I don't know why, but in older systems (e.g. CentoOS 6.5), packages `spyder`, `jupyter` and `pyferret` cause errors like:
```
*** glibc detected *** python: double free or corruption (!prev)
...
(core dumped)
```
So if these packages are needed, it is best to install then in a dedicated env or install olders versions.

#### xESMF

Following [this discussion](https://github.com/JiaweiZhuang/xESMF/issues/47), `xesmf` hast to be installed after `esmpy` to work it correctly.

`conda install xesmf`

#### cx_oracle:
cx_oracle also needs the Oracle library [files](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html).

Dowload and extract:
- instantclient-basic-linux.x64-19.9.0.0.0dbru.zip
- instantclient-sqlplus-linux.x64-19.9.0.0.0dbru.zip

Add to Path:
```
export LD_LIBRARY_PATH=/some/dir/instantclient_19_9:$LD_LIBRARY_PATH
export PATH=/some/dir/instantclient_19_9:$PATH
export ORACLE_HOME=/some/dir/instantclient_19_9
```

## operational env
same as work but without spyder

## env to test CF Conventions (incompatible with other packages)
`conda create -n cfconv cfchecker compliance-checker`

## pycodestyle
in `~/.config/pycodestyle`

```
[pycodestyle]
ignore = E501, E722, W503, W605
```
## to remove an env
```
conda remove --name env_name --all
```
