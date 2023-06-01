# ansible-project
CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP


## Configuring Ansible For Jenkins Deployment

1. Navigate to Jenkins URL

2. Install & Open Blue Ocean Jenkins Plugin

3. Create a new pipeline

![new pipeline](./images/new%20pipeline.png)

4. Select Github

![selectgithub](./images/select%20github.png)

5. Connect Jenkins with GitHub

![connect jenins with github](./images/jenkins%20with%20git.png)

6. Login to GitHub & Generate an Access Token 

![generate token](./images/generate%20token.png)

7. Paste generated Access Token and connect

![paste-token](./images/paste-token.png)

8. Create a new Pipeline

![create-pipeline](./images/create-pipeline.png)


At this point you may not have a Jenkinsfile in the Ansible repository, so Blue Ocean will attempt to give you some guidance to create one. But we do not need that. We will rather create one ourselves.Click on Administration to exit the Blue Ocean console.

Here is the newly created pipeline. It takes the name of your GitHub repository.

![new pipeline](./images/new-pipeline.png)

- Inside the Ansible project, create a new directory deploy and start a new file Jenkinsfile inside the directory.




```
pipeline {
    agent any

  stages {
    stage("Initial cleanup") {
      steps {
        dir("${WORKSPACE}"){
          deleteDir()
        }
      }
    }

    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }

    stage('Test') {
      steps {
        script {
          sh 'echo "Testing Stage"'
        }
      }
    }

    stage('Package') {
      steps {
        script {
          sh 'echo "Packaging Stage"'
        }
      }
    }

    stage('Deploy') {
      steps {
        script {
          sh 'echo "Deployment Stage"'
        }
      }
    }

    stage('Clean Up') {
      steps {
        script {
          sh 'echo "Cleaning Stage"'
        }
      }
    }

    stage("cleanup") {
      steps {
        cleanWs()
       }
    }
}


```

## RUNNING ANSIBLE PLAYBOOK FROM JENKINS

1. Installing Ansible on Jenkins
2. Install Ansible plugin in Jenkins UI

- Parameterizing Jenkinsfile For Ansible Deployment

To deploy to other environments, we will need to use parameters.

1. Update `sit` inventory with new servers

```
[tooling]
<SIT-Tooling-Web-Server-Private-IP-Address>

[todo]
<SIT-Todo-Web-Server-Private-IP-Address>

[nginx]
<SIT-Nginx-Private-IP-Address>

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
<SIT-DB-Server-Private-IP-Address>
```

2. Update Jenkinsfile to introduce parameterization. Below is just one parameter. It has a default value in case if no value is specified at execution. It also has a description so that everyone is aware of its purpose.

```
pipeline {
    agent any

    parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }
```

3. In the Ansible execution section, remove the hardcoded inventory/dev and replace with `inventory/${inventory}

From now on, each time you hit on execute, it will expect an input.

Notice that the default value loads up, but we can now specify which environment we want to deploy the configuration to. Simply type sit and hit Run.

## CI/CD PIPELINE FOR TODO APPLICATION

We already have tooling website as a part of deployment through Ansible. Here we will introduce another PHP application to add to the list of software products we are managing in our infrastructure. The good thing with this particular application is that it has unit tests, and it is an ideal application to show an end-to-end CI/CD pipeline for a particular application.

Our goal here is to deploy the application onto servers directly from Artifactory rather than from git. Update your Ansible with Artifactory role.


### Prepare Jenkins

1. Fork the repository below into your GitHub account

`https://github.com/darey-devops/php-todo.git`

2. On you Jenkins server, install PHP, its dependencies and Composer tool.

`sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}`

3. Install Jenkins Plugin

- Plot plugin

- Artifactory plugin

4. In Jenkins UI configure Artifactory, configure the server ID, URL and Credentials, run Test Connection.

![Jfrog Conf](./images2/Jfrog%20configurration.png)

![Jfrog page](./images2/JfrogPage.png)






