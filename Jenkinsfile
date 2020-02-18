#!groovy

pipeline {
  agent any
  environment {
    REMOTE_HOST = 'ci-staging.mspdev.link'
    REMOTE_USER = 'ubuntu'
    REMOTE_USER = credentials('ci-ssh')
  }
  stages {
    stage('Prepare') {
      // when {
      //   expression {
      //       // Given our default value is true, this should 
      //       // run if I don't change the parameter from its 
      //       // default value of true, to false.
      //     return true // hasTargetPath('server/aa')
      //   }
      // }
      steps {
        script {
          buildConfig = [:]

          buildConfig.app = [path: 'server/app', isChanged: false , build: 'make app', deploy: 'deploy app']
          buildConfig.hello = [path: 'server/hello', isChanged: false , build: 'make hello', deploy: 'deploy hello']

          findTargetPath(buildConfig);
        }

        echo "${buildConfig}" 

        withCredentials([sshUserPrivateKey(credentialsId: 'ci-ssh', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
          def remote = [:]
          // remote.name = "test-ssh-vm"
          remote.host = env.REMOTE_HOST
          remote.allowAnyHosts = true
          remote.user = userName
          remote.identityFile = identity //remote.passphrase = passphrase stage("SSH Steps Rocks!") { writeFile file: 'test.sh', text: 'ls' sshCommand remote: remote, command: 'for i in {1..5}; do echo -n \"Loop \$i \"; date ; sleep 1; done' sshScript remote: remote, script: 'test.sh' sshPut remote: remote, from: 'test.sh', into: '.' sshGet remote: remote, from: 'test.sh', into: 'test_new.sh', override: true sshRemove remote: remote, path: 'test.sh' } }
          shCommand remote: remote, command: 'ls'
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

    stage('Deploy') {
      steps {
        echo 'Deploying....'
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
