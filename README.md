# Project OverView
#### The Purpose of the project is to deploy python-app with redis as a database on a AWS cluster using Terraform to create the environment.
#


## components

#### - VPC
#### - Subnets :
##### Public-Subnet 
##### Private-Subnet
#### - NAT gateway
#### - Private standard EKS cluster
#### - EKS-node
#### - JumpHost - Public instance
#



## 1) Building the Environment using Terraform
### Apply the code using :
```
terraform plan 
terraform init
terraform apply
```

#

## 2) using ansible for configuration 

```
ansible-playbook playbook.yaml -i inventory.txt

```


#



## 3) Using Jenkins to build&push images
### Step:

```
steps {
                git ' https://github.com/AmrGomaa96/final/tree/main/final-pro'
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                cd DevOps-Challenge-Demo-Code-master
                docker build -f Dockerfile -t amr966/app:v1 .
                docker push amr966/app:v1
                """
            }
        }
```


## 4) Using Jenkins to deploy app
### Apply the code using :
```
steps{
                     withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                cd kubernetes-files/
                kubectl create namespace app
                kubectl apply -f deployment-redis.yml
                kubectl apply -f redis-service.yml
                kubectl apply -f app-deployment.yml
                kubectl apply -f lb.yml

                """
                     }
            }
```


