pipeline{
    agent any
    environment{
        IMAGE = "akankshrakesh/flask-demo:latest"
    }
    stages{
        stage('checkout'){
            steps{
                checkout scm
            }
        }
        stage('build'){
            steps{
                dir('samples/jenkins'){
                    bat 'docker build -t %IMAGE% .'
                }
            }
        }
        stage('push'){
            steps{
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-hub',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]){
                    bat '''
                        echo %PASS% | docker login -u %USER% --password-stdin
                        docker push %IMAGE%
                    '''
                }
            }
        }
        stage('deploy'){
            steps{
                bat '''
                    docker run -d --name flask-demo -p 3000:3000 %IMAGE%
                '''
            }
        }
    }
}