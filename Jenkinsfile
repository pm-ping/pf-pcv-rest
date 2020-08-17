pipeline {
    agent { label 'docker' }

    environment {
        JFROG_USER = credentials("cts_jfrog_user")
        JFROG_PASS = credentials("cts_jfrog_password")
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -s .mvn/settings.xml  -Drepo.username=${REPO_CREDS_USR} -Drepo.password=${REPO_CREDS_PSW}  -B -DskipTests compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
//         stage('Automation Test') {
//            steps {
//               timeout(45) {
//                   sh 'Xvfb :80 -screen 0 1280x1024x24 &'
//                   sh 'DISPLAY=:80 mvn -Dsurefire.rerunFailingTestsCount=2 integration-test -P integrationTest'
//               }
//             }
//         }
        stage('Package') {
            steps {
                sh 'mvn -DskipTests package'
            }
        }

//         stage('Push') {
//             steps {
//                 withCredentials([[ $class: 'AmazonWebServicesCredentialsBinding', credentialsId : '0ce8798e-0c75-4357-b687-59c247908b86']]) {
// 	                sh 'aws configure set region us-east-1'
// 	            	sh 'aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID'
// 	            	sh 'aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY'
// 	            	sh '$(aws ecr get-login --no-include-email)'
// 	                sh 'mvn -e clean package dockerfile:push'
//                 }
//             }
//         }

    }
}