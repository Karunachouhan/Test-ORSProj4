pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-21.0.12'
        MAVEN = 'C:\\Users\\Hp\\Downloads\\apache-maven-3.9.4-bin\\apache-maven-3.9.4\\bin\\mvn.cmd'
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Karunachouhan/Test-ORSProj4.git',
                    branch: 'main'
            }
        }

        stage('Build New WAR') {
            steps {
                bat 'call "%MAVEN%" clean package'
            }
        }

        stage('Build Web and Pull MySQL') {
            steps {
                bat 'docker-compose build webapp'
                bat 'docker-compose pull db'
            }
        }

        stage('Docker Tagging') {
            steps {
                bat 'docker tag webapp:latest karunachouhan7067/webapp:latest'
                bat 'docker tag mysql:8.0 karunachouhan7067/mysql:8.0'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DH_USER',
                        passwordVariable: 'DH_PASS'
                    )
                ]) {
                    bat 'docker login -u %DH_USER% -p %DH_PASS%'
                }
            }
        }

        stage('Docker Push') {
            steps {
                bat 'docker push karunachouhan7067/webapp:latest'
                bat 'docker push karunachouhan7067/mysql:8.0'
            }
        }
    }
}
