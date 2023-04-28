//pipeline

library identifier: 'neom-jenkins-sharedlib-terraform@feature/oci-tf-shared-lib', retriever: modernSCM([$class: 'GitSCMSource',
   remote: 'https://github.com/NEOM-KSA/dx-neom-jenkins-sharedlib-terraform.git',
   credentialsId: 'github_token'])

library identifier: 'neom-jenkins-sharedlib-dockerbuild@master', retriever: modernSCM([$class: 'GitSCMSource',
   remote: 'https://github.com/NEOM-KSA/dx-neom-jenkins-sharedlib-dockerbuild.git',
   credentialsId: 'github_token'])

library identifier: 'neom-jenkins-sharedlib-changelog@master', retriever: modernSCM([$class: 'GitSCMSource',
   remote: 'https://github.com/NEOM-KSA/dx-neom-jenkins-sharedlib-changelog.git',
   credentialsId: 'github_token'])

library identifier: 'neom-jenkins-sharedlib-trufflehog_nix@master', retriever: modernSCM([$class: 'GitSCMSource',
   remote: 'https://github.com/NEOM-KSA/dx-neom-jenkins-sharedlib-trufflehog_nix.git',
   credentialsId: 'github_token'])

library identifier: 'neom-jenkins-sharedlib-infra@main', retriever: modernSCM([$class: 'GitSCMSource',
   remote: 'https://github.com/NEOM-KSA/dx-neom-jenkins-sharedlib-infra.git',
   credentialsId: 'github_token'])

library identifier: 'neom-jenkins-sharedlib-teams-notification@master', retriever: modernSCM([$class: 'GitSCMSource',
   remote: 'https://github.com/NEOM-KSA/dx-neom-jenkins-sharedlib-teams-notification.git',
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
          - name: trufflehog
            image: jed.ocir.io/axnfm4jb3i73/trufflehog1:latest
            command:
            - cat
            tty: true
          - name: yamllint
            image: jed.ocir.io/axnfm4jb3i73/dx-coder-images/yamllint:latest
            command:
            - cat
            tty: true
          - name: terraform
            image: jed.ocir.io/axnfm4jb3i73/terraform-oci:latest
            command:
            - cat
            tty: true
          - name: terraform-compliance
            image: jed.ocir.io/axnfm4jb3i73/terraform-compliance1:checkov
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

    stage('oci-authentication') {
      steps {
        container('yamllint') {
          script{
            tfci.config()
          }
        }
      }
    }



    stage("Terraform Compliance and checkov") {
      steps {
        script {
          container('terraform-compliance') {
            tfci.config()
            sh '''
              git config --global url.https://kv00797898:$userpat@github.com/.insteadOf https://github.com/
            '''
                sh """
                  cd '$WORKSPACE'
                  terraform version
                """
          }
        }
      }
    }
     
     stage("yaml lint build") {
      steps {
        script {
          container('yamllint') {
            tfci.config()
            sh '''
              git config --global url.https://kv00797898:$userpat@github.com/.insteadOf https://github.com/
            '''
                sh """
                  echo "pip3 --version"
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

