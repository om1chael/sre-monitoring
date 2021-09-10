 # Week 5  Vagrant
 ## Topics covered 
 - What is an Site reliability Engineer (SRE)
 - What is the Software development cycle  (SDLC)
 - Cloud computing:
  			 - Different types of cloud models e.g. hybrid, on premise etc ... 
			 - Amazon Web Services  
- vagrant:
	  - Demo with code instructions 
________________
#  What is an SRE ?
## SRE introduction

![What is Netflix?](https://help.nflxext.com/43e0db2f-fea0-4308-bfb9-09f2a88f6ee4_what_is_netflix_1_en.png)

#### Ever wounder how netflix is able to steam thousands of videos without (if ever) issues? 
![File:Netflix area.svg - Wikimedia Commons](https://upload.wikimedia.org/wikipedia/commons/thumb/0/09/Netflix_area.svg/1200px-Netflix_area.svg.png)

 The scale of netflix is global, with Netflix operating in 190 countries. The Netflix product is, of course, streaming videos to consumers however, Netflix isnt alone in the video streaming space. Companies like Disney, Hulo and Amazon have their own streaming services that compete with Netflix so, in order to compete Netflix needs to make sure that their site (https://www.netflix.com) is, as you've guessed it - reliable. When a customer from any of the 190 contries wants to watch a new a show, they are not greeted with an error page. 



 ## SRE in a nutshell  ?
SRE's was created in 2003 by Google. In short an SRE aims to 

> Create scalable and highly reliable software systems

 ## How is this achived? 


![](https://1.bp.blogspot.com/-VXvvkuYrXxc/XHpyqpXi74I/AAAAAAABbfc/rsLgyr3ciu8B6FTj98GwIOSq-XNxn4m6ACLcBGAs/s640/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%2B2019-02-28%2B%25E4%25B8%258B%25E5%258D%25886.14.37.png)

### At its cores SRE has 5 Pratices:
- shared Onership
	-  The dvelopers and operations teams works togther insted of working speartly 
- SLO and Blameless PMs
	-  When issues arise, blaming becomes pointless and only takes away from the time it could take to fix the problem 
- Reduce costs of failure 
	- new features are stratigically appiled to the system, thus reducing cost of failure  
- Automate and Toil 
	-  The reptative taks are automated so time is saved and errors are minimised. This could include using software such as Vagrant to automoate in creating vitual machines.  

## SRE TL;DR
 **Benjamin Treynor** at google, the concept orginator, would describe it as:

> "If Google ever stops working, it's my fault" ~ Benjamin Treynor's linkedin

## 
## Cloud Computing with AWS
### SDLC stages
### Risk factors with SDLC stages
#### Monitoring


### Key Advantages:
- Ease of use
- Flexibility
- Robustness
- Cost

  
**Cloud Computing**

- What is Cloud Computing and the benefits of using it?
	- Cheaper than on site data centres
	- pay for what you use 
	- No maintenance costs 
	-  better data recovery 

**What is Amazon Web Services (AWS)**
- Cloud platform that uses a pay as you go service.
- It was created with Amazon level of scaling
- It used by a 1/3 of the internet for housting
  
  
  

**What is SDLS and stages of SDLC**

what is it: - processe for creating high-quality software
the steps of an SDLS include: 
[Requirement analysis] --> [Planning] --> [Software design] --> [Software development --> [Testing] -->[Deployment]

  

![image](https://user-images.githubusercontent.com/17476059/130943584-713b6d21-0d68-4c39-a223-b14f39b9db00.png)

  
  

**What are the Risk level at each stage of SDLC?**

**Low** - Very unlikely that this will occur during the life of the project
**Medium** - There is a 50-50 chance that this will occur during the life of the project
**High** - Very likely that this will occur during the life of the project e.g. the site is down.

 

### What is on prem, cloud and Hybrid cloud and multi cloud

- on premis:
	- This is a database/server that is localy created and maintained by a company
- Cloud:
	- Cloud is an on-demand computer services, that is avaliable and accessable from anywhere.
- Muti Cloud:
	- Multicloud is the use of multiple cloud computing and storage services in a single diverse architecture.
- add a diagram for each case with real life example or use cases
  
  

- pros and cons of each model

  

on premis:

- pros:
	- Higher degree of security since a user is able to set the security.
	- can be created to meet the needs of the company
	- Offer control over server hardware
- cons:
	- Expensive to set up and maintain

Muti-Cloud (Hybrid) :
- pros:
- cons:

Cloud:
- pros:
- cons:


# Tasks:
## Task 1: Set up the page 
    Get the sparta logo to show up on ip: http://192.168.10.100:3000
(Add picture )
## Task 2: Set up a Reverse Proxy : 
    Set up a Reverse Proxy so the website shows up on the ip: http://192.168.10.100/ (without port 3000 added)
 (Add picture )
## Task 3: Set up the mongodb database
    - Environmental varible \
    - Set up vagrant so, it creates two VMs \
    - Connect the Mongodb VM to the app VM to access the posts page   \
 (Add picture )


____

## Task 1:  Set up the page
### provisioning into vagrant
create **provision.sh** file with this in the file:

``` 
!#/bin/bash
sudo apt-get update -y
sudo apt-get upgrade -y 
sudo apt-get install nginx -y 
export DB_HOST=192.168.10.150:27017/posts

```

in the Bash command line
 > `Vgarant up`\
 > `Vagrant SSH`

### Installing dependencies for the **app VM**
```
sudo apt-get install npm -y
sudo apt-get install python-software-properties -y
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install nodejs -y
```
> Change to the **app** directory  

```
sudo npm install pm2 -y
sudo apt-get install nodejs -y
sudo apt-get install -y nodejs 
```

-  `npm install` (app will throw an error if this is not done)  
-   Run npm:  `npm start`
check browser:  `192.168.10.100:3000`
- you should see the Sparta logo in the centre of the page

- To exit the port 3000 in bash press CTRL-C 

___


## Task 2: Set up a Reverse Proxy

Access the nginx configuration file

> sudo nano /etc/nginx/sites-available/default

Delete the contents in the file and copy this into the file.

```
server {
    listen 80;

    server_name _;

    location / {
        proxy_pass http://localhost:3000;      
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade'; 
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;      
    }
}
```
Save the file 
 `CTRL-O and CTRL-X `

```
sudo nginx -t 
sudo systemctl restart nginx
npm start
```
Check the browser without the 3000 : `192.168.10.100`
is should still work

## Task 3: 
### 3.1 Environmental Variable
You need to create an Environmental varible to connect the database to this App vm. TO make it presisitant you need to make it available in .bashrc by doing: 


>echo DB_HOST=192.168.10.150:27017/posts >> ~/.bashrc

Then `persistent` to make it stick  
>source ~/.bashrc
>connect the DB via the export command" 

`export DB_HOST=192.168.10.150:27017/posts`
Type `env` to check if DB_HOST has been saved in the list of variables. 
If its there, enter the command else skip it, since the command is entered in the provision.sh file.

### 3.2 Setting up the Database VM 

Exit the app vm by typing 
`exit` into the bash command line 
 go into your vagrant file and enter this, following the same file configuration in this repo. 

Run the new Vagrant file: with a new virtual machine (vm) called **db**

The provision DB VM is stored in the provision_DB file:
with the following installations: 
```
sudo apt-get update -y
sudo apt-get upgrade -y
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
sudo apt-get update
sudo apt-get install -y mongodb-org
mongod --repair
sudo service mongod start
sudo service mongod status

```
Vagrant SSH into db and configure some files

If you have mongod installed

`sudo nano /etc/mongod.conf`

`ip 127.0.0.1 change to 0.0.0.0 port: 27017`

Restart mongo
 `sudo systemctl restart mongod`
Check status
`sudo systemctl status mongod`

> Exit the DB vm and SSH into the APP 
> 
### 3.3 Connecting and npming the site 

for the final steps enter:
 ```
 node seeds/seed.js
 sudo systemctl restart nginx
npm start
```
You should see a 
"(node:13914) DeprecationWarning: `open()` " warning above the npm start command, this is fine.

> Go back to the IP address: `192.168.10.100/posts`

you should see a page called **Posts** with meaningless text .

Once you see the page load with meaningless text you have done it - Well done .






![image](https://user-images.githubusercontent.com/17476059/131409334-578de0ea-ab94-4461-87e5-7f73c3b956f6.png)
