pipeline {
    // agent {
    //     docker {image 'maven'}
    // }
    agent { label 'jenkins-agent1' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
	        APP_NAME = "app-project-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "dhamodharreddy500"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'feature', credentialsId: 'github', url: 'https://github.com/DhamodharReddy/app-project.git'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean compile" //sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }

       stage('Check Connectivity to SonarQube') {
            steps {
                script {
                    // SonarQube server URL
                    def sonarqubeUrl = 'http://13.126.201.69:9000'

                    // Use withSonarQubeEnv to set up the SonarQube environment
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        // Perform a basic HTTP request to SonarQube server
                        def response = sh(script: "curl -IsS ${sonarqubeUrl} | head -n 1 | awk '{print \$2}'", returnStatus: true)

                        // Check if the response code is 200 (OK)
                        if (response == 200) {
                            echo "Connectivity test to SonarQube server passed (HTTP ${response})"
                        } else {
                            error "Failed to connect to SonarQube server. HTTP ${response}"
                        }
                    }
                }
            }
        }

    //    stage("SonarQube Analysis"){
    //        steps {
	//            script {
	// 	        withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
    //                     sh "mvn sonar:sonar"
	// 	        }
	//            }	
    //        }
    //    }

    //    stage("Quality Gate"){
    //        steps {
    //            script {
    //                 waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
    //             }	
    //         }

    //     }

    //     stage("Build & Push Docker Image") {
    //         steps {
    //             script {
    //                 docker.withRegistry('',DOCKER_PASS) {
    //                     docker_image = docker.build "${IMAGE_NAME}"
    //                 }

    //                 docker.withRegistry('',DOCKER_PASS) {
    //                     docker_image.push("${IMAGE_TAG}")
    //                     docker_image.push('latest')
    //                 }
    //             }
    //         }

    //    }

    //    stage("Trivy Scan") {
    //        steps {
    //            script {
	//             sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image dhamodharreddy500/app-project-pipeline:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
    //            }
    //        }
    //    }

    //    stage ('Cleanup Artifacts') {
    //        steps {
    //            script {
    //                 sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
    //                 sh "docker rmi ${IMAGE_NAME}:latest"
    //            }
    //       }
    //    }

    //    stage("Trigger CD Pipeline") {
    //         steps {
    //             script {
    //                 sh "curl -v -k --user Admin:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'ec2-13-233-114-241.ap-south-1.compute.amazonaws.com:8080/job/gitops-app-project-cd/buildWithParameters?token=gitops-token'"
    //             }
    //         }
    //    }
    }

//     post {
//        failure {
//              emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
//                       subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
//                       mimeType: 'text/html',to: "dhamodharreddy506@gmail.com"
//       }
//       success {
//             emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
//                      subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
//                      mimeType: 'text/html',to: "dhamodharreddy506@gmail.com"
//       }      
//    }
}