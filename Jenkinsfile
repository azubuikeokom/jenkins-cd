pipeline{
    agent any
    stages{
        stage("Build"){
            steps{
                sh "ls -ltr"
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