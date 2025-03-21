# Installing Jenkins in Linux and creating a freestyle Nginx project


## Installing JDK 17
```bash
sudo apt install -y openjdk-17-jdk
sudo update-alternatives --config java
```



 - After this you'll be prompted to select a java version select the index with java-17


## Installing Jenkins
```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```


## Starting Jenkins
```bash
sudo systemctl status jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```
 - Copy the password provided in this step


## Jenkins Initial Setup
 - Open a browser and go to `localhost:8080` and paste in the password


 - Click on `continue`


 - Click on `Install Suggested Plugins`


 - The plugins will eb installed one by one and you'll be redirected to the next page click `Start Jenkins`


 - Fill in your details and create user id and password then click `save and continue`
> [!NOTE]  
> Remember this user id and password. This will overwrite the `Initial Password` generated, and will be required to login everytime you restart Jenkins.


 - Leave the default Jenkins URL then click `Save and Finish`



## Setting up a Nginx freestyle project in Jenkins
 - In Jenkins home page click on `create a job`


 - Enter a project name
 - Select `Freestyle Project`
 - Click `Ok`


 - In the next page scroll down to `Build Steps`
 - Click `Add Build Step`
 - Select `Execute Shell`


 - Paste in the below code
```bash
#!/bin/bash
# Update package lists
sudo apt update -y

# Install Nginx
sudo apt install -y nginx

# Start and enable Nginx
sudo systemctl enable nginx.service
sudo systemctl start nginx

# Verify Nginx is running
systemctl status nginx
```


 - Click `Build Now` in the left side panel

 - Your first Nginx project using Jenkins is successfully deployed!
 - Click on the Build in `Builds` and click on the `Console Output` to view the logs
