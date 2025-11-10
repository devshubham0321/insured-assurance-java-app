pipeline {
    agent any

    environment {
        TOMCAT_USER = 'admin'
        TOMCAT_PASSWORD = 'admin123'
        TOMCAT_URL = 'http://43.205.140.72:8080/manager/text'
        WAR_NAME = 'insured-app.war' // final WAR name
    }

    stages {

        stage('Checkout SCM') {
            steps {
                echo 'Checking out Git repository...'
                git branch: 'main', url: 'https://github.com/devshubham0321/insured-assurance-java-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Java Web App using Maven...'
                // Force plugin update with -U
                sh 'mvn clean package -U'
            }
        }

        stage('Rename WAR') {
            steps {
                echo 'Renaming WAR to fixed name for deployment...'
                sh '''
                mv target/insured-app-1.0-SNAPSHOT.war target/${WAR_NAME}
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying WAR to Tomcat...'
                sh """
                curl --upload-file target/${WAR_NAME} \
                     --user ${TOMCAT_USER}:${TOMCAT_PASSWORD} \
                     ${TOMCAT_URL}/deploy?path=/insured-app&update=true
                """
            }
        }

        stage('Validate Deployment') {
            steps {
                echo 'Validating deployment...'
                sh """
                curl -I http://43.205.140.72:8080/insured-app/
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! App deployed.'
        }
        failure {
            echo 'Pipeline failed. Check the console output for errors.'
        }
    }
}
