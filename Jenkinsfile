pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                script {
                    // Compile the project using Maven
                    sh 'mvn clean compile'
                }
            }
        }

        stage('Unit Test') {
            steps {
                script {
                    // Run unit tests using Maven
                    sh 'mvn test'
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    // Package the application into a WAR file
                    sh 'mvn package'
                }
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the WAR file to Tomcat
                    deploy adapters: [tomcat9(credentialsId: '48558a38-e5fe-495f-b83b-5bccc592e198', path: '', url: 'http://localhost:8082/')],
                            war: '**/*.war'
                }
            }
        }
    }

    post {
        always {
            // Clean up, notify or perform actions regardless of build result
            echo 'Pipeline completed.'
        }
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
