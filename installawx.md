# AWX Install on Ubuntu and running on Docker
REQUIRED OS:
  - Ubuntu 22
  - 2 core = At lease
  - 4 ram = At lease

AWS Instance type:
  - t2.medium

1. Update the system:
```
sudo apt update -y
sudo apt upgrade -y
```

2. Install the ansible and the packages required to install ansible:
```
sudo apt install python-setuptools -y
sudo apt install python3-pip -y
sudo apt install ansible -y
```

Verify the ansible version
```
ansible --version
```

3. Install docker:
```
sudo apt install docker docker.io -y
sudo apt install docker-compose -y
```

Verify the docker version
```
sudo docker --version
sudo docker-compose --version
```

If you are running from other user, run this. If running with ROOT, don't run this:
```
sudo usermod -aG docker $USER
```

4. Clone the ansible awx repo (it should be less then version 17 to install it using docker compose)
```
sudo apt install git 
sudo git clone https://github.com/ansible/awx.git --branch 17.0.1 --depth 1
```

Change the directory to awx/installer (you will not see this folder in new version, so clone the old version i.e 17)
```
cd awx/installer
```

5. Generate a secret key (will be used later)
```
sudo apt install vim pwgen -y
sudo pwgen -N 1 -s 30
```

Key example:
```
n9MaoO1bT3AfHIb7wYklftSNWDMgAJ
```

8. Now we need to modify the inventory file with a text editor(vim) replacing admin_password and secret_key. Remember admin_password as it is needed to login in AWX web interface later. 
```
sudo nano inventory
```

- Modify the admin_password and secret_key and also username if you want. 
- Example:
```
admin_password=password
secret_key=bSrvLpnSsMMrNBWBDtGbSzz6I4GnD9
```

9. After that implement yml playbook which downloads docker container images and sets them up accordingly.
- Make sure you are in this directory awx/installer running the below command.
- Run the following command to apply ansible playbook.
```
sudo ansible-playbook -i inventory install.yml
```

If you want to see the containers that is running then run:
```
sudo docker ps -a
```
10. You can go to your browser and type http://your-server-ip to access AWX GUI. 
- It will run on port 80
- Sign in by using the admin_user, admin_password in the UI


