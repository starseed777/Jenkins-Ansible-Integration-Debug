This is documentation for another bug we have encountered in our current project, this is going to document the process to debugging and how the solution was found.

- The issue: Our second ansible playbook that was supposed to execute our terraform scripts and make a new dynamo DB table was failing.

- So this is what the playbook threw for the dyanmo task >> TASK [dynamodb_table] fatal: [localhost]: FAILED! => {"changed": false, "msg": "boto required for this module"}

- It basically says it right there that "BOTO" is required though we have boto3? so I simply changed our task from the very first playbook "s3-bucket.yml" and changed our boto3 task to just install boto instead

- After doing so the dynamo db table gets sucessfully created but the rest of the jenkins job fails because it throws this new error:
TASK [runs terraform scripts] fatal: [localhost]: FAILED! => {"changed": false, "msg": "Failed to find required executable terraform in paths: /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin"}

- So I checked my executible path in the jenkins server and everything looked right but remember how with anisble we had to install it to our jenkins server, I'm thinking if I install terraform then the executable file will be there in this path to successfully finish this jenkins job so I will be installing terraform to the jenkins server now following this installation guide >> https://www.terraform.io/docs/cli/install/yum.html (in the documentation they used $register in some of the commands just change that to AmazonLinux when typing out those commands) 

- After doing so the error has been solved but the jenkins job threw me an error for the way I was invoking my playbook command with the extra vars so I just fixed a minor syntax error in my Jenkinsfile by doing string concatanation of my extra vars command after calling my playbook function 

- Jenkins job sucessfully passes >> exit criteria met refer to screenshots
