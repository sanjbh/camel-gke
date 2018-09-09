pipeline {
   agent { dockerfile true }
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
