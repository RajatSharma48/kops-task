# KOps
KOps or Kubernetes Operations, the sole purpose of KOps is to provide production grade kubernetes cluster that could have the potential to associate with various cloud platforms and stays efficient through out.


# Steps

Before running directly onto the steps for the KOps cluster creation, there are some initial resources and software that would be needed in the way. So without further we do, take a look on below mentioned requirements.

  ### Requirements
  #### 1. Kubectl: 
	sudo apt-get install kubectl
#### 2. AWS cli
	I would suggest you to go for the aws documentation for the aws configuration.
	https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html
## 1. Install KOps
	curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
	chmod +x kops-linux-amd64
	sudo mv kops-linux-amd64 /usr/local/bin/kops
## 2. On to AWS
Sign in to your respective aws console,
### Setup IAM user
    Search for IAM in the services.
    1. Create a user (Add User).
    2. Give the respective name and access you want to assign.
    3. Select the "Attach existing policy directly"
    4. Select the permissions you want to assign to your user.(AdministratorAccess)
    5. Assign the tags if you want.
    6. Save your Access Key and Secret Key.
    7. Now on your terminal, implement "$aws configure"
  ### Setup your DNS
    Search of Route 53 in services.
    1. Look for DNS management.
    2. Create Hosted Zone.
    3. Assign the Domain name and also assign the zones if you are selecting the private DNS.
    4. On your terminal, implement "dig ns <your-domain-name>" to verify.
  ### Setup your S3 bucket
    You could either choose your terminal or Search "S3" in the services.
    1. Create Bucket.
    2. Specify name and regions.
    3. Copy the details of the bucket that could be used while creating the cluster.
						    Or
    1. On your terminal, implement "aws s3api create-bucket --bucket <name> --region <name-of-region>"

## 3. Cluster Creation
#### Note:
    1. You can use environment variables for the arguements that want to add in your cluster.
    Like export NAME=xyz.com so that you could use them directly by $NAME.
    And same goes for state, export KOPS_STATE_STORE=s3://<output-you-got-from-s3api>
    2. You can choose your nodes sizes, zones, networking,etc.
    3. Try to use the same name as of your DNS name.
    
  #### Commands
  ##### Creation
    kops create cluster --dns private \ 
    --master-size <size-choice> \
    --node-size <size-choice> \
    --networking <name-of-networking> \
     --topology <private|public> \
     --zones <name-of-zones> \
     --name $NAME \
     --state $KOPS_STATE_STORE \
     --yes
 Do validate your cluster once after the successfully creating the cluster. It will take minutes to get your cluster up and running.  
 ##### Validation
 
    kops validate cluster
You can also update your cluster if you want.
 ##### Update
    kops update cluster $NAME --yes

Now, if you are done with creating and setting up your cluster, you could use "kubectl" to implement
kubernetes functionalities.
