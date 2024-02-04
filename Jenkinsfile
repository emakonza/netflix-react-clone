pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("684985117207.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latestv1")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 684985117207.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latestv1'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://684985117207.dkr.ecr.us-east-1.amazonaws.com/netflix-jan', 'ecr:us-east-1:evidence-ecr') {
                    // build image
                    def myImage = docker.build("684985117207.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latestv1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
