//pipeline

library identifier: 'neom-jenkins-sharedlib-dockerbuild@master', retriever: modernSCM([$class: 'GitSCMSource',
   remote: 'https://github.com/NEOM-KSA/dx-neom-jenkins-sharedlib-dockerbuild.git',
   credentialsId: 'github_token'])

library identifier: 'neom-jenkins-sharedlib-changelog@master', retriever: modernSCM([$class: 'GitSCMSource',
   remote: 'https://github.com/NEOM-KSA/dx-neom-jenkins-sharedlib-changelog.git',
   credentialsId: 'github_token'])

library identifier: 'neom-jenkins-sharedlib-infra@main', retriever: modernSCM([$class: 'GitSCMSource',
   remote: 'https://github.com/NEOM-KSA/dx-neom-jenkins-sharedlib-infra.git',
   credentialsId: 'github_token'])

def skipRemainingStages = false
def envTriggered = false

pipeline {
  agent {
    kubernetes {
       yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: gh-cli
            #image: jed.ocir.io/axnfm4jb3i73/mgmt_tools/neom-fss-github-cli-tool:v0.2.0
            image: jed.ocir.io/axnfm4jb3i73/mgmt_tools/neom-fss-github-cli-tool:v0.2.1
            #image: jed.ocir.io/axnfm4jb3i73/github-cli-tool:latest
            command:
            - cat
            tty: true
          - name: python
            image: jed.ocir.io/axnfm4jb3i73/dx-jenkins-images/python:latest
            command:
            - cat
            tty: true
          - name: yamllint
            image: jed.ocir.io/axnfm4jb3i73/dx-coder-images/yamllint:latest
            command:
            - cat
            tty: true
          - name: git
            image: jed.ocir.io/axnfm4jb3i73/git1:latest
            command:
            - cat
            tty: true
          - name: ubuntu
            image: jed.ocir.io/axnfm4jb3i73/ubuntu1:latest
            command:
            - cat
            tty: true
          - name: changelog
            image: jed.ocir.io/axnfm4jb3i73/changlog1:latest
            command:
            - cat
            tty: true
          imagePullSecrets:
          - name: viveksecret
        '''
    }
  }

  environment{
    TENANCY=credentials('tenancy_ocid')
    userpat=credentials('Veerendra_github_token')	
    def projectName = 'neom-fss-infra-oci-mgmt'
    NAMESPACE = "axnfm4jb3i73"
    BUCKET_NAME = "fss-bld-mgmt-obs-mej1-terraform-infra-plans"
    ENVIRONMENT = "bld"
  }
  
  stages {
    stage('Gitcheckout') {
      steps {
        script{
          container('git') {
            checkout scm
           // (version,flag) = semverVersion()
          }
        }
      }
    }

   
 stage("Install Pip3") {
      steps {
        container('python') {
          script{
                    sh """
                  echo "hello1"
                  pip3 --version
                """
          }
        }
      }
    }

     
     stage("yaml lint build") {
      steps {
        script {
          container('yamllint') {
                sh """
                  echo "hello2"
                """
          }
        }
      }
    }
     
     
     
     
     
  }
}


// Testing 1
// Testing 2
// Testing 3
// Testing 4

