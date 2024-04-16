pipeline {
  agent any

  options {
    ansicolor('Xterm')
  }

  parameters {
    string(name: 'ENV', defaultValue: 'prod', description: 'Environment')
    string(name: 'APPNAME', defaultValue: '', description: 'App Name')
    string(name: 'APP_VERSION', defaultValue: '', description: 'App Version')
  }

  stages {
    stage('Get Kubeconfig') {
      steps {
        sh 'aws eks update-kubeconfig --name ${ENV}-roboshop'
      }
    }
    stage('Get App Code') {
      steps {
        dir('App') {
          git branch: 'main', url: 'https://github.com/umamanasa/${APPNAME}'
        }
        dir('CHART') {
          git branch: 'main', url: 'https://github.com/umamanasa/roboshop-helm1'
        }
      }
    }

    stage('Helm Deploy') {
      steps {
        sh 'helm upgrade -i ${APPNAME} ./CHART --debug -f APP/helm/${ENV}.yaml --set APP_VERSION=${APP_VERSION}'
      }
    }

  }

  post {
    always {
      cleanWs()
    }
  }
}
