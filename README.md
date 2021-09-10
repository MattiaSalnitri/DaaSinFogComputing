# DaaS in Fog Computing



variables
<username>
<IP_VM1>
<IP_VM2>

### Start base services:

#### VM1
1. start containers: Login as root in VM1
   1. `cd /home/mangiaracina/prova/VM1/db`
   2. `sudo docker-compose up &`
2. Check the service are up and running: open a shell in you local pc
   1. `ssh <username>@<IP_VM1> -L 9000:<IP_VM1>:9000`
3. open your local browser and go to http://localhost:9000/
   1. login in minio using access key: 'minio', token: 'minio123'
   2. create a new bucket ( button 'create bucket' on the left lower corner)
   3. name of the bucket 'miniobucket'
   4. upload the file XXX


#### VM2
1. Login as root in VM2
   1. `cd /home/mangiaracina/prova/VM2/db`
   2. `sudo docker-compose up &`



