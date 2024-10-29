# Install Apache
    sudo apt update && sudo apt install apache2 -y  # For Ubuntu
    sudo systemctl start apache2  # Ubuntu
    sudo systemctl enable apache2
    sudo systemctl status apache2
# Create a Repository in GitHub
Step 2.1: Create a GitHub Repository
           e.g., apache-deployment.
# Clone the Repository to Your Local Machine
     git config --global user.name "your_username"
     git config --global user.email "your_email@example.com"
     git clone https://github.com/YOUR_GITHUB_USERNAME/apache-deployment.git
     cd apache-deployment
     git checkout -b deployment
# Add a New File Called file1.html
     touch file1.html
      vi file1.html
      <html>
      <head><title>Deployed Page</title></head>
      <body><h1>Hello from Apache on EC2!</h1></body>
      </html>
    Stage the changes
      git add file1.html
    Commit the changes:
      git commit -m "Added file1.html for deployment"
Push Changes to Remote GitHub:
     git push origin deployment
# Set Up a Jenkins Job to Monitor the deployment Branch
 Create a New Jenkins Job
  Open Jenkins and create a new Freestyle Project.
  Name the job, e.g., ApacheDeployment.
# Configure GitHub Repository
Under Source Code Management, choose Git.
   Add the URL of your repository (e.g., https://github.com/YOUR_GITHUB_USERNAME/apache-deployment.git).
  Add your GitHub credentials (username and personal access token)
Configure Build Triggers
# In the job configuration, under Build Triggers, select GitHub hook trigger for GITScm polling.
  This will trigger the Jenkins job whenever a commit is made to the deployment branch.
  Make sure GitHub Webhooks are enabled in the repository (you can configure webhooks in your GitHub repo settings).
# Job to Pull Changes and Copy to EC2 /var/www/html
Step 8.1: Add Build Steps in Jenkins
In the Jenkins job configuration, under Build, add the following steps:
Execute shell script to SSH into the EC2 instance and copy the file1.html to /var/www/html.
# Example script:
  #!/bin/bash
# SSH into EC2 and copy the file
  scp -i /path/to/your-key.pem file1.html ec2-user@YOUR_EC2_PUBLIC_IP:/var/www/html/
# Save and Build the Jenkins Job
 the job, and it will automatically trigger on any commit to the deployment branch.
# Verify the Deployment
http://YOUR_EC2_PUBLIC_IP/file1.html
 **created a full CI/CD pipeline using Git, Jenkins, and an EC2 instance running Apache, where every commit to the deployment branch triggers the job to update the website.**
 path to file.html :/root/apache-deployment/file.html
      <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    # Serve the file.html at the /apache2 path
    Alias /apache2 /var/www/apache2/file.html
    <Directory /var/www/apache2>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
sudo systemctl reload apache2 (reload )
https://www.digitalocean.com/community/tutorials/how-to-use-apache-as-a-reverse-proxy-with-mod_proxy-on-ubuntu-16-04 (reverse proxy)
d /sites-enabled
  112  cd sites-enabled/
  113  ls
  114  vi 000-default.conf
  115  curl http://localhost/apache2
  116  sudo systemctl reload apache2
  117  curl http://localhost/apache2
  118  ls
  119  vi 000-default.conf
  120  cd /var/log/
  121  ls
  122  cd apache2/
  123  s
  124  ls
  125  vi error.log
  126  apachectl -t
  127  systemctl restart apache2
  128  systemctl status apache2
  129  curl http://localhost/apache2
  130  vi error.log
  131  vi access.log
  132  cd /etc/apache2/sites-enabled/
  133  rm -rf ../sites-available/.000-default.conf.swp
  134  vi 000-default.conf
  135  systemctl restart apache2
  136  curl http://localhost/apache2
  137  vi 000-default.conf
  138  systemctl restart apache2
  139  curl http://localhost/apache2
  140  vi 000-default.conf
  141  systemctl restart apache2
  142  curl http://localhost/apache2
  143  vi 000-default.conf
  144  apachectl -t
  145  vi 000-default.conf
  146  apachectl -t
  147  vi 000-default.conf
  148  apachectl -t
  149  cd ..
  150  ls
  151  sudo a2enmod proxy
  152  sudo a2enmod proxy_http
  153  sudo a2enmod proxy_balancer
  154  sudo a2enmod lbmethod_byrequests
  155  apachectl -t
  156  curl http://loclhost/apache2
  157  curl http://localhost/apache2
  158  cd sites-enabled/
  159  vi 000-default.conf
  160  curl http://localhost/apache
  161  vi 000-default.conf
  162  sudo systemctl restart apache2
  163  vi 000-default.conf
  164  apachectl -t
  165  vii 000-default.conf
  166  vi 000-default.conf
  167  apachectl -t
  168  vi 000-default.conf
  169  apachectl -t
  170  cd ..
  171  ls
  172  cd mods-enabled/
  173  ls
  174  ls ../mods-available/
  000-default.conf:
  VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ServerName 127.0.0.1
    ProxyPass /apache2 http://localhost

    # Serve the file.html at the /apache2 path
   # Alias "/apache2" "/var/www/html/"
    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>



    

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    




   
 

            
 