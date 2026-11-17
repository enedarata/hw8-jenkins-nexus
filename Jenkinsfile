
pipeline {
    agent any

    tools {
        maven '3.9.11'
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/enedarata/hw8-jenkins-nexus.git'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Build and Package') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar',
                    fingerprint: true
            }
        }

        stage('Upload JAR to Nexus') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'nexus-hw8-credentials',
                        usernameVariable: 'NEXUS_USER',
                        passwordVariable: 'NEXUS_PASS'
                    )
                ]) {
                    sh '''
                        curl --fail --show-error \
                          --user "$NEXUS_USER:$NEXUS_PASS" \
                          --upload-file target/spring-boot-complete-1.0.0.jar \
                          http://nexus-hw8:8081/repository/hw8-springboot-releases/com/example/spring-boot-complete/1.0.0/spring-boot-complete-1.0.0.jar
                    '''
                }
            }
        }
    }
}
