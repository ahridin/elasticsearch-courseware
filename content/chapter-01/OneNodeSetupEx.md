# One Node Setup Exercise #

* Login into your sandbox
* Update distro using terminal window:
```
sudo apt-get update && sudo apt-get install apt-transport-https -y
```
* Install Java Runtime Environment using terminal window
```
sudo apt-get install default-jre -y
```
* Download and install Public Signing Key:
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
* Add repository definition:
```
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```
* Install Elastic Search:
```
sudo apt-get update && sudo apt-get install elasticsearch
```
* We are running on smaller than recommended hardware, need to adjust jvm.options:
```
sudo nano /etc/elasticsearch/jvm.options
```
* Locate '-Xms2g' and '-Xmx2g' two lines
* Replace with: '-Xms512m' and '-Xmx512m'
* Save the file by pressing: 'Ctrl-O' and then '<Enter>
* Exit nano editor by hitting 'Ctrl-X'
* Start elasticsearch service:
```
sudo service elasticsearch start
```
* Give it a moment to finish the initialization and verify it is running:
```
curl localhost:9200
```
* Expected response:
```
{
  "name" : "Wrecker",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "6j8V904DQIez9oBpejTbGQ",
  "version" : {
    "number" : "2.4.1",
    "build_hash" : "c67dc32e24162035d18d6fe1e952c4cbcbe79d16",
    "build_timestamp" : "2016-09-27T18:57:55Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.2"
  },
  "tagline" : "You Know, for Search"
}
```
* Posting first document:
```
curl -XPOST localhost:9200/ordering/order/1 -d ' {
  "id": "1", 
  "placedOn": "2016-10-17T13:03:30.830Z"
}'
```
* Expected Response:  
{"_index":"ordering","_type":"order","_id":"1","_version":1,"_shards":{"total":2,"successful":1,"failed":0},"created":true}
* First queries:
```
curl 'localhost:9200/ordering/order/_search?q=id:1'
curl 'localhost:9200/ordering/_search?pretty=true&id:1'
```
* Please review results - where are: doc id, document data, index name, type name, and search score? 