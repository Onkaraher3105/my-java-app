pipeline {
    agent any

    environment {
        MAVEN = 'C:\\Program Files\\Apache\\Maven\\apache-maven-3.9.16\\bin\\mvn.cmd'
    }

    stages {
        stage('1. Checkout Code') {
            steps {
                echo 'Fetching source code from GitHub...'
                checkout scm
            }
        }

        stage('2. Build') {
            steps {
                echo 'Compiling the Java application...'
                bat '"%MAVEN%" clean compile'
            }
        }

        stage('3. Unit Test') {
            steps {
                echo 'Running unit tests...'
                bat '"%MAVEN%" test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('4. Package') {
            steps {
                echo 'Packaging application into JAR...'
                bat '"%MAVEN%" package -DskipTests'
            }
        }

        stage('5. Deploy') {
            steps {
                echo 'Deploying application artifact...'
                bat '''
                    echo Deploying target file...
                    if not exist C:\\tmp\\deployed mkdir C:\\tmp\\deployed
                    copy /Y target\\*.jar C:\\tmp\\deployed\\deployed-app.jar
                    echo Deployment completed successfully.
                '''
            }
        }
    }

    post {
        success { echo 'Pipeline completed successfully!' }
        failure { echo 'Pipeline failed. Check the logs for details.' }
    }
}
