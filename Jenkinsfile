pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Build') {
            steps {
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" -c "mvn clean package"'
            }
            post {
                success {
                    echo "✅ Build successful. Archiving the artifacts..."
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy to Tomcat 9') {
            steps {
                echo "🚀 Deploying WAR file to Tomcat server..."
                deploy adapters: [
                    tomcat9(
                        alternativeDeploymentContext: '',
                        credentialsId: 'tomcat-creds',
                        path: '',
                        url: 'http://localhost:8080/manager/text'
                    )
                ],
                contextPath: null,
                war: '**/*.war'
            }
        }
    }
}
