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
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=devsecops -Dsonar.host.url=http://mydemocloud.eastus.cloudapp.azure.com:9000 -Dsonar.login=sqp_41ef952aa545465d84e7e4754ec06d4f4c5f9689'
                // }
                // timeout (time: 3, unit: 'MINUTES') {
                //     script{
                //         waitForQualityGate abortPipeline: false
                //     }
                // }
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