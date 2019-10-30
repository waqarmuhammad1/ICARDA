## Functionalities details

Right now this system supports analysis on following data sources:

1. Excel
2. CSV
3. MYSQL
4. Oracle
5. MSSQL

Following statistical analysis of data is supported by system so far are:

1. Correlation Matrix.
2. Data set description (Mean, mode, median, percentiles, std, max, min) of each available dataset column.
3. Histograms.
4. Scatter plots.
5. Box plots.

Following functionalities available for user to clean their data set:

1. Drop missing columns.
2. Drop missing rows.
3. Fill missing values by mean.
4. Fill missing values by median.
5. Fill missing values by standard deviation.
6. Fill missing values by most frequent values in targeted column.
7. Change string attribute encoding.

The algorithms supported by system are:

1. Linear regression.
2. Elastic regression.
3. Lars regression.
4. Lasso regression.
5. LassoLars regression.
6. Gausian Naive bayes.
7. Bernoulli Naive bayes.
8. Linear SVC
9. Support vector machine.

File formats supported for Human subject analysis are:
1. .pdf
2. .docx
3. .xlsx
4. .ppt
5. .jpg
6. .png
7. .jpeg
8. .mp4
9. .3gp



# Deployment Instructions

> Recommended Code Editor For UI

[Visual Studio Code](https://code.visualstudio.com/download)

This document is only for Ubuntu. Code will work onto other OS but i haven't tried running it


## How to setup UI

The project is build on materialize css and simple javascript, you dont need to download libraries specific for the project, you just need apache2.

## How to setup Apache2

- Apache2: 
    Go onto this link [How to install Apache2](https://www.linode.com/docs/web-servers/apache/apache-web-server-on-ubuntu-14-04/) or follow the instructions below.

     -- `sudo apt-get update && sudo apt-get upgrade`

     -- `sudo apt-get install apache2 apache2-doc apache2-utils`

     -- `sudo service apache2 restart`


After completeing the setup go onto your browser and type [localhost](127.0.0.1) you should be able to see the apache2 welcome page.

> NOTE: Apache sometimes conflicts with nginx 

After doing that copy and paste the Predator folder in `/var/www/html/`

You can now access UI via `localhost/Predator`


## How to setup user login Database [Couchdb](http://couchdb.apache.org/)

I have used NOSQL [couchdb](http://couchdb.apache.org/) docker image as user login database, so you will need to install [Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/#os-requirements)

Install docker via following command.

-- `sudo apt-get update` 

-- `sudo apt-get install docker-ce`

After docker is installed run following command.

-- `docker run -t --name db -p 8091-8094:8091-8094 -p 11210:11210 couchbase/server-sandbox:5.5.0`

This will download and run docker image in data persistance mode.

After the command completes, you can access couchdb from `localhost:8091`

The initial username is `Administrator` and password is `password`

After logging in to the console click on `Buckets` from side menu this will get you onto the page where you can create buckets (Consider bucket sort of like a table in RDBMS is just no relations) and you can store few different types of data types their including whole `python` object by pickling it.

Anyway there should be two buckets already available there
1. user_auth (100mb)
2. email_auth (100mb)

if they are not create them, `auth` is the bucket in which we are storing user's passwords and personal data upon registration by their username and `auth_email` is the one in which we are storing all user information by user's email_id, first bucket let you login by username and password and the second bucket will enable user to login by using their email_id and password combination.




After doing that we will need [couchbase python dependency](https://docs.couchbase.com/python-sdk/2.4/start-using-sdk.html). Install it using these commands.
##For Ubuntu 16.04
-- `wget http://packages.couchbase.com/releases/couchbase-release/couchbase-release-1.0-4-amd64.deb`

-- `sudo dpkg -i couchbase-release-1.0-4-amd64.deb`

-- `sudo apt-get update`

-- `sudo apt-get install libcouchbase-dev build-essential python36-dev python36-pip`

-- `sudo pip3 install couchbase`
##For Ubuntu 18.04

--  `echo "deb http://packages.couchbase.com/ubuntu bionic bionic/main" | sudo tee /etc/apt/sources.list.d/couchbase.list`
--  `wget -O- http://packages.couchbase.com/ubuntu/couchbase.key | sudo apt-key add -`
--  `sudo apt-get update`
--  `sudo apt-get install libcouchbase2-libevent libcouchbase-dev`
-- `sudo pip3 install couchbase`
Link to the [documentation](http://docs.couchbase.com/sdk-api/couchbase-python-client-2.1.1/) of couchbase client

> NOTE: If you are creating the `Buckets` yourself, then add following code in CouchController.py script to fill data or you can simply launch web application (localhost/Predator) to complete the registration process.

-- `couch = CouchAPI('Administrator', 'password', '0.0.0.0')`

-- `couch.open_bucket()`


-- `username = 'admin'`

-- `password = 'password'`

-- `email = 'waqar.muhammad@slu.edu'`

-- `first_name = 'Waqar'`

-- `last_name = 'Muhammad'`

-- `user_auth = {'first_name': first_name, 'last_name': last_name, 'email_id': email, 'pwd': password}`

-- `email_auth = {'first_name': first_name, 'last_name': last_name, 'user_name': username, 'pwd': password}`

-- `couch.store_user_auth('admin', user_auth)`

-- `couch.store_email_auth('waqar.muhammad@slu.edu', email_auth)`



## How to setup python environment

Backend of the project is build on python 3.6, if you are a linux user it comes along with all linux distributions so we don't need to download it specifically



> Don't upgrade your python-pip to newer version, the newer version is unstable you wont be able to install any python dependencies via pip, but instead you will have to use python-pip command to install dependencies.

## [Python Flask](http://flask.pocoo.org/)

To connect Backend API to Frontend we are using [Flask](http://flask.pocoo.org/), to install full stack of flask dependencies run following commands.

`sudo pip3 install flask`

`sudo pip3 install -U flask-cors` This will install [Flask-Cors](https://flask-cors.readthedocs.io/en/latest/).

`sudo pip3 install flask-restful` This will install [Flask-Restful](https://flask-restful.readthedocs.io/en/0.3.5/installation.html)

## Machine Learning & Other libraries

`sudo pip3 install xlrd` #To read excel files

`sudo pip3 install sklearn`

`sudo pip3 install scipy`

`sudo pip3 install pandas`

`sudo pip3 install numpy`

`sudo pip3 install mysql-connector-python`

`sudo python3 -m pip install opencv-python`

`sudo python3 -m pip install opencv-contrib-python`

`sudo pip3 install docx2txt`

`sudo conda create -n imageai -c conda-forge python=3.6`

`sudo conda install -c conda-forge tensorflow keras mkl-service opencv numpy scipy pillow matplotlib h5py`

Download trained object detection model from following link [RetinaNet] (https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/resnet50_coco_best_v2.0.1.h5)

After installing above libraries, you have to run two API files through terminal or any perferred python IDE
1. RunAPI.py (python3 RunAPI.py) machine learning modeling and statistical analysis api running on port 5000 and ip 0.0.0.0
2. HSAPI.py (python3 HSAPI.py) object detection and file image extraction api running on port 5001 and ip 0.0.0.0

You should see something like this.
 * Serving Flask app "flasktet" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 176-475-588
`


##Install using docker compose
Clone
```
$ git clone https://github.com/waqarmuhammad1/ICARDA.git && cd ICARDA
```

Create configVariables.js to define your environment values
```
$ vim PredatorI/js/configVariables.js
```
```
const configVariables = {
    APPLICATION_DOMAIN: 'http://localhost', //<Domain>
    API_PORT: 810, //API port, defined in docker-compose.yml 810:5000
    HSAPI_PORT: 811, //HSAPI port, defined in docker-compose.yml 811:5001
};
```

Download trained object detection model from following link [RetinaNet]
```
$ wget https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/resnet50_coco_best_v2.0.1.h5
```

Build the docker
```
$ docker-compose up -d --build
```

After the build completes, you can access couchdb from `localhost:8091`, the initial username is `Administrator` and password is `password`.
Add the following buckets:
  1. user_auth (100mb)
  2. email_auth (100mb)
  3. auth (100mb)
  4. auth_email (100mb)

Check if the application is running
```
$ docker logs -f --tail 50 python
```
In the output, you should see `Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)` and `Running on http://0.0.0.0:5001/ (Press CTRL+C to quit)`

Access the application through http://localhost:812/PredatorI

**Note:** In case of changing couchdb default username and password, it should be changed in `docker-compose.yml`:
``` 
    environment:
      - "DB_HOST=python_db"
      - "DB_USERNAME=Administrator"
      - "DB_PASSWORD=password"
``` 

