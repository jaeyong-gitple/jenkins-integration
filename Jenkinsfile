#!groovy

pipeline {
  agent any
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
      }
    }

    stage('Build') {
      steps {
        script {
          for (config in ${buildConfig}) {
            if (config.value.isChanged) {
              stage("build: ${config.value.key}") {
                echo "build..... ${config}"
              }
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
