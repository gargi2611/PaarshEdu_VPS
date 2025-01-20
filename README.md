# PaarshEdu_VPS
Documentation

## Deploying MERN Stack Project on Hostinger VPS




- Preparing the VPS Environment
- Setting Up the MySql Database
- Deploying the Express and Node.js Backend
- Deploying the React Frontends
- Configuring Nginx as a Reverse Proxy
- Setting Up SSL Certificates


### 1. Preparing the VPS Environment

Log in to Your VPS in Terminal 

```bash
 ssh root@your_vps_ip
```

Update and Upgrade Your System

```bash
  sudo apt update
```
```bash
  sudo apt upgrade -y
```

Install Node.js and npm ( if not pre-installed)

```bash
  curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash -
```
```bash
  sudo apt-get install -y nodejs
```
Install Git 
```bash
  sudo apt install -y git
```


###  2. Setting Up the MongoDB Database

## Setup MySQL on VPS
### Install MySQL

```bash 
sudo apt update 
```
```bash 
sudo apt install mysql-server -y 
```


### Secure MySQL Installation
```bash 
sudo mysql_secure_installation 
```

Follow the prompts to set a root password, remove anonymous users, disallow remote root login and remove the test database.

### Start and Enable MySQL Service

```bash 
sudo systemctl start mysql 
```
```bash 
sudo systemctl enable mysql 
```

### Verify MySQL Service Status

```bash 
sudo systemctl status mysql 
```


###Configure MySQL Authentication (Optional)
Allowing MySQL Through Firewall
Enable firewall:


```bash 
sudo ufw enable
 ```


Check if the MySQL port (3306) is allowed:
```bash 
sudo ufw status 
```

If not allowed, add the rule:
```bash 
sudo ufw allow 3306 
```
```bash 
sudo ufw allow 'OpenSSH' 
```


Restart MySQL:


```bash 
sudo systemctl restart mysql
 ```

### Secure MySQL by Setting Up a Superuser
Connect to MySQL Shell


```bash 
sudo mysql 
```

Create a Superuser

```bash
 CREATE USER 'username_here'@'localhost' IDENTIFIED BY 'password_here'; GRANT ALL PRIVILEGES ON *.* TO 'username_here'@'localhost' WITH GRANT OPTION; FLUSH PRIVILEGES;
 ```

Exit MySQL Shell

```bash 
exit 
```

### Create Database and User for the Project
Connect to MySQL Shell

```bash
 sudo mysql
 ```

Create a Database


```bash 
CREATE DATABASE database_name; 
```

Create a User with Permissions
```bash 
CREATE USER 'project_user'@'%' IDENTIFIED BY 'password_here'; GRANT ALL PRIVILEGES ON database_name.* TO 'project_user'@'%'; FLUSH PRIVILEGES;
 ```

Exit MySQL Shell

```bash 
exit 
```


MySQL URI to Connect with Projects
```bash 
mysql://project_user:password_here@127.0.0.1:3306/database_name
 ```


### 3. Deploying the Express and Node.js Backend

Clone Your Backend Repository

```bash
 mkdir /var/www
```

```bash
 cd /var/www
```
```bash
 git clone https://github.com/yourusername/your-repo.git
```
```bash
 cd your-repo/backend
```

Install Dependencies

```bash
 npm install
```
Create .env file & configure Environment Variables

```bash
 nano .env
```

add environment variables then save and exit (Ctrl + X, then Y and Enter).


Installing pm2 to Start Backend

```bash
 npm install -g pm2
```
```bash
 pm2 start server.js --name project-backend
```
Start Backend on startup
```bash
 pm2 startup
```
```bash
 pm2 save
```
Allowing backend port in firewall 

```bash
 sudo ufw status
```
If firewall is disable then enable it using 
```bash
 sudo ufw enable
```
```bash
 sudo ufw allow 'OpenSSH'
```
```bash
 sudo ufw allow 4000
```

### 4. Deploying the React Frontends

Creating Build of React Applications
```bash
 cd path-to-your-first-react-app
```
```bash
 npm install
```
If you have ".env" file in your project

Create .env file and paste the variables
```bash
 nano .env
```
Create build of project
```bash
 npm run build
```

Repeat for the second or mulitiple React app.

Install Nginx

```bash
 sudo apt install -y nginx
```

adding Nginx in firewall

```bash
 sudo ufw status
```
```bash
 sudo ufw allow 'Nginx Full'
```


Configure Nginx for React Frontends


```bash
 nano /etc/nginx/sites-available/yourdomain1.com.conf
```

```bash
 server {
    listen 80;
    server_name yourdomain1.com www.yourdomain1.com;

    location / {
        root /var/www/your-repo/frontend/dist;
        try_files $uri /index.html;
    }
}
```
Save and exit (Ctrl + X, then Y and Enter).

