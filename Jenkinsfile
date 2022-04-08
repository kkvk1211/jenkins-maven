pipeline {
    agent any 
    stages {
        stage('Compile and Clean') { 
            steps {

                sh "mvn clean compile"
            }
        }
        stage('Test') { 
            steps {
                sh "mvn test site"
            }
            
             post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'   
                }
            }     
        }

        stage('deploy') { 
            steps {
                sh "mvn package"
            }
        }
   
    stage('Build Docker image'){
            steps {
                sh 'docker build -t pass123a/docker_jenkins_pipeline:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Login'){
            
            steps {
                 withCredentials([string(credentialsId: 'pass123a', variable: 'Pass@123a')]) {
                    sh "docker login -u pass123a -p ${Pass@123a}"
                }
            }                
        }

        stage('Docker Push'){
            steps {
                sh 'docker push pass123a/docker_jenkins_pipeline:${BUILD_NUMBER}'
            }
        }
        
        stage('Docker deploy'){
            steps {
                sh 'docker run -itd -p 3000:3000 pass123a/springboot:0.0.3'
            }
        }

        
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    
    }
   
}
