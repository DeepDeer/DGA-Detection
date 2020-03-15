
Overview of this system
===============

This system is a module of our network security situation awareness project.
It's developed with Python and has two major functions:
* DNS traffic visualization. With nine time series plots of the key fields 
(such as query type、opcode、Top top 5 queried e2ld, etc), you can easily minor
the DNS traffic on network level and user level.

Network level fields | Network level fields
--------- | -------------
request and response | top 5 queried e2ld 
query type  | top 5 queried fqdn
opcode | domain length
packet size | unique e2ld

Fields for specific attack| description
--------- | -------------
average nxdomain response | DGA domain query
average TXT response size | DNS tunnel

User level fields |
--------- |
top K query user| 
top K query e2ld user|

<div align=center><img src="https://github.com/DeepDeer/DGA-Detection/blob/master/plots.png" width="600" height="350"/></div>

* DGA detection. We employ machine learning methods like CNN, LSTM, and Word2vec 
(Not integrated currently) to detect DGA-generated domains based on their 
character-level distributions or inter-domain relations. For more design details, 
please refer to the following papers.
[1]D3N: DGA Detection with Deep-Learning through NXDomain. [KSEM 2019]  

[2]Domain-Embedding Based DGA Detection with Incremental Word2Vec Model.[ISCC 2020 (submitted)]
<div align=center><img src="https://github.com/DeepDeer/DGA-Detection/blob/master/domain.png" width="600" height="350"/></div>

System Framework
---------------------------------------


![Overview of the system](https://github.com/DeepDeer/DGA-Detection/blob/master/structure.jpeg)
As shown in this figure, there are five main modules in this system:
* Parsing module: parase pcap files and extract features with tshark.
* Detect module: 1)filter out error packets and packets with irregular domain names;
* Store module: built with click house.
2)Detect DGA domain with our trained model.
* Present module: built with grafana;
* Data enrichment: public DGA list (from DGArchive and 360 NetLab).
Parsing module

Get Started
-----------

Our system is built with docker, you need docker/docker compose/nvidia-docker-compose, 
and put your pcap files in ./data. 
Run with
```
nvidia-docker-compose –f   
docker-compose-without-procedur.yml up –d
```
Check the system in http://[your ip]:3000 with 'admin' as the username and 'dnssystem' as the password.

Others
-------------
1. There are 3 configurable parameters:
* timeinterval, the time interval used by the statistics
* topdomain, the top K domain names
* topuser, top K users

2. You can also retrain the detection module with your own data and parameters.

