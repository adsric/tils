# A collection of miscellaneous commands for Ubuntu

All the following commands will require access to your droplet on DigitalOcean and require the droplet runs on Ubuntu.

```
ssh root@your_server_ip
```

## Configure timezone

```
sudo dpkg-reconfigure tzdata
```

## Permission commands

```
sudo chown -R www-data:www-data /var/www/*
sudo chmod -R 750 /var/www
sudo chgrp -R www-data /var/www
sudo chmod -R g+w /var/www
```

## File permission commands

```
find . -type f | xargs chmod 664
find . -type d | xargs chmod 775
find . -type d | xargs chmod +s
```

## SFTP Account creation

Allow droplet password authentication (password authentication on)

```
vim /etc/ssh/sshd_config
```

* Add new user
* Add user to privileges group www-data
* Add user to specific home directory

```
adduser username
usermod -a -G www-data username
usermod -m -d /var/www/example.com username
```

## Password protect a directory

Step one - Create the Password File Using the OpenSSL Utilities

```
sudo sh -c "echo -n 'username:' >> /etc/nginx/.htpasswd"
sudo sh -c "echo "password" | openssl passwd -apr1 -stdin >> /etc/nginx/.htpasswd"
```

Step two - Configure Nginx Password Authentication

```
vi /etc/nginx/site-available/example.com

location / {
	auth_basic "Restricted Content";
	auth_basic_user_file /var/www/example.com/.htpasswd;
}
```