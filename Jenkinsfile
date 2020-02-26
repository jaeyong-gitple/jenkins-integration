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

        echo "GIT_COMMIT: ${env.GIT_COMMIT}"
      }
    }
    stage('Build') {
      parallel {
        stage('Build: app') {
          steps {
            build(job: 'jenkins-integration-app', parameters: [[$class: 'StringParameterValue', name: 'param1', value: 'app']], wait: true)
          }
        }
        stage('Build: hello') {
          steps {
            build(job: 'jenkins-integration-hello', parameters: [[$class: 'StringParameterValue', name: 'param1', value: 'hello11111']], wait: true)
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
  // def gitCheckout = "cd \$TP_TARGET_SOURCE; echo 'git checkout -b ${branch} ${commit}'"

  def gitCheckout = """
    echo "git reset --hard"
    echo "git clean -fdx"
    echo "git checkout -f ${commit}"
    echo "git branch -a -v --no-abbrev"
    echo "git branch -D ${branch}"
    echo "git checkout -b ${branch} ${commit}"
    echo "git reset --hard"
    echo "git clean -fdx"
  """
  
  sh "${gitCheckout}"

  // sshCommand remote: remote, command: "${gitCheckout}"
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
