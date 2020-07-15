# conda environment

in `~/.condarc`
```
ssl_verify: false

channels:
    - fbriol   # FES Tide https://bitbucket.org/cnes_aviso/fes/src/master/
    - conda-forge
    - defaults

# Proxy settings (if behind a proxy): http://[username]:[password]@[server]:[port] 
# proxy_servers:
#     http: http://usr:psw@proxyserver.com:8080
#     https: https://usr:psw@proxyserver.com:8080
```

## main work env
```
conda create -n work python=3.7 \
    spyder ipython jupyter \
    psutil requests unidecode humanize termcolor colorama aniso8601 tenacity \
    memory_profiler cx_oracle flask \
    pandas seaborn geopandas descartes geopy alphashape \
    python-docx xlsxwriter xlrd openpyxl \
    dask netcdf4 xarray cfgrib eccodes cdsapi pyferret ferret_datasets pydap metpy xesmf \
    matplotlib windrose cartopy shapely fiona bokeh plotly folium \
    fes
```

### Notes:

#### cx_oracle:
cx_oracle also needs the Oracle library [files](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html).

Dowload and extract:
- instantclient-basic-linux.x64-12.1.0.2.0.zip
- instantclient-sqlplus-linux.x64-12.1.0.2.0.zip

Add to Path:
```
export LD_LIBRARY_PATH=/some/dir/instantclient_12_1/:$LD_LIBRARY_PATH
export PATH=/some/dir/instantclient_12_1/:$PATH
export ORACLE_HOME=/some/dir/instantclient_12_1/
```

#### fiona
fiona is needed because `cartopy.io.shapereader.Reader` is faster with fiona installed. It also avoids errors like
```
UnicodeDecodeError: 'ascii' codec can't decode byte 0xc7 in position 5: ordinal not in range(128)
```
when reading shapefiles with accetend characters.

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
