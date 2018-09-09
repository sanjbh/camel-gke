pipeline {
   /*
   agent { 
      dockerfile  { dir 'builder-image' } 
   }
   
   agent {
      docker {
         image 'herman1975/kubebuilder:v3'         
      }
   }
   
   
   agent {
      kubernetes {
         label 'declarative-pod'
         containerTemplate {
            name 'jenkins-slave-builder'
            image 'jenkins_slave_builder:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
            ttyEnabled true
            command 'cat'
         }
      }      
   }
   */
   
   agent {
      label 'jenkins-slave-builder-label'
   }
   stages {
       stage('Git checkout') {
           steps {
            git 'https://github.com/sanjbh/camel-gke.git'
           }
       }
       stage('Build docker image') {
           steps {
              container('jenkins-slave-builder') {
                 sh "mvn versions:set -DnewVersion=\$(git log -n1 --format=\"%h\")"
                 sh "mvn clean package docker:build -s settings.xml"
                 sh "mvn docker:push"
                 //sh "mvn --version"
                 sh "docker images"
              }
           }
       }
   }
}
