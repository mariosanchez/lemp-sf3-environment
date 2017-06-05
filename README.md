# LEMP stack environment for Symfony3 applications; With Vagrant and Ansible

This is a simple LEMP environment for Symfony3 applications, but it may be useful for 
any PHP application you want 😃

## Prerequisites
In order to run this environment you need to get have done the following:

1. Download and install the latest VirtualBox version from the 
[official page](https://www.virtualbox.org/wiki/Downloads).
2. Download and install the corresponding Vagrant version from the 
[official page](https://www.vagrantup.com/downloads.html).
3. Install Ansible 
[following the documentation](http://docs.ansible.com/ansible/intro_installation.html).

## Get it running

#### Clone the repo
First you need to clone this repo in the local directory that you decide.

#### Install Ansible third party roles
Now you need to install the Ansible roles for this environment. You will find it
in `requirements.yml` file in case you want to know what it's going to be installed in
your machine. In order to install those roles open a shell promt in the directory 
root and run the following command:
```
$ ansible-galaxy install -r requirements.yml
```

#### Replace Vagrantfile.dist
After that you need to rename (or copy & rename) the `Vagrantfile.dist` to `Vagrantfile` file that you
will find in the directory root. You will find some options that you need to fill base in
your application needs:

* <path_to_your_private_ssh_key>: The path to your SSH private key. 
* <path_to_your_public_ssh_key>: The path to your SSH public key.
* <your_app_folder>: The folder wher your Symfony3 application 
* <your_domain>: The domain you will use later in NGINX configuration

SSH keys are meant to be in `keys` folder, but feel free to put it wherever you want. If
you don't know how to generate them you can check multiple post of the community that may
be useful for you. 
[This is an example that can help you](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2).

#### Replace inventory file .dist
The next step is to rename the `inventory/vagrant.dist` to `inventory/vagrant`. You
will need to replace `<path_to_your_private_ssh_key>` with your private ssh key path
as you did in the `Vagrantfile`. 

Feel free to change other params like IP, but make sure that they are 
consistent with both `Vagrantfile` and `ìnventory/vagrant` config we previously edit.

#### Replace Ansible vars .dist files
We will need to rename the following Ansible vars files too:

* `common.yml.dist` to `common.yml`
* `mysql.yml.dist` to `mysql.yml`

In the `common.yml` you will find some parameters you need to set up for make it run
correctly:

* <your_domain>: Your app domain for NGINX. Make sure is the same as in your `Vagrantfile`.
* <your_document_root>: The document root of your application. By default app.php in Symfony3.
* <your_database_root_password>: The password of the root user of MySQL.
* <mysql_database_name>: The default database name for MySQL.

In the `mysql.yml` you will find some examples of creating databases and users. It's only turned 
into a `.dist` because it depends on your application config.

I encourage you to customize any variable file in `vars` directory in case you need it to fit 
your own case.

#### Run vagrant
Now you need to run the following command in the shell promt:
```
vagrant up
```
This may take some minutes because needs to download the Vagrant box and do all the
setup of the VM and provisioning.

#### Finally

If after the process finished the VM is not provisioned, you'll need to run:
```
vagrant provision
```
It will run Ansible playbook and install all the environment packages and configs.

You can achive the same result running this command in the `ansible` folder:
```
ansible-playbook playbook.yml -i inventory/vagrant
```

If you want to enable the rsync for your application you need to run the following command:
```
vagrant rsync-auto
```
It will enable a watcher for your app folder and sync it automatically with the vagrant synced folder.

It's highly provable you need to perform some MySQL dumps or so for you app to work. Remember 
that you can access to the VM via SSH running:
```
vagrant ssh
```

After that you cant modify you `etc/hosts` file to add your app domain or simply access vía IP:
```
http://192.168.10.12/
```

Enjoy your development environment!! 😉

---

##### Important
This is a general purpose environment for personal and academic projects that I want to 
share with any individual that find it useful for any personal or professional case or 
activity. Feel free to open an issue if you have any doubt or make a PR if you think that
something is missing, wrong, ambiguous or you simply cant improve it 😀
