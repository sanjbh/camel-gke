pipeline {

    environment {
        projectName = 'white-berm-210209'
        applicationName = 'camel-gke'
        registry = "asia.gcr.io/${projectName}/${applicationName}"
        commitId = env.GIT_COMMIT.substring(0,7)
    }

    agent {
        label 'jenkins-slave-builder-label'
    }

    stages {       
        stage('Build and push docker image to asia.gcr.io/white-berm-210209') {
            
            steps {
                withCredentials([file(credentialsId: 'gcr-auth-file', variable: 'GC_KEY'), 
                                 file(credentialsId: 'maven-settings-xml-secret', variable: 'MVN_SETTINGS_XML')])  {
                    container('jenkins-slave-builder') {
                        sh '''
                            mvn versions:set -DnewVersion=${commitId} -s ${MVN_SETTINGS_XML}
                            mvn clean package docker:build -s ${MVN_SETTINGS_XML}
                            cat ${GC_KEY} | docker login -u _json_key --password-stdin https://asia.gcr.io
                            docker push ${registry}:${commitId}
                            mvn scm:tag -s ${MVN_SETTINGS_XML}
                        '''    
                    }
                }
            }            
        }
    }
}
