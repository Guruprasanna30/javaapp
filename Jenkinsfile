pipeline {
    agent any
    stages {
        stage('Maven Build'){
            steps{
                sh 'mvn clean package -DskipTests=true'
            }
        }
        stage('Sonarqube SAST analysis') {
            steps {
                // withSonarQubeEnv('SonarQube') {
                    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=java-app -Dsonar.host.url=http://mydemocloud.eastus.cloudapp.azure.com:9000 -Dsonar.login=sqp_f88f69e04b84156aeaaa3a9dda0736bee10ee112'
                // }
                timeout (time: 3, unit: 'MINUTES') {
                    script{
                        waitForQualityGate abortPipeline: false
                    }
                }
            }
        }
        stage('Vulnerability Scan - Pom') {
            steps {
                sh "mvn dependency-check:check"
            }
            post {
                always {
                    dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
                }
            }
        }
    }
}