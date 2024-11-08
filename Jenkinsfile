pipeline
{
agent any
stages
{
  stage('scm checkout')
  { steps {  git branch: 'master', url: 'https://github.com/prakashk0301/maven-project'  } }


stage('Deploy to dev k8s') {
            steps {
                script {
                   sh 'cd mvn-web'
                   sh 'helm install mvn-web .'
                }
            }
        }
  
 

}
}
