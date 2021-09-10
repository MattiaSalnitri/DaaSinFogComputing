# DaaS in Fog Computing



variables
<username> Username of a power user (admin)
<IP_VM1> ip address of Virtual machine 1
<IP_VM2> ip address of Virtual machine 2


## Run Virtual Machines:
Download the images of the two virtual machines and deply them.

Alternatively you can create two virtual machines, with docker installed, and
- ucompress the files used for the simulation of the Fog envirobnment, contained [this]() folder respecively on VM1 and 2.
- uncompress the script files for the test, contained [this]() folder respecively on VM1 and 2.
- update the path of the scripts below

## Start base services:

### VM1
1. start containers: Login as root in VM1
   1. `cd /home/mangiaracina/prova/VM1/db`
   2. `sudo docker-compose up &`
2. Check the service are up and running: open a shell in you local pc
   1. `ssh <username>@<IP_VM1> -L 9000:<IP_VM1>:9000`
3. open your local browser and go to http://localhost:9000/
   1. login in minio using access key: 'minio', token: 'minio123'
   2. create a new bucket ( button 'create bucket' on the left lower corner)
   3. name of the bucket 'miniobucket'
   4. upload this [file ](https://github.com/MattiaSalnitri/DaaSinFogComputing/blob/main/Test%20source/Resources/file1.json)
   5. close the shell ONLY when the upload is finished
4. open your local browser and go to http://<IP_VM1>:8080/
   1. login with the following authentication details
      - system = MySql
      - server = mysql-development
      - username = root
      - password = helloworld
      - database = db
   2. if the database is not empy, drop it
   3. import the database contained in [this file](https://github.com/MattiaSalnitri/DaaSinFogComputing/blob/main/Test%20source/Resources/db.sql.gz)


### VM2
1. Login as root in VM2
   1. `cd /home/mangiaracina/prova/VM2/db`
   2. `sudo docker-compose up &`
2. Check the service are up and running: open a shell in you local pc
   1. `ssh <username>@<IP_VM2> -L 9001:<IP_VM2>:9000`
3. open your local browser and go to http://localhost:9001/
   1. login in minio using access key: 'minio', token: 'minio123'
   2. create a new bucket ( button 'create bucket' on the left lower corner)
   3. name of the bucket 'miniobucket'
   4. upload this [file ](https://github.com/MattiaSalnitri/DaaSinFogComputing/blob/main/Test%20source/Resources/file1.json)
   5. close the shell ONLY when the upload is finished

At this point the virtual machines are ready and configured. Tests can be started.

## Start tests:

There are two types of tests that can be executed:
- stress tests
- increment tests


### Start stress tests:
To start the stress test run the following command:
'nohup ./startBatchTest.sh > logBatch 2>&1 &'

### Start incremental tests:
nohup ./startBatchInjectionIncrementalTest.sh > logBatch 2>&1 &

Both commands will execute the tests and save the logs in the 'logBatch' file. the test may lasts for several days, depending on the amount of resources assigned to the virtual machine. With 20 cores, 32 GB of memoroy, 50 GB hd the tests run for 3 to 5 days. 

### Reproduce a previous experiment
To reproduce a previous experiment, the intial configuration need to be uploaded in the SQL database stored in VM1. [This]() folder contains the initial configuration of the experiments whose results are shown in the paper mentioned at the beginning of this readme. to reprouce the experiment drop the old db and upload the desider one usign the following command in VM1:
- mysql -h 10.75.4.65 --port 3308 -u root -phelloworld -D db -N -e "DROP database db"
- mysql -h 10.75.4.65 --port 3308 -u root -phelloworld -e "create database db"
- mysql -h 10.75.4.65 --port 3308 -u root -phelloworld -D db < < path to desired dump>   