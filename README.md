#Jira setup on Ubuntu with Ansible

##Ansible

The directory structure looks like this:
```
|--ansible-jira
   |--group_vars
   |--roles
      |--java
         |--files
         |--handlers
         |--tasks
         |--vars
      |--jira  
         |--files
         |--handlers
         |--tasks
         |--vars
      |--postgres
         |--files
         |--handlers
         |--tasks
         |--vars
```
There are four variables to set before running the playbook, they are the customer name (this creates a user account that runs the jira instance), the database name, password and the Jira version number. The file that needs editing is the **all** file in the group_vars directory.

Example:
```
customer_name: 'steveweb'
db_name: 'steveweb_db'
db_pass: 'password123'
jira_version: '6.4.11'
```  

The Jira installation files also need to be downloaded and placed into the files directory in tar.gz format. They can be obtained from https://www.atlassian.com/software/jira/download. Click 'All JIRA download options' to show the tar.gz archives.

Example:
```
|--ansible-jira
   |--roles
      |--jira
         |--files
            |--atlassian-jira.6.4.11.tar.gz
```

The host(s) that you wish to run the playbook on will need to be added to the **hosts** file in the ansible-jira directory.

Example:
```
[atlashost]
192.168.2.19
```

Once these files have been edited, the playbook can be run against the hosts. The user specified (deployuser in this case) must have sudo privileges on the hosts.

Example:
```
ansible-playbook ansible-jira/site.yml -i ansible-jira/hosts -u deployuser
```

Once the playbook has finished running, you should be able to access the Jira config page using a web browser with port 8080. Note - it may take a couple of minutes for Jira to startup.

Example:
```
http://192.168.2.19:8080
```
