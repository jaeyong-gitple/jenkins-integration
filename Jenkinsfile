#!groovy

def buildConfig = [:]
buildConfig.app = [path: 'server/app', isChanged: false , build: 'cd $TP_TARGET_SOURCE;ls;echo "app make..."']
buildConfig.hello = [path: 'server/hello', isChanged: false , build: 'cd $TP_TARGET_SOURCE;ls;echo "hello make..."']

pipeline {
  agent any
  environment {
    REMOTE_SSH_HOST = 'ci-dev.raidea.io'
    REMOTE_SSH_CREDS = credentials('dev-ci-ssh')
  }
  stages {
    stage('Prepare') {
      steps {
        script {
          findTargetPath(buildConfig)
          syncRemoteGit(env.REMOTE_SSH_CREDS, env.REMOTE_SSH_CREDS_USR, env.GIT_BRANCH, env.GIT_COMMIT)
        }
      }
    }
    stage('Build') {
      parallel {
        stage('Build: app') {
          when {
            expression {
              return buildConfig['app'].isChanged
            }
          }
          steps {
            script {
              buildTarget(buildConfig['app'], env.REMOTE_SSH_CREDS, env.REMOTE_SSH_CREDS_USR)
            }
          }
        }
        stage('Build: hello') {
          when {
            expression {return buildConfig['hello'].isChanged
            }
          }
          steps {
            script {
              buildTarget(buildConfig['hello'], env.REMOTE_SSH_CREDS, env.REMOTE_SSH_CREDS_USR)
            }
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
def syncRemoteGit(identity, userName, branch, commit) {
  def remote = [:]
  remote.name = env.REMOTE_SSH_HOST
  remote.host = env.REMOTE_SSH_HOST
  remote.allowAnyHosts = true
  remote.user = userName
  remote.identityFile = identity

  println "git info branch:'${branch}', commit-hash:'${commit}'"

  sshCommand remote: remote, command: 'cd $TP_TARGET_SOURCE;git status'
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
            // println "found Path '${filePath}' contains '${config}'"
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
