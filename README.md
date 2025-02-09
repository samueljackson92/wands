# Wide Area Network Data Stream (wands)
UKAEA work with adios to transport data between fusion reactor and CSD3. This is a prototype and very much a work in progress. 

## Setup environment
The easiest way to run the code is to setup a conda environment and install adios2 as a conda package:

```bash
conda create -n <name> -c conda-forge adios2 numpy mpi4py h5py
```

Once the environment is created and all the packages are installed activate this environment:

```bash
conda activate <name>
```

Then use pip to install this package into the environment:

```bash
pip install -e .
```

This setup is necessary in every machine the code is supposed to run.

## Testing the installation

To test the installation, run the test suite:

```bash
pytest
```


## Running the prototype
CSD3 specific instructions to run the code:
1. start an interactive session on CSD3:

```bash 
salloc -t xx:xx:xx -Nx -ppnx -A <project-account> -p <partition> bash
```
2. inquire the IP adress of the allocated node:
``` bash
nslookup cpu-q-xxx.data.cluster
```
3. a) If you want to send data from a remote location to CSD3. At the moment this is done via ssh tunneling (started on remote machine)
```bash
ssh -R 12345:localhost:12345 -o ProxyJump=<login-node> <CSD3ID>@<IP from nslookup>
```
    - first start server on remote machine: 
```bash
python dataman-server.py
```
    - then start receiver on CSD3 node: 
```bash
python dataman-receiver.py
```
        
   b) Send data from CSD3 to remote machine also with an SSH tunnel (started on remote machine)
   
```bash
ssh -L 12345:localhost:12345 -o ProxyJump=<login-node> <CSD3ID>@<IP from nslookup>
```
    - first start server on compute node: 
```bash
python dataman-server.py
```
    - then start receiver on remote host: 
```bash
python dataman-receiver.py
```
    
Note: Setting up an SSH tunnel with proxy jump requires two different TOTP tokens.
