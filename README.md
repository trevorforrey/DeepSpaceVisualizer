# Deep Space Visualizer

## Build Instructions

The only officially supported operating systems are Debian-based Linux Distributions.  
Ubuntu in particular is recommended.

To begin, install the following packages:  
- curl
- libboost-python-dev
- libboost-filesystem-dev
- build-essential
- python3-dev
- nodejs

NOTE: On Ubuntu, you may have to enable the "Universe" PPA to be able to install libboost-python-dev. You can do so by running `add-apt-repository universe` as root.

Cmake is also required but the necessary version is not available in apt repositories as of the time of this writing.  
Cmake can be downloaded and installed from the following link: https://cmake.org/download/. The latest version should be sufficient.  

once dependencies are downloaded, `cd` into the project root.  

The first step is to build the Spyce library.  
run the following commands:  
```bash
cd spyce
cmake .
make
mv spyce.so ../
```

afterwards, `cd` back to the project  

This will also move the spyce library into the project root where the flask server is expecting it.

For the frontend, before being able to compile it, you mus install the dependencies:
```bash
npm install
```

The frontend software must be compiled and can be done so by running the following:

```bash
npm run build
```

This will compile the web files into the `dist/` directory.

The last step is to populate the config directory.
If the kernels were not provided, 2 of them can be downloaded from nasa:

 - The leapseconds descriptor kernel
   - https://naif.jpl.nasa.gov/pub/naif/generic_kernels/lsk/latest_leapseconds.tls
 - Planetary ephemeris kernel
   - https://naif.jpl.nasa.gov/pub/naif/generic_kernels/spk/planets/de430.bsp

A third kernel is required for the ephemeris data of the satellite and this must be provided externally.

All kernel files should be placed in the `config/kernels/` directory. 
A JSON config file will also have to be created at `config/config.json`. 
An example has been provided below:
```JSON
{
    "main_subject_id": -39,
    "main_subject_name": "LMAP",
    "kernels": [
        "planets.bsp",
        "LMAP_FullTrajectory.bsp",
        "latest_leapseconds.tls"
    ]
}
```

Change the data to match the subject satellite and add the kernels as they are in the kernels folder.

Lastly, the server dependencies must be installed which can be done with the following commands:
```
apt-get install -y python3-pip
pip3 install flask
```

Once finished, the application can be run by executing `python3 FlaskServer.py` from the root directory and visiting http://localhost:5000 in a web browser.
