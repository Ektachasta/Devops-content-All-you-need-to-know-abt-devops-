# Docker Task - Configuring Apache with SSL

## Configured Apache on ssl and hosted a simple website

~  by Ekta

install docker if not

```
sudo apt update && sudo apt install -y docker
```

```
sudo systemctl enable --now docker
```

## Check if Docker is installed:

```
docker –version
```

Set Up Website Directory & Permissions

```
sudo mkdir -p /var/www/ektaa.sbs
```

```
sudo chown -R $USER:$USER /var/www/ektaa.sbs
```

```
sudo chmod -R 755 /var/www/ektaa.sbs
```

Create an HTML File for Your Website

```
sudo nano /var/www/ektaa.sbs/index.html
```

paste the content

<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Ektaa SBS</title>

</head>

<body>

<h1>Welcome to Ektaa.sbs</h1>

<p>Your website is live on Apache!</p>

</body>

</html>   (CTRL+X , Y , ENTER)

## Install Certbot & Generate SSL Certificate on Host

```
sudo apt update
```

```
sudo apt install certbot -y
```

generate the SSL certificate for your domain

```
sudo certbot certonly --standalone -d ektaa.sbs -d www.ektaa.sbs
```

enter your email

y

y

-> Modify the Dockerfile to Enable SSL

nano /var/www/ektaa.sbs/Dockerfile

write:

# Use the official Apache image

FROM httpd:latest

# Copy website files to Apache document root

COPY . /usr/local/apache2/htdocs/

# Copy SSL certificates from host to container

COPY ssl/fullchain.pem /usr/local/apache2/conf/fullchain.pem

COPY ssl/privkey.pem /usr/local/apache2/conf/privkey.pem

# Copy custom Apache config for SSL

COPY ssl/httpd-ssl.conf /usr/local/apache2/conf/extra/httpd-ssl.conf

# Expose HTTPS (443) along with HTTP (80)

EXPOSE 80 443

# Start Apache

CMD ["httpd-foreground"]                   (CTRL+X , Y , ENTER)

Copy SSL Certificates Into a Folder

```
sudo mkdir -p /var/www/ektaa.sbs/ssl
```

```
sudo cp /etc/letsencrypt/live/ektaa.sbs/fullchain.pem /var/www/ektaa.sbs/ssl/
```

```
sudo cp /etc/letsencrypt/live/ektaa.sbs/privkey.pem /var/www/ektaa.sbs/ssl/
```

## Configure Apache for SSL

```
sudo nano /var/www/ektaa.sbs/ssl/httpd-ssl.conf
```

add the content:

<VirtualHost *:443>

ServerName ektaa.sbs

ServerAlias www.ektaa.sbs

DocumentRoot "/usr/local/apache2/htdocs"

SSLEngine on

SSLCertificateFile "/usr/local/apache2/conf/fullchain.pem"

SSLCertificateKeyFile "/usr/local/apache2/conf/privkey.pem"

<Directory "/usr/local/apache2/htdocs">

AllowOverride All

Require all granted

</Directory>

ErrorLog "/usr/local/apache2/logs/error_log"

CustomLog "/usr/local/apache2/logs/access_log" combined

</VirtualHost>

now build the image

cd  /var/www/ektaa.sbs

```
docker build -t ektaa-website .
```

## Run the container with SSL enabled

cd

```
docker run -d -p 80:80 -p 443:443 --name ektaa-container ektaa-website
```

verify if the certificates exist or not

ls -l /var/www/ektaa.sbs/ssl/

check if ssl files exist inside the container or not

```
docker exec -it ektaa-container ls -l /usr/local/apache2/conf/
```

check if ssl is enabled

```
docker exec -it ektaa-container httpd -M | grep ssl
```

- expec. output :  ssl_module (shared)

- if you dont see it :

```
docker exec -it ektaa-container sh -c 'echo "LoadModule ssl_module modules/mod_ssl.so" >> /usr/local/apache2/conf/httpd.conf'
```

```
docker exec -it ektaa-container sh -c 'echo "Include conf/extra/httpd-ssl.conf" >> /usr/local/apache2/conf/httpd.conf'
```

```
docker restart ektaa-container
```

check if apache is listening on port 443

```
docker exec -it ektaa-container netstat -tulnp | grep 443
```

If there is no output, Apache is NOT listening on port 443.

if not then, manually start apache inside the container

```
docker exec -it ektaa-container apachectl restart
```

and if a error occurs to set the ServerName globally, then run

```
docker exec -it ektaa-container sh -c 'echo "ServerName ektaa.sbs" >> /usr/local/apache2/conf/httpd.conf'
```

