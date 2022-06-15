# DaaS in Fog Computing

This repository contains the information, resources and results of the experiments run for the paper 'Efficient Data as a Service in Fog Computing: an Adaptive Multi-agent Based Approach' by Giulia Mangiaracina, Pierluigi Plebani, Mattia Salnitri, Monica Vitali. The paper and the experiments were developed at the Department of Electronics, Information, and Bioengineering, Politecnico di Milano.

The repository contains the resources for executing the experiment in the folder 'Test source' while the results of the experiment are in 'Test results'.

Test source contains:
- Code: scripts and files to run the simulation of a Fog environment and execute the tests 
- Resources: files needed to feed the tests
- requirements.xlsx: specifies the requirements of each spark node (called DAA in the paper). Each box specifies the thresholds of each metric, in the bottom part of every box the metric considered are specfied.
- Configuration: this folder contains the configurations used for the experiments shown in the paper 
   - `incremental test/Fog environment configuration`: contains the configuration randomly generated for the incremental test
      - `db.sql`: the dupm of the rational (MySQL) database use to store data of the experiment, included availability, latency of each node and the global whithe board. 
      - `availability.csv`: availability values used in the experiment (incluuded also in db.sql)
      -  `latency.csv`: latency values used in the experiment (incluuded also in db.sql)
   - `stress test/Fog environment configuration`: contains the 4 configurations randomly generated for the stress test. Each folder shares the same folder strucure of `incremental test/Fog environment configuration`.

