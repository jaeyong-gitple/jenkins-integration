#!groovy

pipeline {
  agent any
  environment {
    REMOTE_HOST = 'ci-staging.mspdev.link'
    REMOTE_USER = 'ubuntu'
    REMOTE_PATH = 
  }
  stages {
    stage('Prepare') {
      steps {
        script {
          buildConfig = [:]

          buildConfig.app = [path: 'server/app', isChanged: false , build: 'make app', deploy: 'deploy app']
          buildConfig.hello = [path: 'server/hello', isChanged: false , build: 'make hello', deploy: 'deploy hello']

          findTargetPath(buildConfig);

          withCredentials([sshUserPrivateKey(credentialsId: 'ci-ssh', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
            def remote = [:]
            remote.name = env.REMOTE_HOST
            remote.host = env.REMOTE_HOST
            remote.allowAnyHosts = true
            remote.user = userName
            remote.identityFile = identity 
            sshCommand remote: remote, command: 'cd $TP_TARGET_SOURCE;git '
          }
        }

        echo "${buildConfig}"
      }
    }

    stage('Build') {
      stages {
        stage('Build: app') {
          when {
            expression {
              return buildConfig['app'].isChanged
            }
          }
          steps {
            echo "${buildConfig['app']}"
          }
        }
        stage('Build: hello') {
          when {
            expression {
              return buildConfig['hello'].isChanged
            }
          }
          steps {
            echo "${buildConfig['hello']}"
          }
        }
      }
    }

    stage('Test') {
      steps {
        echo 'Testing..'
      }
    }
  }
}

@NonCPS
def findTargetPath(buildConfig) {
  // echo "Gathering SCM changes"
  def changeLogSets = currentBuild.changeSets

  for (int i = 0; i < changeLogSets.size(); i++) {
    def entries = changeLogSets[i].items

    for (int j = 0; j < entries.length; j++) {
      def entry = entries[j]

      for (file in entry.getAffectedFiles()) {
        def filePath = file.getPath()
        println "path: ${filePath}"

        for (config in buildConfig) {
          // println "config key: ${config.key}"
          // println "config value: ${config.value}"
          if (filePath ==~ /${config.value.path}(.*)/) {
            println "found Path '${filePath}' contains '${config}'"
            config.value.isChanged = true
          }
        }
      }
    }
  }
}
