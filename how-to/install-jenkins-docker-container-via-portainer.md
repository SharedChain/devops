# Install Jenkins as a Docker container via Portainer

Portainer provides a simple way to get a Jenkins server up and running.


## Choose Jenkins in Portainer App Template

![Jenkins Portainer options](https://github.com/SharedChain/devops/raw/master/assets/image/jenkins-application-template-configuration.png "Jenkins Portainer options")


## Fetch admin password from container

```
sudo docker exec -i -t jenkins /bin/bash
jenkins@jenkins:/$ cat /var/jenkins_home/secrets/initialAdminPassword
c8f540c008fd46a94738a0af120a1a52
exit
```

The admin password above is `c8f540c008fd46a94738a0af120a1a52`.


## Unlock Jenkins

You should use the admin password from above.

![Initial Jenkins Admin Login](https://github.com/SharedChain/devops/raw/master/assets/image/jenkins-unlock.png "Initial Jenkins admin login")


## Install default plugins

We want our developers to know the *basics* are there as we define exactly what it is we need. This will be refined as we better understand our needs.

![Default Plugin Installation](https://github.com/SharedChain/devops/raw/master/assets/image/jenkins-plugin-installation-options.png "Install default Jenkins plugins")

It chugs along for a while, but does provide a good insight into progress.

![Jenkins install progress updates](https://github.com/SharedChain/devops/raw/master/assets/image/jenkins-install-progress.png "Jenkins install progress")