```
docker exec -it ektaa-container apachectl restart
```

run this inside the container

```
docker exec -it ektaa-container cat /usr/local/apache2/conf/httpd.conf | grep Include
```

output should be -> Include conf/extra/httpd-ssl.conf

-if not then add it manually

-> docker exec -it ektaa-container sh -c 'echo "Include conf/extra/httpd-ssl.conf" >>  /usr/local/apache2/conf/httpd.conf'

restart apache inside the container

```
docker restart ektaa-container
```

```
docker exec -it ektaa-container cat /usr/local/apache2/conf/extra/httpd-ssl.conf | grep -E "Listen|VirtualHost"
```

If Listen 443 is missing, add it manually:

```
docker exec -it ektaa-container sh -c "echo 'Listen 443' >> /usr/local/apache2/conf/extra/httpd-ssl.conf"
```

restart the cont. -

```
docker restart ektaa-container
```

check apache config.

```
docker exec -it ektaa-container apachectl configtest
```

- If you see Syntax OK then move further

try accessing your file via (outside the container)

curl -v -k https://ektaa.sbs

OR

try accessing your file via (inside the container)

```
docker exec -it ektaa-container sh
```

apt update && apt install -y curl

apk add curl

curl -I https://localhost --insecure

OR

curl -I https://ektaa.sbs --insecure

- If you see a similar HTTP/1.1 200 OK, your SSL is working!

Add a VirtualHost for Port 80 inside the container(Enable HTTP Redirect to HTTPS)

```
docker exec -it ektaa-container sh -c 'echo "
```

<VirtualHost *:80>

ServerName ektaa.sbs

DocumentRoot \"/usr/local/apache2/htdocs\"

Redirect permanent / https://ektaa.sbs/

</VirtualHost>" >> /usr/local/apache2/conf/extra/httpd-vhosts.conf'

```
docker exec -it ektaa-container apachectl restart
```

```
docker exec -it ektaa-container netstat -tulnp | grep -E "80|443"
```

For exit status 127 error , run

```
docker exec -it ektaa-container sh -c "apt update && apt install -y net-tools"
```

expected o/p :

tcp    0    0 0.0.0.0:80      0.0.0.0:*     LISTEN   1234/httpd

tcp    0    0 0.0.0.0:443     0.0.0.0:*     LISTEN   1234/httpd

```
docker exec -it ektaa-container netstat -tulnp | grep 443
```

```
docker exec -it ektaa-container apachectl -S
```

if it shows : *:443 is a NameVirtualHost

Ensure httpd-ssl.conf is Included in httpd.conf

```
docker exec -it ektaa-container cat /usr/local/apache2/conf/httpd.conf | grep httpd-ssl
```

## Check if Listen 443 is defined multiple times

```
docker exec -it ektaa-container grep -rn "Listen 443" /usr/local/apache2/conf
```

->If Listen 443 appears in both httpd.conf and httpd-ssl.conf, remove it from httpd.conf.

```
docker exec -it ektaa-container sed -i '/Listen 443/d' /usr/local/apache2/conf/httpd.conf
```

```
docker exec -it ektaa-container apachectl restart
```

Enable Port 80 (HTTP)

```
docker exec -it ektaa-container cat /usr/local/apache2/conf/httpd.conf | grep httpd-vhosts.conf
```

- to enable it manually

```
docker exec -it ektaa-container sed -i 's|#Include conf/extra/httpd-vhosts.conf|Include conf/extra/httpd-vhosts.conf|' /usr/local/apache2/conf/httpd.conf
```

```
docker exec -it ektaa-container apachectl restart
```

test inside the container:

```
docker exec -it ektaa-container curl -I
```

for exit status 127 error

```
docker exec -it ektaa-container sh -c "apt update && apt install -y curl"
```

```
docker exec -it ektaa-container curl -I https://localhost --insecure
```

expected o/p:

HTTP/1.1 301 Moved Permanently   (for HTTP)

HTTP/1.1 200 OK                 (for HTTPS)

- if not working : (allow firewall rules)

```
sudo ufw allow 80/tcp
```

```
sudo ufw allow 443/tcp
```

```
sudo ufw reload
```

## Check If Ports 80 & 443 Are Exposed on the Host

```
sudo netstat -tulnp | grep -E "80|443"
```

expected o/p:

tcp    0    0 0.0.0.0:80      0.0.0.0:*     LISTEN   <pid>/docker-proxy

tcp    0    0 0.0.0.0:443     0.0.0.0:*     LISTEN   <pid>/docker-proxy
