pipeline
{
agent any
stages
{
  stage('scm checkout')
  { steps {  git branch: 'master', url: 'https://github.com/prakashk0301/helm-mvn'  } }


stage('Deploy to dev k8s') {
            steps { withAWS(credentials: 'awscicd', region: 'ap-southeast-1') {
    // some block

                script {
                   sh 'aws sts get-caller-identity'
                   sh 'aws eks --region ap-southeast-1 update-kubeconfig --name demo-cluster'
                   sh 'sed -i "s/client.authentication.k8s.io\\/v1alpha1/client.authentication.k8s.io\\/v1beta1/g" ~/.kube/config'
                   sh 'kubectl apply -f pod.yaml'
                }
                }
            }
        }
  
 

}
}
