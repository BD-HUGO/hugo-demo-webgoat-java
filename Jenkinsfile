pipeline {
    agent any

    environment {
        MAVEN_OPTS = '-Xmx1024m' // Adjust memory settings as needed
    }

    tools {
        maven 'Maven_3.6.3' // Ensure Maven is configured in Jenkins (change version if needed)
        jdk 'JDK_11'        // Specify the required JDK version (change as per your project)
    }

    stages {
        stage('Checkout Source') {
            steps {
                // Checkout the source code from the branch that triggered the build
                git url: 'https://github.com/BD-HUGO/hugo-demo-webgoat.git', branch: "${env.BRANCH_NAME}"
            }
        }

        stage('Build') {
            steps {
                // Clean and build the Maven project
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                // Run unit tests with Maven
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                // Package the project into a .jar or .war file
                sh 'mvn package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archive the .jar/.war build artifacts for later retrieval
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            // Clean workspace after build to ensure no leftover files
            cleanWs()
        }
        success {
            echo "Build succeeded on branch ${env.BRANCH_NAME}"
        }
        failure {
            echo "Build failed on branch ${env.BRANCH_NAME}"
        }
    }
}