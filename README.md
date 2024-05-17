# OnlineSoftware-BMTL1
------

## Development Environment

For development purposes, a local VM has been set up. In order to access it (if you have account), in a terminal write:
```
ssh -XY [username]@fanae35.geol.uniovi.es
```

NOTE: After the first log in, you can also set up the remote connection in VS Code but the OS version of the VM is not supported. 

## Setup

Once you have logged in for the first time, you need to set up some things, starting with a folder
```
mkdir DT/OnlineSW/
cd DT/OnlineSW/
```
NOTE: The creation of a folder like the above is a neccessity for the following steps and should not be skipped

Now we are ready to set up the herd plugin, inside the above folder, that is available [here](https://gitlab.cern.ch/cms-cactus/phase2/software/plugins/bmtl1). Clone this repository:
- with SSH
```
git clone ssh://git@gitlab.cern.ch:7999/cms-cactus/phase2/software/plugins/bmtl1.git
```
- or with HTTPS
```
git clone https://gitlab.cern.ch/cms-cactus/phase2/software/plugins/bmtl1.git
```
NOTE: If for some reason you cannot clone the above repository, it is very likely that a gitlab token is needed. More info about the tokens can be found [here](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html) 

Once the repository is cloned, build it with the following instructions 
```
cd bmtl1
export LD_LIBRARY_PATH=/opt/cactus/lib:$LD_LIBRARY_PATH
scl enable devtoolset-8 bash # Needed to use gcc 8
mkdir build && cd build && cmake3 .. # prepare for compilation
make -j 8 && make package # compile and produce rpms
```

In order for the herd plugin to work, it is also required to install a low level library from this [repository](https://gitlab.cern.ch/bmtl1-dev/bmtl1-sw). First, go back to working folder
```
cd DT/OnlineSW/
```
Clone the repository:
- with SSH
```
git clone ssh://git@gitlab.cern.ch:7999/bmtl1-dev/bmtl1-sw.git
```
- or with HTTPS
```
git clone https://gitlab.cern.ch/bmtl1-dev/bmtl1-sw.git
```
Once the repository is cloned, build it with the following instructions 

```
cd bmtl1-sw
scl enable devtoolset-8 bash # Needed to use gcc 8
mkdir build && cd build && cmake3 .. # prepare for compilation
make -j $(nproc) package # compile and produce rpms
```

## Herd

Once everything is installed, we can run herd.

Before running herd, we need to:
```
export SWATCH_LOG4CPLUS_CONFIG=/usr/etc/swatch/log4cplus.properties
```

- In order to check if herd has been installed succesfully, we can run some tests. Firstly, we need to change to the directory where the herd plugin has been installed 
```
cd DT/OnlineSW/bmtl1/
```
Then, we can run:
```
herd herd.yml --test plugin
```
and
```
herd herd.yml --test devices
```
- In order to start the herd server:
```
cd DT/OnlineSW/bmtl1/
herd herd.yml
```
This command will stat the herd server on port 3000, i.e. http://fanae35.geol.uniovi.es:3000/

## Shep

Once the herd server has started we can access the shep server, located in: http://fanae35.geol.uniovi.es

If you are trying to access the above address from outside the network, it is very likely that you will not have access. In order to overcome this you can tunnel through ssh.

In order to tunnel through ssh you need to change the network configuration in your browser. 
- Go to the browser settings and select "manual proxy configuration".
- In Host SOCKS put localhost and port 8080 (socks v5)

For example here is a screenshot with the correct settings for Firefox
![image](https://github.com/nplastir/OnlineSoftware-BMTL1/assets/138513470/f2f85d7c-c136-43ea-8c3c-96dbdf2a6217 | width=100)
