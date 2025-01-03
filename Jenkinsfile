pipeline {
    agent any
    
    environment {
        // Set JAVA_HOME to use the installed JDK for the build and SonarQube analysis
        //JAVA_HOME = 'C:/Program Files/Java/jdk-17;C:/Program Files/Java/jdk-17/bin'
        JAVA_HOME = 'C:/Program Files/Java/jdk-17'  // Set your JDK path here
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }
    
    tools {
        // Use the Maven installation defined in Jenkins
        maven 'Maven Default'  
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
                // Fetch SonarQube token from Jenkins credentials
                SONAR_TOKEN = credentials('sonar_token')  // Adjust with your SonarQube token credential ID
            }
            steps {
                // For Windows systems, use the 'bat' command
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
