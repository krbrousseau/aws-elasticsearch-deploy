# aws-deploy
Deploys an AWS instance and configures the instance for the specified role.

## Goal
Deploy an AWS instance. 
Install, configure, and secure ElasticSearch.

## Run Requirements
* AWS account with valid AWS access keys, ssh key pair, and security groups.
* Python 2.7.x with `boto3`, `cryptography`, and `paramiko` packages.

## Setup
Copy `template_config.json` as `config.json` and modify the following entries : 
```
session.aws_access_key_id      // your access key
session.aws_secret_access_key  // your secret key
session.region_name            // your aws region
ssh_key_path                   // /path/to/.ssh/
roles_location                 // /path/to/aws-deploy/roles
```
You will need the following security groups configured in AWS with these names : 
```
"ssh"            inbound port 22 open to all
"http"           inbound port 80 open to all
"https"          inbound port 443 open to all
"elasticsearch"  inbound ports 9200/9300/9400 open to the "elasticsearch" security group
```

## Deploying
With the config file set up and your security groups created : 
```
$ python deploy.py <role>
```
The instance will be deployed to AWS based on the role specified. 
A setup script and multiple config files will be uploaded to the instance.
The `elasticsearch` role is configured to install and secure ElasticSearch.
On success, the final output will have the IP address of the instance.
To test that ElasticSearch is running, navigate in your web browser to the IP of the instance or run :
```
curl -k -u <user>:<password> -X GET 'https://<IP of the instance>'
```
You should receive a response similar to the following : 
```
{
  "status" : 200,
  "name" : <name generated by ElasticSearch>,
  "cluster_name" : "elasticsearch",
  "version" : {
    "number" : "1.7.3",
    "build_hash" : "NA",
    "build_timestamp" : "NA",
    "build_snapshot" : false,
    "lucene_version" : "4.10.4"
  },
  "tagline" : "You Know, for Search"
}

```
## Resources
 - lots of Googling
 - Boto 3 Docs
 - Elasticsearch docs and discussion posts
 - NGINX docs
 - stackoverflow
 - DigitalOcean Tutorials

### Reasoning, Feedback, and Questions can be found [HERE](notes/thoughts.md)
