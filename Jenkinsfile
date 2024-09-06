pipeline {
    agent any

    environment {
        MAVEN_OPTS = '-Xmx1024m'
    }

    tools {
        maven 'Maven_3.9.5'
        jdk 'JDK_21'
    }

    stages {
        stage('Checkout Source') {
            steps {
                // Checkout the source code from the branch that triggered the build
                git url: 'https://github.com/BD-HUGO/hugo-demo-webgoat.git', branch: "${env.BRANCH_NAME}"
            }
        }

        stage('Cov-Build') {
            steps {
                sh 'synopsys_scan coverity_args: '--enable-audit-mode --webapp-security --aggressiveness-level high -j auto', coverity_build_command: 'cov-build --dir idir mvn clean compile', coverity_config_path: 'C:/Program Files/Coverity/Coverity Static Analysis/config/template-java-config-0/coverity_config.xml', coverity_local: true, coverity_prComment_enabled: true, coverity_project_name: 'e jenkins-ci-demo-java', coverity_stream_name: 'e jenkins-ci-demo-java-1', mark_build_status: 'UNSTABLE', product: 'coverity''
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
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
