pipeline{
    agent any
    options{
        timeout(time: 1, unit: 'HOURS')
    }
    parameters{
        choice(name: 'CHOICE', choices: ["prod","staging","dev"], description: "work environmnent")
    }
    environment{
         OWNER = 'Azubuike'
         ROLE = 'DevOps'
         DOCKER_CRED = credentials('Docker-Credentials')
    }
 
    stages{
        stage("Build"){
            steps{
                sh "ls -ltr"
                sh "echo hello! this will go into my workspace > build-job.txt"
                sh "ls -ltr"
                echo "Building....."
                echo "${OWNER} is  running this pipeline"
                echo "docker username ${DOCKER_CRED_USR}"
            }
        }
        stage("Test"){
            steps{
                echo "Testing ....."
                echo "choice ${params.CHOICE}"
            }
        }

        stage("Deploy"){
            steps{
                echo "Deploying ...."
            }
        }
    }

}