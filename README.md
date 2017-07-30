# cf-learn

The repository contains Cloudformation and Ansible code to spin up a Simple Web and DB Stack with ELB and Internet Gateway. Deployment of Cloudformation stack is done using Ansible `cloudformation` module.

## How to Deploy

To deploy the Cloudformation stack with this repo you need Ansible installed.

```
pip install ansible
```

or if you are running `RHEL` variants use `EPEL` repository.

```
# centos example
yum install epel-release
yum install ansible
```

Clone this repository to your local machine and customize the vars in `ansible/stack-deploy.yml` as needed.

```
aws_access_key: Access Key of the AWS Account
aws_secret_key: Secret Key of the AWS Account
base_stack_name: CF Stack Name for the Base Stack
shared_stack_name: CF Stack Name for the Shared Stack
```

We are installing Wordpress with MySQL Backend. Use the below vars to customize the wordpress parameters.

```
wordpress_db_user: ""
wordpress_db_pass: ""
wordpress_db_name: ""
```

Use `Ansible-Vault` to encrypt the secrets if you are planning to push the customized code back to SCM.

Use the below command to deploy the stack.

```
ansible-playbook -c local stack-deploy.yml
```

## Output

If you have only tweaked the AWS Credentials this is what you will get from following the above steps.

```
# CloudFormation Stacks
TechChallenge-ELB	- Cloudformation Template for Creating FrontEnd Services
TechChallenge-AppInstance2 -	Cloudformation Template for Creating App Instances
TechChallenge-AppInstance1 - Cloudformation Template for Creating App Instances
TechChallenge-DBInstance2	- Cloudformation Template for Creating DB Instances
TechChallenge-DBInstance1	-	Cloudformation Template for Creating DB Instances
TechChallenge-Shared - CloudFormation Template for Tech Challenge: Shared Layer [Security Group, Subnets]
TechChallenge-Base - AWS CloudFormation Template for Tech Challenge: Base Layer [VPC,Gateways,NATS]
```

The `UserData` of the instances will bring up `MySQL` and `WordPress` containers.

You can hit the ELB URL to access the WordPress instances. Since this is a fresh install, it will take you to the WordPress Installation Wizard.

### TODO

* Enable NACLs in the VPC
* Investigate more on the Ansible `docker_container` module bug.
  * https://github.com/ansible/ansible/issues/20492
  * https://github.com/ansible/ansible-modules-core/issues/4246
* Enable TLS for ELB
* Add feature to tear down the stack in Ansible Playbook
* Add Ansible Docker Module in UserData
