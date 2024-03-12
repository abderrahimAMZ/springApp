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
        stage("Run application"){
        steps {
        script {
            sh '''
                # Identify the process
                PID=$(lsof -t -i:8088)

                # If a process is found, stop it
                if [ -n "$PID" ]; then
                    kill $PID
                    echo "Stopped process with PID: $PID"
                else
                    echo "No process is listening on port 8088"
                fi
                touch myapp.log
                nohup java -jar target/chat-1.jar > myapp.log &
                '''

        }
        }
        }
    }
}