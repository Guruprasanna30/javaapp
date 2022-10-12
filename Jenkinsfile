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
                mvn clean verify sonar:sonar -Dsonar.projectKey=java-app -Dsonar.host.url=http://mydevsecops.eastus.cloudapp.azure.com:9000 -Dsonar.login=sqp_39bd95adb896ae5f06701ff80a5ee024327c8766
            }
        }
    }
}