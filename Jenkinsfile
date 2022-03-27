// pipeline {
//     agent none
//     stages {
//         stage('git pull') {
//             agent any
//             steps {
//                 sh ''
//             }
//         }
//         stage('Front-end') {
//             agent {
//                 docker { image 'node:16.13.1-alpine' }
//             }
//             steps {
//                 sh 'node --version'
//             }
//         }
//     }
// }


pipeline {
    agent any
    environment {
        registry = "acct_id.dkr.ecr.us-east-1.amazonaws.com/harispring"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/Madan421/Demo']]])     
            }
        }
  
    // Building Docker images\
        stage ('build pkg') {
            steps{
                script {
                    sh 'pwd'
                    sh 'mvn clean package'
                }
            }
        }

        stage ('sharing pkg to s3, build an image and push to ecr') {
            steps {
                script {
                    sh 'chmod 777 /var/lib/jenkins/workspace/source1code_master/target/*.jar'
                    sh 'cd ~'
                    sh 'pwd'
                    sh 'echo [hari] >> ~/.aws/credentials'
                    sh 'echo "[profile hari]" >> ~/.aws/credentials'
                    sh 'echo "region = us-east-1" >> ~/.aws/credentials'
                    sh 'echo "aws_access_key_id = AKIA6LM5JB2DCI662XV4" >> ~/.aws/credentials'
                    sh 'echo "aws_secret_access_key = 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" >> ~/.aws/credentials'
                    sh 'aws s3 cp /var/lib/jenkins/workspace/source1code_master/*/*.jar s3://hari220/spring1code --profile "hari"'
                    sh 'docker image ls'
                    sh 'docker image build -i spring1code:hari /var/lib/jenkins/workspace/source1code_master/'
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 986561711750.dkr.ecr.us-east-1.amazonaws.com'
                    //  need to add tag
                    sh 'docker push 986561711750.dkr.ecr.us-east-1.amazonaws.com/jenkins'
                }
            }
        }
    //     stage('Building image') {
    //         steps{
    //             script {
    //                 dockerImage = docker.build registry
    //             }
    //         }
    //     }
   
    // // Uploading Docker images into AWS ECR
    //     stage('Pushing to ECR') {
    //         steps{  
    //             script {
    //                 sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin acct_id.dkr.ecr.us-east-2.amazonaws.com'
    //                 sh 'docker push acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo:latest'
    //             }
    //         }
    //     }
   
    //      // Stopping Docker containers for cleaner Docker run
    //     stage('stop previous containers') {
    //         steps {
    //             sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
    //             sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
    //         }
    //     }
      
    //     stage('Docker Run') {
    //         steps{
    //             script {
    //                 sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo:latest'
    //             }
    //         }
    //     }
    }
}