# Elasticsearch Notes

## Create Certificates
### Create CA
[elastic@person certs]$ /home/elastic/elasticsearch/bin/elasticsearch-certutil ca --out config/certs/ca --pass <set-a-password>

### Create Certificates for Nodes
/home/elastic/elasticsearch/bin/elasticsearch-certutil cert --ca config/certs/ca --ca-pass elastic_la --name master-1 --out config/certs/master-1 --pass <set-a-password>


### Transfer Certificates to other Nodes of Cluster
Remember to update the permissions of the files. In this case "data-1" is the directory
As a root user, change the user: 
chown elastic:elastic data-1
Now you can move it to the certs directory within the config directory of elasticsearch as the "elastic" user
mv /tmp/data-1 elasticsearch/conf/cert

## Secure a Cluster
As elastic user, add password to keystore
Transport
bin/elasticsearch-keystore add xpack.security.transport.ssl.keystore.secure_password
bin/elasticsearch-keystore add xpack.security.transport.ssl.truststore.secure_password

Client network
bin/elasticsearch-keystore add xpack.security.http.ssl.keystore.secure_password
bin/elasticsearch-keystore add xpack.security.http.ssl.truststore.secure_password

Validate the entries were added to the keystore
bin/elasticsearch-keystore list

Update the elasticsearc.yml file
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate 
xpack.security.transport.ssl.keystore.path: certs/<dir-name>
xpack.security.transport.ssl.truststore.path: certs/<dir-name>
