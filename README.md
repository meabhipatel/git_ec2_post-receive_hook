# Git EC2 Post Receive Hook
This ReadMe tells us how we can setup git on EC2 instance and push code there directly from vs code using git push command
---

## Setps To Connect your EC2 with your local git 

### 1. Connect with EC2 console.

### 2. Create a working project directory where you want to copy the code after push.
```sh
cd ~
sudo mkdir theabhipatel.com
sudo chown ubuntu -R theabhipatel.com
```
- Make sure to give correct permission to your working dir

### 3. Go to /var and create a repositories directory.
```sh
cd /var
sudo mkdir repositories
```

### 4. Create a directory with your project name
```sh
cd repositories
sudo mkdir theabhipatel.com.git
sudo chown -R ubuntu:ubuntu theabhipatel.com.git
chmod -R 775 theabhipatel.com.git
```

### 5. Initialized an empty git repo
```sh
cd theabhipatel.com.git
sudo git init --bare
```

### 6. Go into hooks dir and create a `post-receive` file 
```sh
cd hooks 
sudo nano post-receive
```

### 7. Copy and paste below code into `post-receive` file
```sh
#!/bin/sh
git --work-tree=/home/ubuntu/your_dir_name --git-dir=/var/repos/your_dir_name.git checkout -f <branch_name>
```
- Note : use `<branch_name>` if you have any other branch except master to copy code from.

  
### 8. Save and Exit the editor with `Ctrl + O` , `Enter` and `Ctrl + X`. Also give execute permission to this file.
```sh
sudo chmod +x post-receive
```

### 9. Now you have to add ssh key to your ec2 to connect via ssh
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

### 10. Now you have to go into vs code and add git remote then you can push code
```sh
git remote add <origin-name>  ubuntu@instance_public_ip:/var/repositories/theabhipatel.com.git
```
- EX : git remote add production ubuntu@52.212.199.123:/var/repositories/theabhipatel.com.git

### 11. Check your  remote added or not 
```sh
git remote -v
```
- You will find like below Example  
```
origin  https://github.com/meabhipatel/theabhipatel.git (fetch)    
origin  https://github.com/meabhipatel/theabhipatel.git (push)      
production      ubuntu@52.212.199.123:/var/repositories/theabhipatel.com.git (fetch)           
production      ubuntu@52.212.199.123:/var/repositories/theabhipatel.com.git (push)
```

### 12. Now you can push your code to the EC2 instance with below command
```sh
git push production
```

## Additional Step 
If you are using docker-compose to run application and want to rebuild autmaticaly  docker compose after pushing code to the instance follow this below step and udpate your `post-receive` file as follows.
```
echo "Restarting Docker Compose..."
cd /home/ubuntu/theabhipatel.com
docker-compose down
docker-compose up -d --build
```
