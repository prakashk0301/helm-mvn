pipeline {
    agent any
    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/prakashk0301/helm-mvn'
            }
        }
        stage('Deploy to Dev K8s') {
            steps {
                withAWS(credentials: 'awscicd', region: 'ap-southeast-1') {
                    script {
                        sh 'aws sts get-caller-identity'
                        sh 'rm -f /var/lib/jenkins/.kube/config'
                        sh 'aws eks --region ap-southeast-1 update-kubeconfig --name demo-cluster'
                        sh 'sed -i "s/client.authentication.k8s.io\\/v1alpha1/client.authentication.k8s.io\\/v1beta1/g" /var/lib/jenkins/.kube/config'
                        
                        // Output the kubeconfig for debugging
                        sh 'cat /var/lib/jenkins/.kube/config | grep exec -A 5'
                        
                        // Set the correct context
                        withEnv(["KUBECONFIG=/var/lib/jenkins/.kube/config"]) {
                            sh 'kubectl config use-context arn:aws:eks:ap-southeast-1:058264063726:cluster/demo-cluster'
                            
                            // Get the token and set credentials
                            def token = sh(script: 'aws eks get-token --cluster-name demo-cluster --region ap-southeast-1 --query "status.token" --output text', returnStdout: true).trim()
                            sh "kubectl config set-credentials arn:aws:eks:ap-southeast-1:058264063726:cluster/demo-cluster --token=${token}"
                            
                            // Apply the pod configuration with debugging
                            sh 'kubectl apply -f pod.yaml --validate=false --v=8'
                        }
                    }
                }
            }
        }
    }
}
