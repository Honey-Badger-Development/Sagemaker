pipeline {
    agent {
        label 'docker && t2.micro'
    }
    environment {
        CI = 'true'
        ecr = "587764000924.dkr.ecr.us-east-1.amazonaws.com/"
    }
    stages {
        stage('Docker Build') {
            when {
                branch 'master'
            }
            steps { 

                    sh "nvidia-docker pull nvidia/cuda"

                    sh "$(aws ecr get-login --no-include-email)"

                    sh "docker build -t interconnector ."
                    sh "docker tag interconnector ${ecr}interconnector:${env.BUILD_ID}"
                    sh "docker tag interconnector ${ecr}interconnector:latest"
 
                    sh "nvidia-docker pull nvidia/cuda"

                    sh "$(aws ecr get-login --no-include-email)"

                    sh "docker build -t inference ."
                    sh "docker tag inference ${ecr}inference:${env.BUILD_ID}"
                    sh "docker tag inference ${ecr}inference:latest"               
                
            }
        }
        stage('Docker Push') {
            when {
                branch 'master'
            }
            steps {
                sh "\$(aws ecr get-login --no-include-email --region us-east-1)"

                sh "docker push ${ecr}interconnector:${env.BUILD_ID}"
                sh "docker push ${ecr}interconnector:latest"
                
                sh "docker push ${ecr}inference:${env.BUILD_ID}"
                sh "docker push ${ecr}inference:latest"

            }
        }
    }
}
