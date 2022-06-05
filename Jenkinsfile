pipeline {
    agent any

    stages {
        stage('build docker') {
            steps {
                    sh 'docker build -t thakorn09/todoapp .'
                }
            }

        stage('push docker') {
            steps {
                    sh 'docker push  thakorn09/todoapp'
                }
            }
        
        stage('Prepare Hello script') {
            steps {
                sh 'echo \'docker run --name hello hello-world\' > hello.sh'
            }            
        }
        stage('Run Hello World Docker') {
            
            steps {
                sshagent(['prod-credential']) {
                    // some block
                    sh 'scp -o StrictHostKeyChecking=no hello.sh ubuntu@ip-172-31-22-175:/home/ubuntu/hello.sh'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@ip-172-31-22-175 bash /home/ubuntu/hello.sh'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@ip-172-31-22-175 docker rm hello'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@ip-172-31-22-175 docker image rm hello-world'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@ip-172-31-22-175 rm /home/ubuntu/hello.sh'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@ip-172-31-22-175 docker pull thakorn09/todoapp'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@ip-172-31-22-175 docker run -d -p 1000:3000 thakorn09/todoapp'
                    }
                    
                }  
            }
    }

    }

