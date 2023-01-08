pipeline{
    agent any
    environment{
         OWNER = 'Azubuike'
         ROLE = 'DevOps'
    }
    stages{
        stage("Build"){
            steps{
                sh "ls -ltr"
                sh "echo hello! this will go into my workspace > build-job.txt"
                sh "ls -ltr"
                echo "Building....."
                echo "${OWNER} is  running this pipeline"
                sh 'printenv'
            }
        }
        stage("Test"){
            steps{
                echo "Testing ....."
                sh "docker version"
            }
        }

        stage("Deploy"){
            steps{
                echo "Deploying ...."
            }
        }
    }

}