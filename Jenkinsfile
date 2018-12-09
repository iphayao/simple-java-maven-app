pipeline {
    // agent {
    //     docker {
    //         image 'maven:3-alpine'
    //         args '-v /root/.m2:/root/.m2/'
    //     }
    // }
    agent any
    stages {
        stage('Build') {
            agent { docker 'maven:3-alpine' }
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            agent { docker 'maven:3-alpine' }
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
            agent any
            steps {
                withDockerRegistry(credentialsId: 'd9a99034-e268-42bf-97dd-55d64938bcc6', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t my-app .'
                }
            }
        }
        // stage('Deliver') {
        //     steps {
        //         sh './deliver.sh'
        //     }
        // }
    }
}
