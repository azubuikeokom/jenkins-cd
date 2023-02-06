# maven configurations in settings.xml and on pom.xml

    <settings>
        <pluginGroups>
            <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
        </pluginGroups>
        <profiles>
            <profile>
                <id>sonar</id>
                <activation>
                    <activeByDefault>true</activeByDefault>
                </activation>
                <properties>
                    <!-- Optional URL to server. Default value is http://localhost:9000 -->
                    <sonar.host.url>
                      http://54.159.55.206:9000
                    </sonar.host.url>
                </properties>
            </profile>
         </profiles>
    </settings> 


 lock down maven sonar plugin under project build 

    <build>
        <pluginManagement>
          <plugins>
            <plugin>
              <groupId>org.sonarsource.scanner.maven</groupId>
              <artifactId>sonar-maven-plugin</artifactId>
              <version>3.9.1.2184</version>
            </plugin>
          </plugins>
        </pluginManagement>
      </build>

 maven sonar command

    mvn sonar:sonar -Dsonar.host.url=http://x.x.x.x:9000 -Dsonar.login=<token generated at the sonarqube server>

 when host url is confiured already in settings.xml

    mvn sonar:sonar -Dsonar.login=<token generated at sonarqube>

 Jenkins pipeline

    pipeline {
        agent any
        tools { 
            maven 'maven3.8.7'
        }
        environment {
            DOCKERHUB_TOKEN=credentials('dockerhub-token')
            IMAGE_NAME='springboot-server'
            DOCKER_USERNAME='zubyking'
        }
        stages{
            stage('Checkout the project'){
                steps{
                    git branch: 'master', credentialsId: 'github-private-key', url: 'git@github.com:azubuikeokom/springboot-maven-micro.git'
                }
            }
            stage('Build package'){
                steps{
                    sh 'mvn clean package'
                }
            }
            stage('Sonar Quality checks'){
                steps{
                        withSonarQubeEnv(installationName:'sonar9.8',credentialsId:'sonarqube-token') {
                          sh 'mvn sonar:sonar'
                    }
                
                }
            }
            stage("Quality Gate") {
                steps {
                    timeout(time: 1, unit: 'HOURS'){
                    waitForQualityGate abortPipeline: true                   
                    }
    
                }
            }
    
            stage('Build and Push image to dockerhub'){
                steps{
                    script{
                            docker.withRegistry('','docker-credentials'){
                            app=docker.build("${DOCKER_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}")
                            app.push()   
                        }
                    }
                }
                        post{
                success{
                    mail bcc: '', body: 'Your pipeline was successful', cc: '', from: 'azubuikeokom@gmail.com', replyTo: '', subject: 'SUCCESSFUL', to: 'azubuikeokom@gmail.com'
                }
                failure{
                     mail bcc: '', body: 'Your pipeline faied', cc: '', from: 'azubuikeokom@gmail.com', replyTo: '', subject: 'FAILURE', to: 'azubuikeokom@gmail.com'
                }
            }
            }
    
        }
    }
    

