pipeline {
  agent any
  stages {
    stage('Deploy start') {
      steps {
        slackSend(message: "Deploy ${env.BUILD_NUMBER} Started"
        , color: 'good', tokenCredentialId: 'slack-key')
      }
    }      
    stage('git pull') {
      steps {
        // https://github.com/yoojaemyeong/GitOps.git will replace by sed command before RUN
        git url: 'https://github.com/yoojaemyeong/GitOps.git', branch: 'main'
      }
    }
    stage('k8s deploy'){
      steps {
        kubernetesDeploy(kubeconfigId: 'kubeconfig',
                         configs: '*.yaml')
      }
    }
    stage('send diff') {
      steps {
        slackSend(message: """${env.JOB_NAME} #${env.BUILD_NUMBER} ${env.BUILD_URL} End 
        """, color: 'good', tokenCredentialId: 'slack-key')             
      }
    }
  }
}
