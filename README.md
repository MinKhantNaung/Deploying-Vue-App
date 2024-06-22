# Deploy Vue.js Application In DigitalOcean  
## Setting Up Server 
In DigitalOcean account, click 'Create' button.  
Choose Droplet menu.  
Choose DataCenter region. (Singapore or)  
Choose An Image - (you may select Nodejs Image) (In my case, select Ubuntu LTS version)  
And Choose CPU option:  
And Choose Authentication Method- choose 'Password'  
Then Enter password for 'root':  
In Hostname, give name:  
In Tags, select you want (Vue)  
Then, let's hit 'Create Droplet':  

#### Link Domain 
Click Domain  
In Hostname, enter '@'  
Select droplet  
And then, click 'Create Record'  
And In Hostname, enter 'www'  
And then, click 'Create Record' button again  
#### Login To Our Server and Create new user  
- ssh root@192.168.43.1  (in IP address, you can use 'domain_name' but domain name is too long to ready in internet)  
Enter password:  
##### Add user
- adduser username  
Enter New password:   
Retype new password:  
Full Name, Room Number, Work Phone, Home Phone, Other, (Enter for default)  
##### Next, let's make newly created user to a sudo user  
- usermod -aG sudo username  
- exit  
And then, login with newly created user  
- ssh username@192.168.43.1  
Enter password:  
#### Set up SSH to connect to github  
Generat SSH  
- ssh-keygen -t rsa -b 4096  
Let's use default file to save (Enter)  
Keep passphrase empty:  
Now, copy the public key of ID RSA:  
- cat ~/.ssh/id_rsa.pub  
Copy and go to github account  
In profile, got to setting:  
Click 'SSH and GPG keys'  
Click 'New SSH key'  
Enter the title:  'SSH key for Vue App'  
Paste the SSH key:  
Click 'Add SSH key'  

#### After added, go back to terminal and test SSH connection:  
- ssh -T git@github.com  
Are you sure you want to continue connecting? (yes)  
Now, our server is able to connect to github through SSH.

## Setting Up Node.js  
Login to ssh:  
Before install Node.js, let's update Ubuntu server-  
- sudo apt update  
Enter password:   
When done, finally install Node.js.  
- sudo apt install nodejs (you will get lates nodejs version which is perfect fine)
but I will install (nvm - node version manager)  
Go to browser (https://github.com/nvm-sh/nvm)  
Go to installing and updating section  
You can download 'curl' or 'wget', download with 'curl' in my case.  
Copy 'curl command' and run in command  
- curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash  
Next, to make nvm accessible.  
- source ~/.bashrc  
Now, if we execute
- nvm ls-remote  
We'll see a list of Node.js versions.  
In this case, I'll use latest and LTS version  
So copy version.  
- nvm install v18.16.0  
After install, verify node.js  
- node -v  
And see npm version  
- npm -v  






