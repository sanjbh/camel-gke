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
         name 'jenkins_slave_builder'
         image 'jenkins_slave_builder:latest'
         ttyEnabled true
         command 'cat'
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
              container('jenkins_slave_builder') {
                 sh "mvn --version"
                 sh "docker version"
              }
           }
       }
   }
}
