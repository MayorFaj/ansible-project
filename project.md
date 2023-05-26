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