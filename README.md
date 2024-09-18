# git_ec2_post-receive_hook
This repo tells us how we can setup git on EC2 instance and push code there directly from vs code using git push command
---

```sh
// post-receive
#!/bin/sh
git --work-tree=/home/ubuntu/proptechpro_rei_crm_be --git-dir=/var/git/proptechpro.git checkout -f feature/acquisition-transaction-api
```
