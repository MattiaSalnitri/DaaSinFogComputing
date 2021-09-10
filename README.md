# DaaS in Fog Computing

This repository contains the information, resources and results of the experiments run for the paper 'Data as a Service in Fog Computing: an Adaptive Multi-agent Based Approach' by Giulia Mangiaracina, Pierluigi Plebani, Mattia Salnitri, Monica Vitali. The paper and the experiments were developed at the Department of Electronics, Information, and Bioengineering, Politecnico di Milano

The repository contains the resources for executing the experiment in the folder 'Test source' while the results of the experiment in 'Test results'

Test source contains:
- Code: scripts and files to run the simulation of a Fog environment and execute the tests 
- Resources: files needed to feed the tests
- requirements.xlsx: specifies the requirements of each spark node (called DAA in the paper). Each box specifies the thresholds of each metric, in the bottom part of every box the metric considered are specfied.

Test results contains:
- incremental test: contains the configuration randomly generated for the incremental test
- stress test: contains the configuration randomly generated for the stress test
- results.xlsx:  contains all raw and aggregated results presented in teh paper.


The next part of this file details how to execute the experiments. Few variables are used:
- `<username>` Username of a power user (admin) of the virtual machine
- `<IP_VM1>` ip address of Virtual machine 1
- `<IP_VM2>` ip address of Virtual machine 2
- `<scriptHome>` path to the folder where the [scripts](https://github.com/MattiaSalnitri/DaaSinFogComputing/tree/main/Test%20source/Code/Test%20scripts) are stored. If the virtual machine provided in this repository are used, then the value of this variable is `/home/salnitri`.
- `<simulationHome>` path to the folder where the [simulation files](https://github.com/MattiaSalnitri/DaaSinFogComputing/tree/main/Test%20source/Code/Fog%20simulation%20environment) are stored. If the virtual machine provided in this repository are used, then the value of this variable is `/home/mangiaracina`.


## Run Virtual Machines:
Download the images of the two virtual machines and deply them.

Alternatively you can create two virtual machines: 
- recomended resources: 20 cores, 32 GB of memoroy, 50 GB hd
- operative system: Linux version 5.4.0-81-generic (buildd@lgw01-amd64-052) (gcc version 9.3.0 (Ubuntu 9.3.0-17ubuntu1~20.04))
- installed software: Python 3.8.10, Docker version 20.10.7, build 20.10.7-0ubuntu1~20.04.1

To set-up the virtual machines:
- uncompress the files used for the simulation of the Fog envirobnment, contained [this](https://github.com/MattiaSalnitri/DaaSinFogComputing/tree/main/Test%20source/Code/Fog%20simulation%20environment) folder respecively on VM1 and 2.
- uncompress the script files for the test, contained [this](https://github.com/MattiaSalnitri/DaaSinFogComputing/tree/main/Test%20source/Code/Test%20scripts) folder respecively on VM1 and 2.
- update the path of the scripts below

## Start base services:

### VM1
1. Start containers: Login as root in VM1
   1. `cd <simulationHome>/prova/VM1/db`
   2. `sudo docker-compose up &`
2. Check the service are up and running: open a shell in you local pc
   1. `ssh <username>@<IP_VM1> -L 9000:<IP_VM1>:9000`
3. Open your local browser and go to http://localhost:9000/
   1. login in minio using access key: 'minio', token: 'minio123'
   2. create a new bucket ( button 'create bucket' on the left lower corner)
   3. name of the bucket 'miniobucket'
   4. upload this [file ](https://github.com/MattiaSalnitri/DaaSinFogComputing/blob/main/Test%20source/Resources/file1.json)
   5. close the shell ONLY when the upload is finished
4. Open your local browser and go to `http://<IP_VM1>:8080/`
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
   1. `cd <simulationHome>/prova/VM2/db`
   2. `sudo docker-compose up &`
2. Check the service are up and running: open a shell in you local pc
   1. `ssh <username>@<IP_VM2> -L 9001:<IP_VM2>:9000`
3. Open your local browser and go to http://localhost:9001/
   1. login in minio using access key: 'minio', token: 'minio123'
   2. create a new bucket ( button 'create bucket' on the left lower corner)
   3. name of the bucket 'miniobucket'
   4. upload this [file ](https://github.com/MattiaSalnitri/DaaSinFogComputing/blob/main/Test%20source/Resources/file1.json)
   5. close the shell ONLY when the upload is finished

At this point the virtual machines are ready and configured and tests can be started.

## Start tests:

There are two types of tests that can be executed:
- stress tests
- increment tests.


### Start stress tests:
To start the stress test run the following command:
`nohup <scriptHome>/startBatchTest.sh > logBatch 2>&1 &`

### Start incremental tests:
`nohup <scriptHome>/startBatchInjectionIncrementalTest.sh > logBatch 2>&1 &`

Both commands will execute the tests and save the logs in the 'logBatch' file. the test may lasts for several days, depending on the amount of resources assigned to the virtual machine. With 20 cores, 32 GB of memoroy, 50 GB hd the tests run for 3 to 5 days. 

### Reproduce a previous experiment
To reproduce a previous experiment, the intial configuration need to be uploaded in the SQL database stored in VM1. [This](https://github.com/MattiaSalnitri/DaaSinFogComputing/tree/main/Test%20results/incremental%20test/Fog%20environment%20configuration) and [this](https://github.com/MattiaSalnitri/DaaSinFogComputing/tree/main/Test%20results/stress%20test/Fog%20environment%20configuration) folders contain the initial configuration of the experiments whose results are shown in the paper mentioned at the beginning of this readme. to reprouce the experiment drop the old db and upload the desider one usign the following command in VM1:
- `mysql -h 10.75.4.65 --port 3308 -u root -phelloworld -D db -N -e "DROP database db"`
- `mysql -h 10.75.4.65 --port 3308 -u root -phelloworld -e "create database db"`
- `mysql -h 10.75.4.65 --port 3308 -u root -phelloworld -D db < <path to desired dump>`   