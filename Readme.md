#Production Environment Setup
This is ment to be a proof-of-concept showing how to deploy a complete LEMP application environment from scratch using **[Ansible](https://www.ansible.com/)** as the automation tool. This is by no means a complete solution in terms of security..

In this test scenario we will have two servers, one of them will host the web application and the other one will host the database. Even when we are using only one web node, this deployment process would work with any number of web nodes without modification other than the targeting.

For the purposes of this demo we will spin up the two servers in a local environment using **[Vagrant](https://www.vagrantup.com/)** managed virtual machines. After the _servers_ are up and running we will provision and deploy in one step with the provided **[Ansible](https://www.ansible.com/)** playbooks and roles. This provisioning process is _underlying hardware agnostic_, meaning that it would work even if the servers are on-premise, on the cloud or in Docker containers, as long as they are Ubuntu 14.04 LTS operating systems with standard SSH access.

_** Note: this was tested on a workstation running Ubuntu 16.04 Linux distribution._
***


###Required Software
If you already have all the needed software you can skip to the [next section](#headerSS).
The software required in your workstation is the following (tested versions between parentheses):

#### VirtualBox (5.0.18)
Go to [VirtualBox download page](https://www.virtualbox.org/wiki/Downloads)  and install VirtualBox version specific to your workstation's operating system.

#### Vagrant (1.8.5)
Go to [Vagrant download page](https://www.vagrantup.com/downloads.html) and install the Vagrant version specific to your workstation's operating system.

#### Ansible (2.1.1.0)
On Debian based distros run the following:
`sudo apt-get install -y software-properties-common`
`sudo apt-add-repository ppa:ansible/ansible`
`sudo apt-get update`
`sudo apt-get install -y --force-yes ansible`


## <a name="headerSS"></a>Spining up the servers
####Clone this repository 
`git clone git@github.com:oajara/pptyx_prod_env.git`

####Bring up the virtual machines
`cd pptyx_prod_env && vagrant up`

This makes use of the *Vagrantfile* file to build the two virtual machines. If this is the first time you run the command you will probably need to wait for the Ubuntu virtual machine image and add-ons to download from Vagrant repositories. After a few minutes you should have the two virtual machines up and running.

## Provisioning Process
The Database Server is provisioned with two roles:

* role _db_ takes care of installing MySQL server and basic initial housekeeping like changing the root password and deleting the test database.
* role _employees_data_ takes care of importing the [test database](https://github.com/datacharmer/test_db) directly from the repo. It also creates the database user allowed to connect to the _employees_ database.

The Web Server is provisioned from two roles as well:

* role _web_ installs Nginx server and PHP and does some initial housekeeping such as disabling the default virtual host.
* role _nice_app_ sets up the virtual host, deploys the [NiceApp](https://github.com/oajara/pptyx_nice_app) code and sets the configuration parameters.

####Test Ansible connectivity
The VMs IPs are set in the _Vagrantfile_ and are included in the _ansible/inventory_ file. We need to make sure Ansible has control over the target operating systems.

`export ANSIBLE_HOST_KEY_CHECKING=False`

`ansible -i ansible/inventory -m ping dbservers` 

`ansible -i ansible/inventory -m ping webservers`

You should see green light in the latest two commands.

####Provision and Deploy
`ansible-playbook -i ./ansible/inventory ./ansible/deploy.yml`



##Use the environment!
You can browse http://10.100.199.201/ in your browser.


