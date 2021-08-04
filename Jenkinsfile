pipeline {
  agent any
  environment {
    COURSE = 'Calgary DevOps'
    BRANCH = 'main'
    WWWROOT = '/var/www'
    SSHUSER = 'jenkins'
  }
  stages {
    stage('Audit Tools') {
      steps {
        echo "Audit all tools to be use on this pipeline ${BRANCH}"
        sh "git --version"
        sh "node --version"
        sh "npm --version"
        sh "ng --version"
        sh "ansible --version"
        echo "Workspace Folder: ${WORKSPACE}"
      }
    } 
    stage('Install packages') {
      steps {
        dir("${WORKSPACE}/conduit-ui") {
          echo "Install conduit UI packages"
          sh "npm install"
        }
      }
    }
    stage('Run linting') {
      steps {
        dir("${WORKSPACE}/conduit-ui") {
          echo "npm run lint"
        }
      }
    }
    stage('Build UI') {
      steps {
        dir("${WORKSPACE}/conduit-ui") {
          sh "npm run build"
        }
      }
    }
    stage('Deploy app to WEB01') {
      steps {
        sh "ssh web01 rm -rf /home/${SSHUSER}/conduit"
        sh "scp -r ${WORKSPACE}/conduit-ui/dist web01:/home/${SSHUSER}/conduit"
        sh "ssh ubuntu@web01 sudo -i rm -rf ${WWWROOT}/conduit"
        sh "ssh ubuntu@web01 sudo -i cp -r /home/${SSHUSER}/conduit ${WWWROOT}/conduit"
      }
    }
  }
}
