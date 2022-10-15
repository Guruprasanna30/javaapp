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
                    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=java-app -Dsonar.host.url=http://mydevsecops.eastus.cloudapp.azure.com:9000 -Dsonar.login=sqp_89301dacb5af2bf68771261c32f95b5347a6df76'
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