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
   */
   
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
   stages {
       stage('Git checkout') {
           steps {
            git 'https://github.com/sanjbh/camel-gke.git'
           }
       }
       stage('test') {
           steps {
              container('jenkins-slave-builder') {
                 sh "mvn --version"
                 sh "docker version"
              }
           }
       }
   }
}
