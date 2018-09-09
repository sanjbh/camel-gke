pipeline {
   /*
   agent { 
      dockerfile  { dir 'builder-image' } 
   }
   */
   agent {
      docker {
         image 'herman1975/kubebuilder:v3'
         label 'my-jenkins-slave-builder'
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
              sh "mvn version"
              sh "docker version"
           }
       }
   }
}
