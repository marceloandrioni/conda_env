# Install
Download [Anaconda](https://www.anaconda.com/products/individual#Downloads) and install (tested with `Anaconda3-2021.11-Linux-x86_64.sh`)

# Activate `base` environment

Open a terminal and run `eval "$(/home/<user>/anaconda3/bin/conda shell.bash hook)"`

# Configure .condarc
Create the file with a text editor if it doesn't exist or just run `conda config` to create an empty file.

`/home/<user>/.condarc`

## Content:

```
channels:
  - defaults
  - conda-forge
  - fbriol   # FES tide https://github.com/CNES/aviso-fes

# https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#ssl-verification-ssl-verify
ssl_verify: false

# https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#show-channel-urls-show-channel-urls
show_channel_urls: True
```

If bebind a proxy (usual in corporations), it's possible to set proxy variables in the `.condarc` file like [this](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#configure-conda-for-use-behind-a-proxy-server-proxy-servers). However, the `mamba` package manager seems to ignore these definitions and `conda` itself ignores the `no_proxy` variable as reported [here](https://github.com/conda/conda/issues/7818). Due to this, is recommended to only set the proxy variables in the `~/.bashrc` file, e.g.:

```
export {all_proxy,ALL_PROXY,http_proxy,HTTP_PROXY,https_proxy,HTTPS_PROXY,ftp_proxy,FTP_PROXY}=http://myuser:mypsw@my-company.proxy.com:8080
export {no_proxy,NO_PROXY}=localhost,127.0.0.1,my-company.com
```

# New envs

**_NOTE:_** [mamba](https://github.com/mamba-org/mamba) is a reimplementation of the conda package manager in C++, so it is much faster when solving dependencies. It is worth installing it in the base env and just replacing the `conda` with `mamba` command when creating a new env (`mamba create -n <env_name> ...`) or installing a new package (`mamba install <pkg>`). I could not find a single reason to not use `mamba` instead of `conda` when creating a `env` or installing a package. But in case of a problem with any of the commands below, `conda` can be used instead. `mamba` has some dependencies, so it can take `conda` a few minutes (~5) to isntall it.

```
conda install -n base mamba
```

After installing `mamba` it is best to change the `base` environment (env) as little as possible and always create a new env when trying new packages. The new env can be created "by hand", e.g.:

`mamba create -n <env_name> python=3.8
spyder mamba
pandas missingno
xarray dask netcdf4 cfgrib rasterio rioxarray eccodes cdsapi
metpy metar seawater gsw pyfes pyinterp
unidecode humanize tabulate termcolor aniso8601
bs4 tenacity cachetools
pipreqs pikepdf cx_oracle flask flask-restful celery flower
cartopy seaborn windrose plotly python-kaleido folium ipyleaflet cmocean colorcet cmasher
python-docx xlsxwriter xlrd openpyxl
geopy alphashape descartes
gitpython git
lftp awscli
nco cdo ncview`

or imported from text files, e.g.:

`txt` with exact copy (versions) of packages:
* `conda list --explicit > spec-file.txt`
* `conda create --name <env_name> --file spec-file.txt`

`yml` with same packages (but not necessarly same versions):
* `mamba env export --no-build > environment.yml`
* `mamba env create -f environment.yml`
but remembering to edit the `name` and `prefix` variables at the first and last lines to the desired values.

**_NOTE:_** even using `mamba` an `env` with so many packages can take several (~5) minutes to solve all the dependencies, best to go grab a ☕.

After creation, the new env must be activated, e.g.:

`conda activate <env_name>`

### cx_oracle:
To connect to Oracle databases using the `cx_oracle` library, the user also needs the [Oracle library](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html) files.

Dowload and extract:
- instantclient-basic-linux.x64-21.1.0.0.0.zip
- instantclient-sqlplus-linux.x64-21.1.0.0.0.zip

Set the paths:
```
export LD_LIBRARY_PATH=/some/dir/instantclient_21_1:$LD_LIBRARY_PATH
export PATH=/some/dir/instantclient_21_1:$PATH
export ORACLE_HOME=/some/dir/instantclient_21_1
```

### env to test CF Conventions
`mamba create -n cfconv cfchecker compliance-checker`

### Others
List the available envs:

`conda env list`

Remove an env:

`conda remove --name <env_name> --all`

# pycodestyle
Ignore some errors and warnings in the code style:

`/home/<user>/.config/pycodestyle`

## Content:
```
[pycodestyle]
ignore = E501, E722, W503, W605
```
