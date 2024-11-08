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
                        
                        // Remove existing kubeconfig and update
                        sh 'rm -f /var/lib/jenkins/.kube/config'
                        sh 'aws eks --region ap-southeast-1 update-kubeconfig --name demo-cluster'
                        
                        // Update the kubeconfig to replace v1alpha1 with v1beta1
                        sh 'sed -i "s/client.authentication.k8s.io\\/v1alpha1/client.authentication.k8s.io\\/v1beta1/g" /var/lib/jenkins/.kube/config'
                        
                        // Set the correct context
                        sh 'kubectl config use-context arn:aws:eks:ap-southeast-1:058264063726:cluster/demo-cluster'
                        
                        // Apply the pod configuration with debugging
                        sh 'kubectl apply -f pod.yaml --validate=false --v=8'
                    }
                }
            }
        }
    }
}
