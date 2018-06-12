pipeline {
  agent {
    node {
      label 'dev'
    }

  }
  stages {
    stage('git') {
      steps {
        dir(path: '/home/shiyanlou/test_git') {
          git(url: 'https://github.com/wjstud/test_git', branch: 'master', changelog: true, credentialsId: 'aa4e100ec15da949c3b3cbdc336846d504ae012b', poll: true)
        }

      }
    }
    stage('build') {
      steps {
        dir(path: '/home/shiyanlou/test_git') {
          sh 'sudo pip install -r requirements.txt'
        }

      }
    }
    stage('run') {
      steps {
        dir(path: '/home/shiyanlou/test_git') {
          sh 'python app.py'
        }

      }
    }
  }
}