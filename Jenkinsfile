#!groovy

pipeline {
  agent any
  environment {
    REMOTE_SSH_HOST = 'ci-staging.mspdev.link'
    REMOTE_SSH_USER = 'ubuntu'
    REMOTE_SSH_CREDS = credentials('ci-ssh')
  }
  stages {
    stage('Prepare') {
      steps {
        script {
          buildConfig = [:]

          buildConfig.app = [path: 'server/app', isChanged: false , build: 'cd $TP_TARGET_SOURCE;ls;echo "app make..."']
          buildConfig.hello = [path: 'server/hello', isChanged: false , build: 'cd $TP_TARGET_SOURCE;ls;echo "hello make..."']

          findTargetPath(buildConfig)
        }

        echo "${buildConfig}"

        sh 'echo "SSH private key is located at $REMOTE_SSH_CREDS"'
        sh 'echo "SSH user is $REMOTE_SSH_CREDS_USR"'
        sh 'echo "SSH key is $REMOTE_SSH_CREDS"'

        // default env
        // GIT_COMMIT=520843eb66353c8dfa40ca24c82dced3beafc482
        // GIT_BRANCH=develop
        sh "printenv"
      }
    }

    stage('1') {
      steps {
        script {
          def tests = [:]
          for (config in buildConfig) {
            echo "${config}"
            echo "${config.key}"
            tests["${config.key}"] = {
              // node {
                stage("${config.key}") {
                  echo '${config.key}'
                }
              // }
            }
          }
          parallel tests
        }
      }
    }

    // stage('Build') {
    //   parallel {
    //     stage('Build: app') {
    //       // when {
    //       //   expression {
    //       //     return buildConfig['app'].isChanged
    //       //   }
    //       // }
    //       steps {
    //         script {
    //           buildTarget(buildConfig['app'], env.REMOTE_SSH_CREDS, env.REMOTE_SSH_CREDS_USR)
    //         }
    //         echo "${buildConfig['app']}"
    //       }
    //     }
    //     stage('Build: hello') {
    //       // when {
    //       //   expression {
    //       //     return buildConfig['hello'].isChanged
    //       //   }
    //       // }
    //       steps {
    //         script {
    //           buildTarget(buildConfig['hello'], env.REMOTE_SSH_CREDS, env.REMOTE_SSH_CREDS_USR)
    //         }
    //         echo "${buildConfig['hello']}"
    //       }
    //     }
    //   }
    // }

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

@NonCPS
def buildTarget(buildConfig, identity, userName) {
  def remote = [:]
  remote.name = env.REMOTE_SSH_HOST
  remote.host = env.REMOTE_SSH_HOST
  remote.allowAnyHosts = true
  remote.user = userName
  remote.identityFile = identity
  sshCommand remote: remote, command: "${buildConfig.build}"
}
