# LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS
##### Task
This project consists of two parts:

- Configure Nginx as a Load Balancer
- Register a new domain name and configure secured connection using SSL/TLS certificates
  #### CONFIGURE NGINX AS A LOAD BALANCER - create an ubuntu server and open port 443 and 80 and tag it load balancer
 - update the instance and install Nginx
 - sudo vi /etc/nginx/nginx.conf to configure the nginx.config file see below to see code -

  #### REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES
  In order to get a valid SSL certificate – you need to register a new domain name, you can do it using any Domain name registrar – a company that manages reservation of domain names. The most popular ones are: Godaddy.com, Domain.com, Bluehost.com.

Register a new domain name with any registrar of your choice in any domain zone (e.g. .com, .net, .org, .edu, .info, .xyz or any other)
Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP
You might have noticed, that every time you restart or stop/start your EC2 instance – you get a new public IP address. When you want to associate your domain name – it is better to have a static IP address that does not change after reboot. Elastic IP is the solution for this problem.
- Configure Nginx to recognize your new domain name.
- Install certbot and request for an SSL/TLS certificate using [sudo systemctl status snapd && sudo snap install --classic certbot]
- Request your certificate (just follow the certbot instructions – you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file so make sure you have updated it on step 4).
- sudo ln -s /snap/bin/certbot /usr/bin/certbot
- sudo certbot --nginx
Test secured access to your Web Solution by trying to reach https://<your-domain-name.com>
By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.
You can test renewal command in dry-run mode
- sudo certbot renew --dry-run
Best practice is to have a scheduled job that to run renew command periodically. Let us configure a cronjob to run the command twice a day.
To do so, lets edit the crontab file with the following command:
- crontab -e
Add following line:
- * */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1
You can always change the interval of this cronjob if twice a day is too often by adjusting schedule expression.




