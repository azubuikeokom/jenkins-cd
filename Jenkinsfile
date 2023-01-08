pipeline{
    agent any
    stages{
        stage("Docker"){
           steps{
            dockerNode('docker') {
                docker version
                }
           }     
        }
        stage("Build"){
            steps{
                sh "ls -ltr"
                sh "echo hello! this will go into my workspace > build-job.txt"
                sh "ls -ltr"
                sh "pwd"
                echo "Building....."
            }
        }
        stage("Test"){
            steps{
                echo "Testing ....."
            }
        }

        stage("Deploy"){
            steps{
                echo "Deploying ...."
            }
        }
    }

}