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
        stage("Perform Test"){
            steps{
                sh "mvn test"
            }
        }   
        stage("Perform Sonar Analysis"){
            steps{
                withSonarQubeEnv("Test_Sonar")
                {
                    sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
                }
            }
        }
        
        stage("Upload to Artifactory"){
            
            steps{
                sh "mvn deploy"
                rtPublishBuildInfo(
                    serverId: '123456789@artifactory',
                        )
            }
        }
    }
}
