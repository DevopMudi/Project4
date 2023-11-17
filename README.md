# Project4
LEMP STACK IMPLEMENTATION

**Required Steps**

- Step 1 : Sign into your AWS account
 ![image](https://github.com/DevopMudi/Project4/assets/149855241/d6994eb4-4afe-46ea-9f68-d592e1361c85)


- Step 2: launch an EC2 instance with Ubuntu OS pre install and connect with the bash

![image](https://github.com/DevopMudi/Project4/assets/149855241/c8112826-0b6d-4fdf-870c-8d6f23f238ed)

![image](https://github.com/DevopMudi/Project4/assets/149855241/2b76acf5-3912-4a4d-94f0-e75c207aa6c4)

![image](https://github.com/DevopMudi/Project4/assets/149855241/b4dc8a37-083f-41aa-8ff9-8de3707096bf)

![image](https://github.com/DevopMudi/Project4/assets/149855241/f877f6f5-f9c7-4268-bda3-59c7b53e29e6)





- Step 3 : Install Nginx, the web server
  
To update and install nginx, use the command : **sudo apt update**  , after updating the advance package tool(apt), you install nginx with : **sudo apt install nginx**
Note : After installing the nginx , confirm if its active and running with this command: **sudo systemctl status nginx** if it shows running **active**
then we are good to go.

![image](https://github.com/DevopMudi/Project4/assets/149855241/9a33345e-9fb3-4553-8658-625d5c2b0cd7)

![image](https://github.com/DevopMudi/Project4/assets/149855241/446f6883-97b9-4214-974b-9dc4100bf38e)

![image](https://github.com/DevopMudi/Project4/assets/149855241/32a4fc5f-5ac0-45d4-818e-d775b106a4f8)

![image](https://github.com/DevopMudi/Project4/assets/149855241/d25e95f3-1e9b-4cdc-9e60-289954ebaace)



Edit inbound rule in AWS instance to http and save

![image](https://github.com/DevopMudi/Project4/assets/149855241/7995ec6b-a456-4bc7-8f81-4f919f18ea33)

![image](https://github.com/DevopMudi/Project4/assets/149855241/3d2f681a-3127-43b0-a95c-c1319bb42b54)


 Enter the public IPv4 address on your browser you should see the below image, then we are good

 ![image](https://github.com/DevopMudi/Project4/assets/149855241/4ccbfe5d-d8ed-4bb6-a563-3459e755a051)


 
- Step 4 : Install mysql, (for data base)

  Use the command: **sudo apt install mysql-server** and when prompted,do you want to continue, type in y

  ![image](https://github.com/DevopMudi/Project4/assets/149855241/aef5cf25-9e37-4398-ace7-e964eb414861)

  ![image](https://github.com/DevopMudi/Project4/assets/149855241/0fbee677-29a5-461c-bbd6-a8075058c5ec)

  After installation login with **sudo mysql** just to check if its install and exit when you are in

  ![image](https://github.com/DevopMudi/Project4/assets/149855241/f7529fd8-32ec-4343-9c7d-b48240db7462)

Note: its recommended we run a security script to ensure mysql is secure however we will skip that for now and lets work with the root.

- Step 5 : Install  PhP

We have Nginx installed to serve our content and MySQL installed to store and manage your data. Now we will install PHP to process code and generate dynamic content for the web server.
Note: **While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies**

To install these 2 packages at once, run: **sudo apt install php-fpm php-mysql** When prompted, we type Y and press ENTER to confirm installation.
We now have our PHP components installed. Next, we will configure Nginx to use them.

![image](https://github.com/DevopMudi/Project4/assets/149855241/dd5a511d-6a01-405f-a8c0-1d3809c1a9a7)


- Step 6: Configuring Nginx to Use PHP Processor

Note: When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use projectLEMP as an example domain name.
On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

Create the root web directory for your_domain as follows:

**sudo mkdir /var/www/projectLEMP**

Next, assign ownership of the directory with the $USER environment variable, which will reference our current system user:

**sudo chown -R $USER:$USER /var/www/projectLEMP**

Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:

**sudo nano /etc/nginx/sites-available/projectLEMP**

![image](https://github.com/DevopMudi/Project4/assets/149855241/2db4ec4f-b867-450b-8131-4aadcae44c15)


This will create a new blank file. Paste in the following bare-bones configuration

![image](https://github.com/DevopMudi/Project4/assets/149855241/2e5691d0-d09e-4f49-bf65-67fc3312ddb7)

When we’re done editing, save and close the file. If we’re using nano, we can do so by typing CTRL+X and then y and ENTER to confirm.

Lets activate our configuration by linking to the config file from Nginx’s sites-enabled directory with the below command:

**sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/**

This will tell Nginx to use the configuration next time it is reloaded. We can test our configuration for syntax errors by typing:

**sudo nginx -t**

We will see following message:

**nginx: the configuration file /etc/nginx/nginx.conf syntax is ok**

**nginx: configuration file /etc/nginx/nginx.conf test is successful**

If any errors are reported, go back to your configuration file to review its contents before continuing.

We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

**sudo unlink /etc/nginx/sites-enabled/default**

When we are ready, we reload Nginx to apply the changes: **sudo systemctl reload nginx**

Our new website is now active, but the web root /var/www/projectLEMP is still empty. We will Create an index.html file in that location so that we can test that our new server block works as expected by the running the below:

**sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html**


![image](https://github.com/DevopMudi/Project4/assets/149855241/3e4b1229-0a01-413d-8b6a-367c55985cd2)


Now we go to our browser and try to open our website URL using its IP address: http://13.49.23.137/


![image](https://github.com/DevopMudi/Project4/assets/149855241/761098f9-cbef-4f42-b24d-2d351db51f49)

If you see the text from ‘echo’ command you wrote to index.html file, then it means your Nginx site is working as expected.

Note **You can leave this file in place as a temporary landing page for your application until you set up an index.php file to replace it. Once you do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.**

**Our LEMP stack is now fully configured. In the next step, we’ll create a PHP script to test that Nginx is in fact able to handle .php files within your newly configured website.**

STEP 6 – TESTING PHP WITH NGINX:

Our LEMP stack should now be completely set up. At this point, our LAMP stack is completely installed and fully operational.

We can test it to validate that Nginx can correctly hand .php files off to our PHP processor.

We can do this by creating a test PHP file in our document root. Open a new file called info.php within our document root in our text editor using command:

**sudo nano /var/www/projectLEMP/info.php**

Type or paste the following lines into the new file as in the image below. This is valid PHP code that will return information about your server:

![image](https://github.com/DevopMudi/Project4/assets/149855241/ea13a07b-2a36-4361-9377-2436fbf51ed5)


We can now access this page in our web browser by visiting the domain name or public IP address we’ve set up in our Nginx configuration file, followed by /info.php:

**http://13.49.23.137/info.php**

We will see a web page containing detailed information about our server as image below:

![image](https://github.com/DevopMudi/Project4/assets/149855241/d016e97f-f9d7-4980-8b12-e21538f80f52)

Note: After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:

**sudo rm /var/www/your_domain/info.php**


STEP 7 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)

In this step we will create a test database (DB) with simple “To do list” and configure access to it, so the Nginx website would be able to query data from the DB and display it.
Note : **At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8.**

We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.

We will create a database named example_database and a user named example_user, but you can replace these names with different values.

First, connect to the MySQL console using the root account: **sudo mysql**

To create a new database, run the following command from your MySQL console: **mysql> CREATE DATABASE `example_database;**

Now you can create a new user and grant him full privileges on the database you have just created.

The following command creates a new user named example_user, using mysql_native_password as default authentication method. We’re defining this user’s password as password, but you should replace this value with a secure password of your own choosing.

**CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';**

Now we need to give this user permission over the example_database database: __GRANT ALL ON example_database.* TO 'example_user'@'%';__

This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.
Now exit the MySQL shell with: mysql> exit

You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

**mysql -u example_user -p**

Notice the -p flag in this command, which will prompt you for the password used when creating the example_user user. After logging in to the MySQL console, confirm that you have access to the example_database database: **SHOW DATABASES;**

This will give us the following output:


![image](https://github.com/DevopMudi/Project4/assets/149855241/ed875255-4e51-444c-a59c-9500e0e9a747)

![image](https://github.com/DevopMudi/Project4/assets/149855241/89e473c0-9f0b-4142-a4a7-2741a1fb6d8f)

![image](https://github.com/DevopMudi/Project4/assets/149855241/a74dfb4d-0896-430c-b8c1-48b0095e3090)

![image](https://github.com/DevopMudi/Project4/assets/149855241/880d0034-97d3-486f-a26c-7f977967e44a)

After inserting all comment in the todo list check to confirm from the database that all item are captured as initially inputed

![image](https://github.com/DevopMudi/Project4/assets/149855241/ab38cef2-fd12-4732-8377-8bb922390c0c)

Now lets create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that: **nano /var/www/projectLEMP/todo_list.php**


![image](https://github.com/DevopMudi/Project4/assets/149855241/b88dc229-c0ea-4cc7-b64a-6e2b9286e90d)

Check to see if php script is connecting our database to browser as shown in image below

![image](https://github.com/DevopMudi/Project4/assets/149855241/2339fb96-6169-4a6a-9cea-4bb30086860c)

Check to see if php updating to do list from database as shown below
![image](https://github.com/DevopMudi/Project4/assets/149855241/8d0d0fc9-954c-4b69-bbb1-467a7b66ca0a)

![image](https://github.com/DevopMudi/Project4/assets/149855241/00e1433f-f597-467a-8d32-380d69e2ffa1)


![image](https://github.com/DevopMudi/Project4/assets/149855241/3426e72a-74f4-4d14-b72c-67507e20dbb0)

























































  
