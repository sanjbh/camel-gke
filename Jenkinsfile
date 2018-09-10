pipeline {
   
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
                    sh "mvn versions:set -DnewVersion=\$(git log -n1 --format=\"%h\") -s settings.xml"
                    sh "mvn clean package docker:build -s settings.xml"
                    //sh "cat \$GC_KEY | docker login -u _json_key --password-stdin https://asia.gcr.io"
                    sh "gcloud auth print-access-token | docker login -u oauth2accesstoken --password-stdin https://asia.gcr.io"
                    sh "mvn docker:push"
                    //sh "mvn --version"
                    sh "docker images"
                 }
              }
           }
       }
   }
}
