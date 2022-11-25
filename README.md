# 2420_week12_Lab

## Team Members

Sethavan Sen and Coburn Nguyen

# Setting Up a NGINX Server

#### **Important:** If you see "{content}", do not run as it is, this indicates your information.

## Setup

1. Create a new droplet in Digital Ocean
2. Create a private/public key for the server
3. Make sure you are able to ssh into your new server
4. Create a regular user for the server
5. Disable root log in
6. Now you have setup your server, we can get started!

## Install nginx
This step will provide steps on installing your nginx server and check if it is running properly

1. On your local host run `ssh -i {path/of/key} {USER}@{IP address}` to log into your server
2. Run ``sudo apt update && sudo apt upgrade``

![Install nginx](/images/1.png)

3. When you see this library box pop up, press ENTER

![Install nginx](/images/2.png)

4.Run `sudo apt install nginx`

![Install nginxtest](/images/3.png)

5. When you see the library box pop up again, press ENTER

### Check If Your nginx Is Running

You want to check if your nginx is running so you can proceed to the next steps below smoothly. To check:

1. Run `systemctl status nginx`

![Check If Your nginx Is Running](/images/4.png)

2. Another way to check is to type the IP address in your web browser

It should look like this!

![Check If Your nginx Is Running](/images/5.png)

If both methods work, great! We can move on to the next step!

## Create an HTML Document

This HTML document will serve the main page for your web server. Creating the HTML document will be created on your local wsl host.

1. Create an "index.html" document by running `vim index.html` in your desired directory

![Create an HTML Document](/images/6.png)

2. Write a simple HTML document, this is a simple HTML document you can use, I will be using a different HTML document so the web page will look different from mine.

```
<!DOCTYPE html>
<html lang="en">
<html>
    <head>
        <meta charset="UTF-8" />
        <title>Web-one Lab</title>
    </head>
    <body>
        <h1>Success</h1>
        <h2 style="color: red;">All your internets are belong to us!</h2>
    </body>
</html>

```

## Create an nginx Server Block

The nginx server block will be used to serve the HTML document. This server block will also be created on your local wsl host.

1. In the same directory where you created the "index.html" file, create a server block and name the file to the server's ip by running `vim {IP address}`

-![Create an nginx Server Block](/images/7.png)

2. In the server block, add the following content:

**Note:** Do not leave "{IP adress}" as it is, it indicates that you have to put your ip address down

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/{IP address}/html;
        index index.html;

        server_name {IP address};

        location / {
                try_files $uri $uri/ =404;
        }
}
```

It should look like this!

![Create an nginx Server Block](/images/8.png)

## Upload Files to Server

Once you have the two files: index.html and your IP address in the same directory, you can now transfer the files from your local wsl host to the web server.

1. In your local wls host, run: `rsync -auvz -e "ssh -i {path/of/the/key}" {directory/of/files/on/host} {user}@{IP address}:{directory/of/web_one/server}`

It should look like this.

![Upload Files to Server](/images/9.png)

### Check If Files Has Transfered

Now the files has transfered, lets check if it was done correctly.

1. ssh into your web server by running `ssh -i {path/of/key} {user}@{IP address}`. Enter a your passphrase if prompt.

![Check If Files Has Transfered](/images/10.png)

2. Now you are in your web server, run `ls` in your web server directory.

![Check If Files Has Transfered](/images/12.png)

### Moving the files

Now you have checked if the files has tranfered correctly, now you want to move the index.html and your IP address in the correct directories.

1. Copy your server block (The file with your IP address) file into /etc/nginx/sites-available by running `sudo cp {location/of/server block} /etc/nginx/sites-available/`

![Moving the files](/images/13.png)

2. Create a soft link to your new server block by running `sudo ln -s /etc/nginx/sites-available/{IP address} /etc/nginx/sites-enabled/`

![Moving the files](/images/13.png)

3. Create a directory named after your IP address and an html directory within that directory by running `sudo mkdir /var/www/{IP address} && sudo mkdir /var/www/{IP address/html}`

![Moving the files](/images/15.png)

5. Copy your index.html file into the directory you just created by running `sudo cp {file/path/of/index.html} /var/www/IP-addr/html/`

![Moving the files](/images/16.png)

6. Check if both files are in the correct directory by running `ls /etc/nginx/sites-enabled && ls /var/www/{IP address}/html` It should show the file of your IP address and the index.html file

![Moving the files](/images/17.png)

## Start/Restart nginx Server

Now that we have all the files in the correct directories, We can now start the nginx server.

1. run `sudo nginx -t`

![Start/Restart nginx Server](/images/18.png)

2. Restart your server by running `service nginx restart`

![Start/Restart nginx Server](/images/19.png)

## Check If Server Is Running

To check if the server is running, type your web server IP address into the browser.

![Check If Server Is Running](/images/20.png)

Your HTML document should now be rendered!

## Set up Firewall

Now you have the server running, we will be creating a firewall through digital ocean.

1. Visit <https://www.digitalocean.com/> and login
2. In the Manage dropbox located on the lect side of the website, click Networking

![Set up Firewall](/images/21.png)

3. Click Firewalls and click Create Firewall

![Set up Firewall](/images/22.png)

4. In the Inbound Rules, add an HTTP type

![Set up Firewall](/images/23.png)

5. Under Apply to Droplets, add your web server in the search box

![Set up Firewall](/images/24.png)

6. You can confirm you have made a firewall by going to networking and click firewalls.

![Set up Firewall](/images/25.png)

## After Firewall

Now you have setup your firewall, you want to check if you can still ssh to your web server and check if the server is still running.

1. On your local host run `ssh -i {path/of/key} {USER}@{IP address}` and if you can still ssh in your web server, this is a good sign

![After Firewall](/images/26.png)

2. Lastly, type your web server IP address into the browser and check it is running.

![After Firewall](/images/27.png)

Awesome! You have successfully created your first nginx server.



