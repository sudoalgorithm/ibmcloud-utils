
# Pre-Requisites

+ Get a [IBM Cloud account](https://bluemix.net)
+ Install the [IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/ibmcloud/download_cli.html#install_use)


# Step 1 - Install IBM Cloud Infrasctructure plugins

To create VPC, and VSI, install the Infrastructure plug-in.

1. Open a command line utility.

1. Before installing the Infrastructure plugin, we need to add the repository hosting IBM Cloud CLI plug-ins.
    ```
    ibmcloud plugin repos
    ```
    Output:
    ```
    Listing added plug-in repositories...

    Repo Name   URL
    Bluemix     https://plugins.ng.bluemix.net
    ```

1. If you don't see a repository, run the following command:
    ```
    ibmcloud plugin repo-add Bluemix https://plugins.ng.bluemix.net
    ```

1. To install the Infrastructure Service plugin, run the following command:
    ```
   ibmcloud plugin install infrastructure-service
    ```

1. To verify that the plug-in is installed properly, run the following command:
    ```
    ibmcloud plugin list
    ```
    and both plug-ins are displayed in the results:
    ```
    Listing installed plug-ins...

    Plugin Name          Version
    infrastructure-service                 0.1.3
    ```

1. To update the plugin
    ```
    ibmcloud plugin update
    ```


# Step 2 - Connect to IBM Cloud

1. Login to IBM Cloud
    ```
    ibmcloud login
    ```

1. Select the region (API Endpoint) where you deployed your application.

    | Location | Acronym | API Endpoint |
    | ----- | ----------- | ----------- |
    |Germany|eu-de|https://api.eu-de.bluemix.net|
    |Sydney|au-syd|https://api.au-syd.bluemix.net|
    |US East|us-east|https://api.us-east.bluemix.net|
    |US South|us-south|https://api.us-south.bluemix.net|
    |United Kingdom|eu-gb|https://api.eu-gb.bluemix.net|

    >  To switch afterwards to a different region, use the command `ibmcloud target -r eu-de`

 
# Connect to IaaS
```
ibmcloud sl init -c 1433073
```
```
ibmcloud sl vs list
```
 
# Show commands
```
ibmcloud is help
```

# Create VPC
```
ibmcloud is vpcs
```
```
ibmcloud is vpc-create vpc-lma
```
```
ibmcloud is vpcs
```
```
$vpc="9ffa38c7-c59c-4a38-94bb-615b35f58012"
```
 
# Show regions and zones
```
ibmcloud is regions
```
```
ibmcloud is zones us-south
```
```
ibmcloud is zone us-south us-south-2
```
 
# Create subnets
```
ibmcloud is subnet-create Z2-S1-P $vpc us-south-2 --ipv4_cidr_block 10.1.1.0/24
```
```
ibmcloud is subnet-create Z2-S2-A $vpc us-south-2 --ipv4_cidr_block 10.1.2.0/24
```
```
ibmcloud is subnet-create Z2-S3-D $vpc us-south-2 --ipv4_cidr_block 10.1.3.0/24
```
```
ibmcloud is subnets
```
```
ibmcloud is subnets --vpc $vpc
```
```
$p="487df7a9-38aa-45c1-b301-a415b3c59402"
```
```
$a="6c7dc57c-15a9-46f8-8364-a28956c321e6"
```
```
$d="0c8a095c-9c40-4622-91fa-0449e03d0c79"
```
 
# Create public gateway
```
ibmcloud is public-gateway-create Gateway-Z2-S2-A $vpc us-south-2
```
```
ibmcloud is pubgws
```
```
$gateway="7749299f-7503-4fe7-a177-6fbbef95b9a6"
```
```
ibmcloud is subnet-update $a --gw $gateway
```
```
ibmcloud is subnets
```
 
# Add key
```
ibmcloud is key-create --name Eychenne-Steven-IBM --key "ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAht/ujDr180
ejC3uItul59tX6MvlIPvbYqHm9BvhmSDBTK6X0ZGfEBWVq7lRMmHbdj6sluRl09YjmrqqOo5em73wn46OGSWivuDilAKO5MBsgNEtdavaDNAhUZj7MVgpRm6
ATV4HAIDYzk8AIaVUvddDZGIIkVbWL1dXMQ1w2CzoHHlmDIsnwhqxga4xg2yrTCUwgRB6fBTw8T9w8YGH8xNp2V1lVpgph54WHKfgeilPIScxsjLX/6J3qiU
SmicyQY2mmBF7lFYzz1gVxXEeZWYZusydXO1HThA/sMS3+hlVvm60euo2bDDVPtRfwYJuUJFI0zCR4eORYF6i0+pvPzQ==" --rg 1
```
```
ibmcloud is keys
```
```
$key="8cda4a00-946c-47ad-8044-bbc3bb09fbd1"
```
 
# Show images & profiles
```
ibmcloud is images
```
```
$image="cc8debe0-1b30-6e37-2e13-744bfb2a0c11"
```
```
ibmcloud is instance-profiles
```
```
$profile="c-1x1"
```
 
# Create VMs
```
ibmcloud is instance-create P1 $vpc us-south-2 $profile $p 1000 --image $image --keys $key
```
```
ibmcloud is instance-create A1 $vpc us-south-2 $profile $a 1000 --image $image --keys $key
```
```
ibmcloud is instance-create D1 $vpc us-south-2 $profile $d 1000 --image $image --keys $key
```
```
ibmcloud is instances
```
 
# Create floating IP
```
ibmcloud is instance 66f31a3d-09d1-44a9-b7fe-e6b716737273
```
```
$nic="53f8d4e4-5531-4cf5-8388-157ce65ffac6"
```
```
ibmcloud is floating-ip-reserve VIP-P1 --nic $nic
```