Create a similar file for the second or multiple React app.

```bash
 nano /etc/nginx/sites-available/yourdomain2.com.conf
```

```bash
server {
    listen 80;
    server_name yourdomain2.com www.yourdomain2.com;

    location / {
        root /var/www/react-app-2/dist;
        try_files $uri /index.html;
    }
}
```

Create symbolic links to enable the sites.

```bash
ln -s /etc/nginx/sites-available/yourdomain1.com.conf /etc/nginx/sites-enabled/
```

```bash
ln -s /etc/nginx/sites-available/yourdomain2.com.conf /etc/nginx/sites-enabled/
```

Test the Nginx configuration for syntax errors.

```bash
nginx -t
```

```bash
systemctl restart nginx
```

### 5. Configuring Nginx as a Reverse Proxy

Update Backend Nginx Configuration

```bash
nano /etc/nginx/sites-available/api.yourdomain.com.conf
```
```bash
server {
    listen 80;
    server_name api.yourdomain.com;

    location / {
        proxy_pass http://localhost:4000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Create symbolic links to enable the sites.

```bash
ln -s /etc/nginx/sites-available/api.yourdomain.com.conf /etc/nginx/sites-enabled/
```

Restart nginx

```bash
systemctl restart nginx
```

### Connect Domain Name with Website

Point all your domain & sub-domain on VPS IP address by adding DNS records in your domain manager 

Now your website will be live on domain name

### 6. Setting Up SSL Certificates 

Install Certbot

```bash
sudo apt install -y certbot python3-certbot-nginx
```

Obtain SSL Certificates

```bash
certbot --nginx -d yourdomain1.com -d www.yourdomain1.com -d yourdomain2.com -d api.yourdomain.com
```

Verify Auto-Renewal

```bash
certbot renew --dry-run
```


# Video Hosting

## Step 1. Hostinger's File Manager to upload video files to VPS. 
      Navigate to the directory where to store videos 

Step 2. Set Up a Video Player ( Video.js )

```bash
   {JS}     <link href="https://vjs.zencdn.net/7.11.4/video-js.css" rel="stylesheet" />
     <script src="https://vjs.zencdn.net/7.11.4/video.min.js"></script>
```  

Step 3. Embed Videos in Website: Use the video player to embed videos in the desired location on website.
using Video.js:

```bash
     <video id="my-video" class="video-js" controls preload="auto" width="640" height="264" data-setup="{}">
       <source src="https://domain.com/videos/array_1.mp4" type="video/mp4" />
     </video>
```


Step 4 : Add Video Lectures
Upload Videos to VPS:
Use SCP or FTP to upload video files to a directory on your VPS (e.g., /var/www/videos).
Serve Videos with Nginx:

Add a location block in the Nginx configuration to serve videos:
```bash 
location /videos/ { alias /var/www/videos/; } 
```

Restart Nginx:

```bash 
sudo systemctl restart nginx 
```
Link Videos in  Website:
In React frontend, link the videos using the URL:

```bash 
http://your-domain.com/videos/video-name.mp4 
```

Step 5. Configure Server Settings : Ensure server is configured to handle video streaming. This may involve adjusting settings in Apache to allow for larger file sizes and proper MIME types.


Open the Nginx configuration file, typically located at /etc/nginx/nginx.conf or in a site-specific file under /etc/nginx/sites-available/.


   ```bash 
 sudo nano /etc/nginx/nginx.conf
 ```

     
Add MIME Type for Video Files: 
```bash 
http {
    include       mime.types;
    default_type  application/octet-stream;

    types {
        video/mp4    mp4;
        video/webm   webm;
        video/ogg    ogg;
    }

    # Other configurations
}
```


Streaming often involves large video files. Adjust the client_max_body_size setting to allow large uploads (if needed) and configure sendfile for efficient file delivery.

```bash
server {
    listen 80;
    server_name yourdomain.com;

    root /path/to/your/videos;

    location / {
        client_max_body_size 100M;  # Adjust as needed
        sendfile on;
    }

    location ~ \.(mp4|webm|ogg)$ {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Accept-Ranges' 'bytes';
    }
}
```




   
  # Optional

# --------------->>>>>>> partially content delivery [Byte-Range Requests (for Streaming)] <<<<<<<-----------------------

```bash
location /videos/ {
    root /path/to/videos;
    add_header 'Accept-Ranges' 'bytes';
}
```

Test

```bash 
sudo nginx -t
```


Reload Nginx

```bash
sudo systemctl reload nginx
```




###### CDN services that can be integrated with VPS.




 other just for few videos to host them

{   


Add video to cloud based storage like Google Drive


then click on share – access to anyone

then click on embedded item 


} 
