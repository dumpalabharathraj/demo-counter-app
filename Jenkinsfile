pipeline{
    agent any
    stages{
        stage('Git Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/dumpalabharathraj/demo-counter-app.git'    
            }
        }
        stage('Unit Testing'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Integration tesing'){
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('Maven Build'){
            steps{
                sh 'mvn clean install'
            }
        }
 
    }
}
