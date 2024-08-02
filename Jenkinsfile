pipeline{
    agent any
    tools {
        maven 'maven3.9.4'
    }
    stages{
        stage("Cloning Repository"){
            steps{
                echo "Cloning repository"
                git branch: 'main', url: 'https://github.com/mkushimo/docker_local.git'
                echo "Cloning repository completed"
            }
        }
        stage("Building"){
            steps{
                echo "Building package"
                sh "mvn package"
                echo "Building package completed"
            }
        }
        stage("Testing"){
            steps{
                echo "Testing package Source"
                sh "mvn sonar:sonar"
                echo "Testing package Source completed"
            }
        }
        stage("Artifactory"){
            steps{
                echo "Pushing to artifact"
                sh "mvn deploy -s ./settings.xml"
                echo "Pushing to artifact completed"
            }
        }
        stage("Deploy to web"){
            steps{
                echo "Deploy to Tomcat"
                deploy adapters: [tomcat9(credentialsId: 'tomcat-user', path: '', url: 'http://tomcat:8080')], contextPath: null, war: 'target/*.war'
                echo "Deploy to Tomcat completed"
            }
        }
    }
}
