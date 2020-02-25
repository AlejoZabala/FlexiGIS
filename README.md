# FlexiGIS: an open source GIS-based platform for modelling energy systems and flexibility options in urban areas.
FlexiGIS stands for Flexibilisation in Geographic Information Systems (GIS). It extracts, filters and categorises the geo-referenced urban energy infrastructure, simulates the local electricity consumption and power generation from on-site renewable energy resources, and allocates the required decentralised storage in urban settings. FlexiGIS investigates systematically different scenarios of self-consumption, it analyses the characteristics and roles of flexibilisation technologies in promoting higher autarky levels in cities. The extracted urban energy infrastructure are based mainly on OpenStreetMap data.

[![Documentation Status](https://readthedocs.org/projects/flexigis/badge/?version=latest)](https://flexigis.readthedocs.io/en/latest/?badge=latest)


# FlexiGIS Components

## Module I: FlexiGIS urban spatial platform
This package establishes urban energy infrastructure. It extracts, acquires and processes urban georeferenced data extracted from OpenStreetMap. In order to extract the OpenStreetMap georeferenced datasets of urban energy infrustructure datasets and its required features, Module I derives an automatised extraction procedure. Firstly, the raw OpenStreetMap data is downloaded from the OpenStreetMap database for the invistigated urban space from `Geofabrik`. Second, the OpenStreetMap datasets are filtered for the respective case study defined by the provided `.POLY` file using the open source java tool `osmosis`. The OpenStreetMap data are filtered for the follwoing OSM elements:
* `landuse = *` tag provides information about the human use of land in the respective area (see [Figure 1](data/04_Visualisation/landuse.png))
* `building = *` tag describes all mapped objects considered as buildings of different categories, example residential, schools, etc. (see [Figure 2](data/04_Visualisation/buildings.png))
* `highway = *` tag describes all lines considered as streets, roads, paths, etc. (see [Figure 3](data/04_Visualisation/highway.png))
![FlexiGIS Buildings](data/04_Visualisation/landuse.png)
Fig. 1. Extracted OpenStreetMap `landuse` datasets for the city of Oldenburg. Credits: OpenStreetMap contributors.
![FlexiGIS Buildings](data/04_Visualisation/buildings.png)
Fig. 2. Extracted OpenStreetMap `building` datasets for the city of Oldenburg. Credits: OpenStreetMap contributors.
![FlexiGIS Highway](data/04_Visualisation/highway.png)
Fig. 3. Extracted OpenStreetMap `highway` datasets for the city of Oldenburg. Credits: OpenStreetMap contributors.

After filtering the OSM raw data, the georeferenced building and roard infrastructure in the city of Oldenburg are exported to a relational postgis-enabled database using the open source `osm2pgsql`. Theses datasets can be exported as CSV files or visualised as maps (see Figures 1-3).

## Module II: FlexiGIS temporal dimension
It simulates urban energy requirments (consumption and generation). The spatio-temporal electricity consumption and renewable energy generation from PV and wind in the defined urban area are modelled. This component models the combined spatial parameters of urban geometries (Module I) and linkes them to real-world applications using GIS. Here, a bottom-up simulation approach is developed to calculate local urban electricity demand and power generation from available renewable energy resources. For instance, using open source datases like Standardised Load Profiles and publicaly available weather data, [ Figure 4 ](data/04_Visualisation/Energy_Requirments.png) shows the generated quarter-hourly time series of the aggregated load and PV power supply profile for invistigated case study.

![FlexiGIS Simulated Energy_requirements](data/04_Visualisation/Energy_Requirments.png)
Fig. 4. Simulated electricity consumption (green) and solar power generation (red) for the city of Oldenburg.

## Module III: Optimisation of flexibility options
The spatial-temporal simulation outputs from Module I and II are highly resolved time series of electricity demand and supply. Theses generated datasets will be used by the [urbs](https://urbs.readthedocs.io/en/latest/) model as inputs to the linear optimisation problem. This module aims to determine the minimum system costs at the given spatial urban scale while matching simultenously the simulated electricity demand. In addition, it aims to allocate and optimise distributed storage and other felecibility options in urban energy systems. This component will be explained in details. The codes, scripts and coupling of urbs model and FlexiGIS platform will be added.

## System requirements
FlexiGIS is developed and tested on Linux (Ubuntu 16.04.6). The tools and software used in FlexiGIS and their versions are listed in the following:

* Operating system: Ubuntu 16.04.6 LTS, Release: 16.04, Codename: xenial
* PostgreSQL version: 11.2 (64-bit)
* PostGIS version: 1.5.3
* osmosis version: 0.44.1
* osm2pgsql version: 0.88.1 (64bit id space)
* GNU Make version: 4.2.1
* Python: 3.6.7
* GNU bash: 4.3.48(1)-release (x86_64-pc-linux-gnu)

PostgreSQL: To install PostgreSQL, refer to the [PostgreSQL](http://www.postgresql.org/) webpag. Please note that you need at least the version or higher to run FlexiGIS.

Osmosis: To use [Osmosis](http://wiki.openstreetmap.org/wiki/Osmosis), unzip the distribution in the location of your choice. On unix/linux systems, make the bin/osmosis script executable (ie. chmod u+x osmosis). If desired, create a symbolic link to the osmosis script somewhere on your path (eg. ln -s appdir/bin/osmosis ~/bin/osmosis).

osm2pgsql: Instruction are available on how to download and install osm2pgsql for Linux systems on the [osm2pgsql](http://wiki.openstreetmap.org/wiki/Osm2pgsql) webpage.

Python: Ensure you can run python (with version 3 and above) on your OS. Python can be downloaded following this [link](https://www.python.org/downloads/) or download the [Anaconda](https://www.anaconda.com/distribution/) distro.

## Getting Started
To use the FlexiGIS spatial-temporal platform (Module I and II) download the FlexiGIS code and data folder as a zip file or clone the repository from the FlexiGIS GitHub repo. After downloading the FlexiGIS code, unzip the folder FlexiGIS in the location of your choice. The file structure of the FlexiGIS code folder is as follows:

* FlexiGIS
    * ├── code
    * ├── data
    * │   ├── 01_raw_input_data
    * │   ├── 02_urban_output_data
    * │   ├── 03_urban_energy_requirements
    * │   └── 04_Visualisation
    * ├── doc
    * ├── README.md
    * └── requirements.txt

## Installation
After making sure all system requirements are satisfied, clone FlexiGIS repo
create a Python virtual environment where the required python dependencies can be installed using pip. Please note, that this applies to Module I and II. Python virtual environment can be created by following the tutorial steps from [here](https://packaging.python.org/tutorials/installing-packages/).

first clone the FlexiGIS code from the GitHub repository by running:
```console
user@terminal:~$ git clone https://github.com/FlexiGIS/FlexiGIS.git
```
In the next step, create a Python virtual environment (give a name e.g. _env_name) where the required python dependencies can be installed using pip.

```console
user@terminal:~$ python3 -m venv _env_name
```

Then, activate the virtual environment:

```console
user@terminal:~$ source _env_name/bin/activate
```
After creating and activativating the python virtual environment, cd into the cloned FlexiGIS directory:

```console
(_env_name) user@terminal:~$ cd ../FlexiGIS
```

install the required python dependencies by running:

```console
(_env_name) user@terminal:~/FlexiGIS$ pip install -r requirements.txt
```
Now you are ready to excute FlexiGIS (Please note, that this applies for both Module I and II).

## Running FlexiGIS
To run the first two components of the FlexiGIS platform (Module I and II), execute the following steps:

1. After completing the setting up of FlexiGIS environment as mentioned in the steps above, go to the folder code, check the parameters in config.mk file.

2. Ensure that your database parameters are properly set in the config.mk file, and also the OSM data link of the spartial location of choice.

3. Ensure the poly file of the spartial location of choice is available in the data/01_raw_input_data directory before running FlexiGIS.

###### The available makefile options are:
To run the available makefile options, go into the code directory in your linux terminal, then run
```console
(_env_name) user@terminal:~/FlexiGIS/code$ make all
```
make-all executes multiple make options for the OSM data, from download to the simulation of load and PV profiles for the urban infrastructures. However, you can run these step one after the other by running
```console
(_env_name) user@terminal:~/FlexiGIS/code$ make download
```
make download, downloads spatial OSM data of a given location
```console
(_env_name) user@terminal:~/FlexiGIS/code$ make filter_data
```
filter the downloaded OSM data using a bounding poly file of the location
```console
(_env_name) user@terminal:~/FlexiGIS/code$ make export_data
```
exports the spatially filtered OSM data to a PostgreSQL database using [osm2pgsql](http://wiki.openstreetmap.org/wiki/Osm2pgsql)
```console
(_env_name) user@terminal:~/FlexiGIS/code$ make abstract_data
```
extract categorised highway, building and landuse data from filtered OSM data
```console
(_env_name) user@terminal:~/FlexiGIS/code$ make demand_supply_simulation
```
executes the simulation of load and PV profiles for the urban infrastructures
```console
(_env_name) user@terminal:~/FlexiGIS/code$ make drop_database
```
provides option to either drop the database or not.
```console
(_env_name) user@terminal:~/FlexiGIS/code$ make example
```
make example can be run to generate an example simulation of aggregated load and PV profile for Oldenburg. The example imports spatially filtered OSM Highway, landuse and building data stored as csv files in the ../data/01_raw_input_data/example_OSM_data folder. In other words, it runs steps e and g using the provided example data. After running the FlexiGIS model using the makefile, the resulting aggregated load and PV profiles, urban infrastructure data are stored as .csv data in folder data/03_urban_energy_requirements, also static plots of the urban infrastrure and simulated load and PV profiles are created and stored in the data/04-visualisation folder. To visualised the extracted georeferenced urban infrastructure interactively, run
```console
(_env_name) user@terminal:~/FlexiGIS/code$ make shapefile
```
this generates shapely objects of the extracted urban infrastructure, which can be used to generate interactive plots in [QGIS](https://www.qgis.org/en/site/).

## Documentation
Jump to [documentation](https://flexigis.readthedocs.io/en/latest/) for more details about FlexiGIS.

## License
FlexiGIS is licensed under the [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause), "New BSD License" or "Modified BSD License".

The OpenStreetMap (OSM) data is available under the Open Database License (ODbL). A description of the ODbL license is available [here](http://opendatacommons.org/licenses/odbl). OpenStreetMap cartography is licensed as CC BY-SA. For more information on the copyright of OpenStreetMap please visit [here](http://www.openstreetmap.org/copyright).

## Contact
Author: Alaa Alhamwi alaa.alhamwi@dlr.de

Organisation: German Aerospace Center ([DLR](https://www.dlr.de/ve/)) - Institute of Networked Energy Systems, department for Energy system Analysis(ESY).

## Project Team
Dr. Wided Medjroubi (project leader), Dr. Alaa Alhamwi (FlexiGIS main author), MSc. Chinonso C. Unaichi
