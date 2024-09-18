# Git EC2 Post Receive Hook
This repo tells us how we can setup git on EC2 instance and push code there directly from vs code using git push command
---

# Setps To Connect you EC2 with your local git 
1. Connect with EC2 console.
2. Go to /var and create a git dir
```sh
cd /var
sudo mkdir git
```
3. Create a dir with your project name
```sh
cd /git
sudo mkdir api.alanced.com.git
```
4. Initialized an empty git repo
```sh
cd api.alanced.com.git
sudo git init --bare
```
5. Go into hooks dir and create a `post-receive` file 
```sh
cd hooks 
sudo nano post-receive
```

6. Copy and paste below code into `post-receive` file
```sh
#!/bin/sh
git --work-tree=/home/ubuntu/your_dir_name --git-dir=/var/git/your_dir_name.git checkout -f feature/auth-api
```
- Note : use -f if you have to copy code from another branch.
  
7. Save and Exit the editor with `Ctrl + O` , `Enter` and `Ctrl + X`
8. 
