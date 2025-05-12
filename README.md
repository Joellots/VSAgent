|-------------------------------|
|      VSAGENT 504 VERSION      |
|-------------------------------|

INSTALLATION
------------

SERVER CONFIGURATION

    # Install the required packages
        # RedHat / CentOS
	sudo yum install httpd php php-devel php-pdo sqlite3
	# Debian / Ubuntu
	apt install php7.0-fpm php7.0-sqlite sqlite

    # Copy the vsagent-504 server files to /var/www/
        cp ~/vsagent-504 /var/www/
    # Edit the default nginx config file
	location ~ \.php$ {
            include snippets/fastcgi-php.conf
            fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        }

    # Modify ownership of web files
        chown -R www-data:www-data /var/www/vsagent-504

    # Trash the existing database
        rm /var/www/vsagent-504/server/data.db

    # Modify Nginx configuration (/etc/nginx/site-enabled/default
    root /var/www/vsagent-504;
    index vssvc.php;
    server_name _;

    # Restart nginx
        service nginx restart 

ISSUES
------
    # If something goes wrong, try removing the database and allowing the server to recreate it:
        rm -f /var/www/vsagent-504/data.db

USAGE
-----
    Start the agent
        python vsagent-504.py http://127.0.0.1/vssvc.php
    
    Browse to the Command and Control Interface
        firefox http://127.0.0.1/vsgui.php
    
    View the help menu at the bottom right to view special commands
    
    Any input which isn't a special command is passed to cmd.exe
