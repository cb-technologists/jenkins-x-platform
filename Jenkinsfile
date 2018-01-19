pipeline {
    environment {
        GH_CREDS = credentials('jenkins-x-github')
    }
    agent {
        label "jenkins-jx-base"
    }
    stages {
        stage('CI Build') {
            when {
                branch 'PR-*'
            }
            steps {
                container('jx-base') {
                    sg "helm init --client-only"
                    sh "make build"
                    sh "helm template ."
                }
            }
        }
    
        stage('Build and Push Release') {
            when {
                branch 'master'
            }
            steps {
                container('jx-base') {
                    // until kubernetes plugin supports init containers https://github.com/jenkinsci/kubernetes-plugin/pull/229/
                    sh 'cp /root/netrc/.netrc ~/.netrc'

                    sg "helm init --client-only"
                    sh "make release"
                }
            }
        }
    }
}
