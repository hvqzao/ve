# ve

Created to provide fast provisioning - automated download, build and set up Python or
Node.JS virtual environments. It uses Python 2 virtualenv, Python 3 (built-in) venv and
nodeenv (installed from PyPI repo). Sourcing Node will automatically link node_modules
to base directory. Python venv activate include new setdsm function for Django users.

Windows platform is not supported.

## Usage

```
Usage: ve [options...] [py-env] [node-env]

  -b	--base-dir				- echoes base directory for project
  -r	--prerequisites				- installs required prerequisites (deb)
  -i	--installed				- list installed virtual environments
  -p	--python-newest-available		- print newest Python versions available
  -P	--python-build <py-version>		- build Python environment for version
  -n	--node-available <py-env>		- print list of Node versions available
  -N	--node-build <py-env> <node-version>	- build Node environment for version
  [env]						- source environment i.e.: . ./ve py-3.4.3
```

## Configuration

.ve file in script directory if present, will be sourced. BASE_DIR, PY, NODE,
VE_BIN are expected environment variables. PY and NODE will be used when script
will detect that it has been sourced with no parameters given. .ve file will
also be searched in BASE_DIR. BASE_DIR/VE_BIN will be added to PATH when script
is sourced.

New functions available after sourcing:
deact() will restore PATH and deactivate Python and Node envs.
cdve() changes directory to BASE_DIR.

Eventually, above mentioned parameters can be hardcoded in ve script, i.e: 

```
BASE_DIR="/srv/project"
PY="py-3.4.3"
NODE="node-0.12.7"
VE_BIN="bin"
```

If not retrieved or set, script base directory will act as a fallback value.

## Example setup

```
/srv/project/
├── bin/
│   ├──.ve
│   ├──ve
│   └──[...other tools...]
└── .ve
```

`/srv/project/bin/.ve` contents:
```
BASE_DIR=/srv/project
```

`/srv/project/.ve` contents:
```
PY=py-3.4.3
NODE=node-0.12.7
VE_BIN=bin
```

Example use with this setup, we go to project directory and see which Python versions
are current:
```
~$ cd /srv/project
/srv/project$ ./bin/ve -p
3.4.3
2.7.10
```

Now we decide to download Python 3.4.3:

```
/srv/project$ ./bin/ve -P 3.4.3
```

Right now we check which node versions are available. To do that we need to point out
installed Python virtual environment, we use 3.4.3 installed a moment ago:

```
/srv/project$ ./bin/ve -n 3.4.3
Requirement already up-to-date: nodeenv in ./lib/python3.4/site-packages
0.0.1	0.0.2	0.0.3	0.0.4	0.0.5	0.0.6	0.1.0	0.1.1
0.1.2	0.1.3	0.1.4	0.1.5	0.1.6	0.1.7	0.1.8	0.1.9
[...]
0.11.11	0.11.12	0.11.13	0.11.14	0.11.15	0.11.16	0.12.0	0.12.1
0.12.2	0.12.3	0.12.4	0.12.5	0.12.6	0.12.7
```

We install most current version of Node.JS:

```
/srv/project$ ./bin/ve -N 3.4.3 0.12.7
```

Installed Python and Node.JS environments will land in `/srv/project/build/`.
Right now we have everything set, we can activate both environments:

```
/srv/project$ . ./bin/ve
(node-0.12.7) (py-3.4.3) /srv/project$ 
```

The above will:
- set up symbolic links in `/srv/project` directory to our environments
- set up symbolic link to `node_modules`
- add `deact` function to environment will will deactivate both environments
- add `cdve` function which will change current directory to `/srv/project`

In Python virtual environments there is symbolic link automatically created
to `site-packages` in main virtual environment directory, for convenience.

## License

[MIT License](https://github.com/twbs/bootstrap/blob/master/LICENSE)
