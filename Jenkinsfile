pipeline {
    agent any
    
    tools {
        maven 'local_maven'
    }

    parameters {
        string(name: 'staging_server', defaultValue: '13.232.37.20', description: 'Remote Staging Server')
    }

    stages {
        stage('Build') {
            steps {
                // Use bat instead of sh for Windows
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**\\target\\*.war'
                }
            }
        }

        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        // Use pscp (from PuTTY) on Windows instead of scp
                        bat """
                        pscp -v -batch -unsafe **\\*.war root@${params.staging_server}:/opt/tomcat/webapps/
                        """
                    }
                }
            }
        }
    }
}
