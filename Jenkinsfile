pipeline{
    agent any
    stages{
        stage("First - Code Checkout"){
            steps{
                checkout scm
            }
        }
        stage("Second - Build Code"){
            steps{
                sh "mvn install"
            }
        }
        stage("Third - Perform Unit Test"){
            steps{
                sh "mvn test"
            }
        }   
        stage("Fourth - Perform Sonar Analysis"){
            steps{
                withSonarQubeEnv("Test_Sonar")
                {
                    sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
                }
            }
        }
        
        stage("Fifth - Upload to Artifactory"){
            
            steps{
                sh "mvn deploy"
                rtPublishBuildInfo(
                    serverId: '123456789@artifactory',
                        )
            }
        }
        
        stage('Docker - Build Image'){
            steps{
                sh "docker build -t devopsassignmentimage:${BUILD_NUMBER} ."
            }
        }
        
        stage('Docker - Deployment'){
            steps{
                sh "docker run --name devopsassignmentcontainer -d -p 9050:8080 devopsassignmentimage:${BUILD_NUMBER}"
            }
        }
    }
}
