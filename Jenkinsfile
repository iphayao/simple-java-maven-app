pipeline {
    // agent {
    //     docker {
    //         image 'maven:3-alpine'
    //         args '-v /root/.m2:/root/.m2/'
    //     }
    // }
    agent any
    environment {
        dockerImage = ''
    }
    stages {
        stage('Build') {
            // agent { 
            //     docker {
            //         image 'maven:3-alpine' 
            //         args '-v /root/.m2:/root/.m2/'
            //     }
            // }
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            // agent { 
            //     docker {
            //         image 'maven:3-alpine' 
            //         args '-v /root/.m2:/root/.m2/'
            //     }
            // }
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build("phayao/my-app")
                }
            }
        }
        stage('Push image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'd9a99034-e268-42bf-97dd-55d64938bcc6', url: 'https://index.docker.io/v1/') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deployment') {
            steps {
                sh 'kubectl apply -f myapp-deployment.yml';
            }
        }
    }
}
