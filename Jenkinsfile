pipeline {
   
   environment {
      registry = "asia.gcr.io/white-berm-210209/camel-gke"
      commitId = env.GIT_COMMIT.substring(0,8)      
    }
   
   agent {
      label 'jenkins-slave-builder-label'
   }
   
   stages {
       stage('Git checkout') {
           steps {
            git 'https://github.com/sanjbh/camel-gke.git'
           }
       }
       stage('Build and push docker image to asia.gcr.io') {
           steps {
              withCredentials([file(credentialsId: 'gcr-auth-file', variable: 'GC_KEY')])  {
                 container('jenkins-slave-builder') {
                    sh '''
                     mvn versions:set -DnewVersion=${commitId} -s settings.xml
                     mvn clean package docker:build -s settings.xml
                     cat ${GC_KEY} | docker login -u _json_key --password-stdin https://asia.gcr.io
                     docker push ${registry}:${commitId}
                     docker images
                    '''                    
                 }
              }
           }
       }
   }
}
