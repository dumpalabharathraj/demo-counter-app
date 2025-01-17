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
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-api'){
                     sh 'mvn clean package sonar:sonar'
                }
                 }
             }           
            }
        stage('Uplaod jar files to Nexus'){
            steps{
                script{
                    def pom = readMavenPom file: 'pom.xml'
                    def nexusRepo = pom.version.endsWith("SNAPSHOT") ? "demoapp-SNAPSHOT" : "demoapp-release"
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', 
                            classifier: '', file: 'target/Uber.jar', 
                            type: 'jar'
                        ]
                    ], 
                    credentialsId: 'nexus-auth', 
                    groupId: 'com.example', 
                    nexusUrl: '54.221.145.201:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepo, 
                    version: "${pom.version}"
                }
            }
        }
        stage('Docker Image Build'){
            steps{
                script{
                    sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID bharathraj111/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID bharathraj111/$JOB_NAME:latest'

                }
            }
        }
    }
}
