pipeline
{
agent any
stages
{
  stage('scm checkout')
  { steps {  git branch: 'master', url: 'https://github.com/prakashk0301/maven-project'  } }

  stage('code build')
  { steps {  withMaven(jdk: 'LocalJDK', maven: 'LocalMaven') {
      sh 'mvn clean package'                    // provide maven command

} } }
  
  stage ('docker build and push')
  {steps 
    { withDockerRegistry(credentialsId: 'DockerHub', url: 'https://index.docker.io/v1/') {
   sh 'docker build -t pkw0301/feb-maven-web:latest .'
   sh 'docker images'
   sh 'docker push pkw0301/feb-maven-web:latest'
  }}}

stage('Deploy to dev k8s') {
            steps {
                script {
                    namespace = 'development'

                    echo "Deploying application ${ID} to ${namespace} namespace"
                    createNamespace (namespace)

                    // Remove release if exists
                    helmDelete (namespace, "${ID}")

                    // Deploy with helm
                    echo "Deploying"
                    helmInstall(namespace, "${ID}")
                }
            }
        }
  
 

}
}
