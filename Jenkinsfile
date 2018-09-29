pipeline {

    environment {
        region = 'asia.gcr.io'
        projectName = 'strong-eon-217812'
        applicationName = 'camel-gke'
        registry = "${region}/${projectName}/${applicationName}"
        commitId = env.GIT_COMMIT.substring(0,7)
        stageString = "Build and push ${applicationName} docker image to ${region}/${projectName}"
    }

    agent {
        label 'jenkins-k8s-slave-label'
    }

    stages {       
        stage(stageString) {
            
            steps {
                withCredentials([file(credentialsId: 'gcr-secrets-file', variable: 'GC_KEY'), 
                                 file(credentialsId: 'maven-settings-xml-secret', variable: 'MVN_SETTINGS_XML')])  {
                    container('custom-jenkins-slave') {
                        sh '''
                            mvn versions:set -DnewVersion=${commitId} -s ${MVN_SETTINGS_XML}
                            mvn clean package docker:build -s ${MVN_SETTINGS_XML}
                            cat ${GC_KEY} | docker login -u _json_key --password-stdin https://${zone}
                            docker push ${registry}:${commitId}
                            mvn scm:tag -s ${MVN_SETTINGS_XML}
                        '''    
                    }
                }
            }            
        }
    }
}
