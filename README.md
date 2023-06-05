**# EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP**


Learn more about the project from this short [VIDEO]()

Here is the repo link to the [todo app](https://github.com/MayorFaj/php-todo/blob/main/Jenkinsfile) we also buillt









#### Dependences to be installed
====================================
- yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
- yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
- yum install python3 python3-pip wget unzip git -y
- python3 -m pip install --upgrade setuptools
- python3 -m pip install --upgrade pip
- python3 -m pip install PyMySQL
- python3 -m pip install mysql-connector-python
- python3 -m pip install psycopg2==2.7.5 --ignore-installed

#### Installing  JAVA
====================================
- sudo yum install java-11-openjdk-devel -y



##### Install  php
=====================================
- yum module reset php -y
- yum module enable php:remi-7.4 -y
- yum install -y php  php-common php-mbstring php-opcache php-intl php-xml php-gd php-curl php-mysqlnd    php-fpm php-json
- systemctl start php-fpm
- systemctl enable php-fpm


#### Ansible dependencies to install
=====================================
* For Mysql Database
- ansible-galaxy collection install community.mysql

* For Postgresql Database
- ansible-galaxy collection install community.postgresql

#### Install composer
=====================================
- curl -sS https://getcomposer.org/installer | php 
- sudo mv composer.phar /usr/bin/composer

##### Verify Composer is installed or not
- composer --version


#### Install phpunit, phploc
=====================================
- sudo dnf --enablerepo=remi install php-phpunit-phploc
- wget -O phpunit https://phar.phpunit.de/phpunit-7.phar
- chmod +x phpunit
- sudo yum  install php-xdebug

#### for database connection
====================================
DB_CONNECTION=mysql
DB_PORT=3306

sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
sudo yum install mysql -y



#### JEnkinsfile for Quick Task
==================================

```
  pipeline {
    agent any

  stages {
    stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
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

    stage('Package'){
      steps {
        script {
          sh 'echo "Packaging App" '
        }
      }
    }

    stage('Deploy'){
      steps {
        script {
          sh 'echo "Deploying to Dev"'
        }
      }

    }
    
    stage("clean Up"){
       steps {
        cleanWs()
     }
    }
     
    }
}
```

