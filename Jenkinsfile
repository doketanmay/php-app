pipeline{
    
    tools{
        dockerTool 'mydocker'
    }
    agent any
    environment{
        IMAGE_NAME='doketanmay01/myprivaterepo:php$BUILD_NUMBER'
    }
    stages{
        stage("Build the docker image for php"){
            steps{
                script{
                    echo "Building the docker image"
                    sh 'sudo systemctl start docker'
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                        sh "sudo docker build -t ${IMAGE_NAME} ."
                        sh 'sudo sudo docker login -u $USERNAME -p $PASSWORD'
                        sh "sudo docker push ${IMAGE_NAME}"
                }
                }
            }
        }
        stage("Deploy app via dockercompose file"){
            steps{
                script{
                    echo "deploy with dockercompose"
                     sh 'sudo systemctl start docker'
                    sh "bash ./remote-server.sh ${IMAGE_NAME}"
                }
            }
        }
    }
}
