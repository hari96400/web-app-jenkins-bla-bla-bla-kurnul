 serverId: 'arty'
   pipeline {
    agent slave1
     tools {
        maven 'maven'
        jdk 'jdk'
    }
    stages{
        stage ('build') {
            steps {
        echo "code is building"
         sh 'mvn clean'
         sh 'mvn install'
            }
        }

        stage ('Bulding docker docker image') {
            steps {
                echo "build docker image"
                sh 'docker build -t ripy .'
            }
        }
        stage ('Uploading to Ecr') {
            steps {
                echo "uploading to ECR "
                sh '$(aws ecr get-login --no-include-email --region ap-south-1)'
                sh 'docker tag ripy:latest 220454890480.dkr.ecr.ap-south-1.amazonaws.com/firstrepo:latest'
                sh 'docker push 220454890480.dkr.ecr.ap-south-1.amazonaws.com/firstrepo:latest'
        }
        }
        
        
        stage ('Deploying to eks') {
            steps {
                 echo "Deploying imgae to EKS"
                 sh 'rm -rf /var/lib/jenkins/.kube/ && aws eks update-kubeconfig --name MYCLUSTER '
                 sh 'kubectl apply -f deploy.yml'
                 sh 'kubectl apply -f service.yml'
            }
        }
        }
}
}
