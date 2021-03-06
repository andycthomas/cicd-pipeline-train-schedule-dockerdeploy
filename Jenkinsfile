pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }

        stage('Build Docker Image'){
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("andycthomas/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        } 

        stage ('Push Docker Image'){
            when {
                branch 'master'
            }

            steps {
                sript {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_loging'){
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }      
        
    }
}
