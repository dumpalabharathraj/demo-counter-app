pipeline{
    agent any
    stages{
        stage('Git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/dumpalabharathraj/demo-counter-app.git'
            }
        }
        stage('Unit Testing'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Integration Testing'){
            steps{
                sh 'mvn verify -DskiUnitTests'
            }
        }
        stage('Maven Building'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('SonarQube analysis'){
            steps{
                scripts{
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                     sh 'mvn clean package sonar:sonar'

                 }
                }
           
            }
        }
    }
}
