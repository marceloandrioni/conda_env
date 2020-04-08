# conda_env

in `~/.condarc`
```
ssl_verify: false

channels:
    - fbriol   # FES Tide https://bitbucket.org/cnes_aviso/fes/src/master/
    - conda-forge
    - defaults

# Proxy settings: http://[username]:[password]@[server]:[port]
proxy_servers:
    http: http://usr:psw@proxyserver.com:8080
    https: https://usr:psw@proxyserver.com:8080
```

# main work env
```
conda create -n work python=3.7 \
    spyder ipython \
    psutil requests unidecode humanize termcolor colorama aniso8601 tenacity \
    pandas geopandas seaborn xlrd \
    dask netcdf4 xarray cfgrib eccodes cdsapi pyferret ferret_datasets metpy xesmf \
    cx_oracle fes \
    matplotlib windrose cartopy shapely fiona bokeh plotly folium
```

# operational env
same as work but without spyder

# env to test CF Conventions (incompatible with other packages)
`conda create -n cfconv cfchecker compliance-checker`
