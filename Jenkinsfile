pipeline{
    agent any;
     tools{
       maven 'maven'
       jdk 'JDK11'
   }
    stages{
        stage('Fetch project from github'){
            steps{
                git branch: 'master', url: 'https://github.com/keerthi-battula/todo-app.git'
            }
                
        }
        stage('Maven package'){
            steps{
                bat 'mvn -f app/pom.xml package'

            }
        }
        stage('sonar analysis'){
            steps{
                withSonarQubeEnv('sonarQube'){
                    bat 'mvn -f app/pom.xml sonar:sonar'
                }
            }
        }
        stage('deploy to artifactor'){
            steps{
                rtUpload (
            serverId: 'ARTIFACTORY-SERVER',
            spec: '''{
                 "files": [
                             {
                                "pattern": "app/target/*.war",
                                "target": "art-doc-dev-loc/todo-app/"
                            }
                        ]
            }''',
            )

            }
        }
        //  stage('Location'){
        //     steps{
        //         //sh 'cd artifacts'
        //         //sh 'ls ~/var/lib/jenkins/workspace/test/todo-app/todo-app/artifacts'
        //     }
        // }
        stage('download artifact'){
            steps{
                 rtDownload (
                 serverId: "ARTIFACTORY-SERVER",
                spec:"""{
                     "files": [
                                {
                                    "pattern": "art-doc-dev-loc/todo-app/**",
                                    "target": "app/artifacts/"      
                                }
                            ]
              }"""
            )
            
            }
        }

        
       /* stage('Docker build'){
            steps{
                // dir("var/lib/jenkins/workspace/test/todo-app/todo-app/artifacts"){
                    sh 'docker-compose build'
                // }
                
            }
        }
        stage('Pushing images to docker hub'){
            steps{
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPWD')]) {
                            // some block
                   sh "docker login -u sujjad -p ${dockerHubPWD}"

                }
                sh "docker commit todo-mysql sujjad/todo-mysql:v${env.BUILD_ID}"
                sh "docker push sujjad/todo-mysql:v${env.BUILD_ID}"
                sh "docker commit cisample_app_1 sujjad/todo-app:v${env.BUILD_ID}"
                sh "docker push sujjad/todo-app:v${env.BUILD_ID}"


            }
        }*/

    }
}
