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
                   sh 'kubectl apply -f pod.yaml'
                }
                }
            }
        }
  
 

}
}
