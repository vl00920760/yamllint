// Pipeline for helm chart

pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: helm
            image: jed.ocir.io/axnfm4jb3i73/dx-jenkins-images/helm-oci:latest
            command:
            - cat
            tty: true
          - name: git
            image: jed.ocir.io/axnfm4jb3i73/dx-jenkins-images/git:latest
            #image: jed.ocir.io/axnfm4jb3i73/git1:latest
            command:
            - cat
            tty: true
          imagePullSecrets:
          - name: viveksecret
            '''
    }
}

    stages{
      
      stage('Run gitcheckout') {
      steps {
          script{
             container('git') {
                checkout scm
             }
        }
      }
    }
 
       stage('deploy helm') {
        steps {
            script {
          container('helm') {
              
            withCredentials([usernamePassword(credentialsId: 'github_token', passwordVariable: 'pass', usernameVariable: 'user')]){
                      sh """
                        git config --global url.https://${user}:${pass}@github.com/.insteadOf https://github.com/
                      """
                    }
                    sh '''
                    oci --version
                    echo "testing helm version"
                    #helm version
                    
                    # add sonarqube helm chart repo
                    
                    #helm repo add bitnami https://charts.bitnami.com/bitnami
                    #helm repo update
                    
                    # create a namespace
                   # kubectl create namespace sonarqube-vivek3
                    
                    #install sonarqube
                    #helm install my-sonarqube bitnami/sonarqube -n sonarqube-vivek3
                    
                    #echo " sonarqube installed"
                    
                    '''
            }
            }
         }
     
       }

  }
}
