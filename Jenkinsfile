pipeline{
    agent{
        label "jenkins-agent"
        }

    tools {
        jdk 'java17'
        maven 'maven3'
    }

    stages {

        stage("Cleanup Workspace") {
            steps {
                cleanWs()
                }

        }

        stage("Checkout from SCM"){
            steps {
        script{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/abderrahimAMZ/springApp.git'
                }
        }
        }
        stage("Build application"){
            steps {
                sh "mvn clean package"
                }
        }
        stage("test application"){
            steps {
                sh "mvn test"
                }
        }
        stage("SonarQube analysis"){
            steps {
            script {
                withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                    sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage("Quality Gate"){
            steps {
            script {
                waitForQualityGate abortPipeline: false , credentialsId : 'jenkins-sonarqube-token'
                }
            }
        }

    }

}