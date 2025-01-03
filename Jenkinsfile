pipeline {
    agent any
    
    environment {
        JAVA_HOME = 'C:/Program Files/Java/jdk-17'  // Set your JDK path here
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }
    
    tools {
        maven 'Maven Default'  // Ensure Maven is installed on Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Build the project using Maven
                bat 'mvn clean install'  // Use 'bat' for Windows instead of 'sh'
            }
        }

        stage('SonarAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonar_token')  // Adjust with your SonarQube token credential ID
            }
            steps {
                bat """
                    set PATH=%JAVA_HOME%\\bin;%PATH%
                    mvn clean verify sonar:sonar ^
                    -Dsonar.projectKey=task2 ^
                    -Dsonar.projectName='task2' ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=%SONAR_TOKEN%
                """
            }
        }
    }

    post {
        success {
            echo 'Build and SonarQube analysis completed successfully!'
        }
        failure {
            echo 'Build or SonarQube analysis failed!'
        }
    }
}
