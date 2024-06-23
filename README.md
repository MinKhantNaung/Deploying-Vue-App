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

## Setting up Nginx with SSL  
Install Nginx  
- sudo apt install nginx  
Next, create a new folder to store vue.js application  
- sudo mkdir -p /var/www/vueapp/dist/  
So, whenever we build our vue.js application, a new directory will be created. And it will contains 'index.html' as the compiled version of javascript and css of our vue.js application.  
Next, let's change the ownership of the /var/www  
- sudo chown -R username:www-data /var/www 
We also need to change the file mode of the directory by running this command:  
- sudo chmod g+s /var/www 

#### Next, let's define a new Nginx configuration file.  

- cd /etc/nginx/  
Then, let's create a new file.  
Inside sites-available folder, i will create 'vueapp'.  
- sudo vim sites-available/vueapp  
Let's change the editor to the editor to Insert mode:  
And then , paste codes.  

- server {  
    listen 80;  
    server_name todo.pro www.todo.pro;  
    charset utf-8;  
    root /var/www/vueapp/dist;  
    index index.html index.htm;  

    location / { 
            
        root /var/www/vueapp/dist;  
        try_files $uri /index.html;    
    }  
    error.log /var/log/nginx/vue-app-error.log;  
    access.log /var/log/nginx/vue-app-access.log;
}
If you want to cancel file changes 
- :q!
save
- :qw  
Next, let's make a symlink of Nginx configuration we just created to the site-enabled folder.  
- sudo ln -s /etc/nginx/sites-available/vueapp /etc/nginx/sites-enabled/  
Then, let's unlink default Nginx configuration.  
- sudo unlink /etc/nginx/sites-enabled/default
Next, let's test our Nginx configuration.  
- sudo nginx -t  
will show this 'nginx: the configuration file /etc/nginx/nginx.conf test is successful'  
Restart Nginx Service:  
- sudo service nginx restart 
Now, Let's create a test file to see if our configuration is working as expected.   
- cd /var/www/vueapp/dist/
Create a test file.  
- sudo vim index.html  
I will show '<h1>Hellow World</h1>'  
- :wq  
Go to browser, enter our domain.  
If our domain is unavailable, come back when domain is available.  
After available, you will see 'Hello World': 

#### Let's add SSL  
Now, Certbot recommends using snap package for installation.  So, let's install snap with:  
- sudo snap install core  
It could take a few minute.  
When done, let's execute this command:  
- sudo snap refresh core  
Let's install Certbot using this command:  
- sudo snap install --classic certbot 
It stores in snap/bin/
- sudo ln -s /snap/btn/certbot /usr/bin/certbot  
All right, let's finall set up SSL by executing this command:
- sudo certbot --nginx -d todo.pro
If domain is two.
- sudo certbot --nginx -d todo.pro -d www.todo.pro
Enter email:  
Then let's hit 'y' to accept terms:  
Hit 'n' to cancel  to receive compaigns:   
...Requesting certificate for todo.pro:  
When successfully, go back to browser:  
Let's refresh the page, now we have successfully install SSL.  

## Deploying Vue.js app  
Go to our remote server:  
- cd /var/www/vueapp/
I'll remove 'dist' directory.  
- rm -rf dist  
Let's pull from github repository:  
- git clone remote_url .  
- ls 
Here, we see vue.js application file structure.  
Now, install dependencies. 
- npm install  
Next, copy .env file if you have .env file.  
- cp .env.example .env  
When finish, open .env
- vim .env  
In .env, you can change your set up. 
Next, build npm dependencies.  
- npm run build  
When complete, go to browser. Refresh.  
Finish deploying Vue.js app.  














