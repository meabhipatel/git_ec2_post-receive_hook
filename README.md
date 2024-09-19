# Git EC2 Post Receive Hook
This repo tells us how we can setup git on EC2 instance and push code there directly from vs code using git push command
---

# Setps To Connect you EC2 with your local git 
### 1. Connect with EC2 console.
### 2. Go to /var and create a git dir
```sh
cd /var
sudo mkdir git
```

### 3. Create a dir with your project name
```sh
cd /git
sudo mkdir api.alanced.com.git
sudo chown ubuntu -R api.alanced.com.git
```
- Note: Also make sure to give permission to your dir where you want to copy code in this situation `cd ~ && sudo chown ubuntu -R api.alanced.com`

### 4. Initialized an empty git repo
```sh
cd api.alanced.com.git
sudo git init --bare
```
### 5. Go into hooks dir and create a `post-receive` file 
```sh
cd hooks 
sudo nano post-receive
```

### 6. Copy and paste below code into `post-receive` file
```sh
#!/bin/sh
git --work-tree=/home/ubuntu/your_dir_name --git-dir=/var/git/your_dir_name.git checkout -f feature/auth-api
```
- Note : use `-f` if you have to copy code from another branch.
- Also give execute permission to this file `sudo chmod +x post-receive`
  
### 7. Save and Exit the editor with `Ctrl + O` , `Enter` and `Ctrl + X`

### 8. Now you have to add ssh key to your ec2 to connect via ssh
- Generate SSH key (if not already generated) on your local machine:
```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
- Copy the public key to the EC2 instance
```sh
cat ~/.ssh/id_rsa.pub
```
- Copy the output and paste it into the `~/.ssh/authorized_keys` file on your EC2 instance.
- SSH into the EC2 instance
```sh
ssh ubuntu@your-instance-public-ip
```

### 9. Now you have to go into vs code and add git remote then you can push code
```sh
git remote add <origin-name>  ubuntu@instance_public_ip:/var/git/api.alanced.com.git
```
- EX : git remote add production  ubuntu@52.212.199.123:/var/git/api.alanced.com.git


production      ubuntu@54.242.192.123:/var/git/proptechpro.git
### 10. Check you origin added or not 
```sh
git remote -v
```
- You will find like below Example
  `origin  https://github.com/meabhipatel/alanced_be.git (fetch)
origin  https://github.com/abhipatelwiz91/alanced_be.git (push) 
production      ubuntu@52.212.199.123:/var/git/proptechpro.git (fetch)      
production      ubuntu@52.212.199.123:/var/git/proptechpro.git (push)`

### 11. Now you can push you code to the EC2 instance with below command
```sh
git push production
```

