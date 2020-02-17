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
      parallel {
        stage('Build') {
          steps {
            echo "Building.. ${buildConfig}"
          }
        }

        stage('Build2') {
          steps {
            echo 'build2'
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
          println "config key: ${config.key}"
          println "config value: ${config.value}"
          if (filePath ==~ /${config.value.path}(.*)/) {
            println "found Path '${filePath}' contains '${config}'"
          }
        }
      }
    }
  }

  return false
}

@NonCPS
def getChangeString() {
  MAX_MSG_LEN = 100
  def changeString = ""

  echo "Gathering SCM changes"
  def changeLogSets = currentBuild.changeSets

  // testjy
  println "changeLogSets: $changeLogSets"

  for (int i = 0; i < changeLogSets.size(); i++) {
    def entries = changeLogSets[i].items

    // testjy
    println "entries: $entries"

    for (int j = 0; j < entries.length; j++) {
      def entry = entries[j]
      // def files = entry.getAffectedFiles()

      // testjy
      println "entry: $entry"

      for (file in entry.getAffectedFiles()) {
        println "path: ${file.getPath()}"
      }

      // for (int k = 0; k < files.size(); k++) {
      //   def file = files[k]
      //   println "  ${file.editType.name} ${file.path}"
      // }

      truncated_msg = entry.msg.take(MAX_MSG_LEN)

      // testjy
      println "truncated_msg: $truncated_msg"

      changeString += " - ${truncated_msg} [${entry.author}]\n"
    }
  }

  if (!changeString) {
    changeString = " - No new changes"
  }

  // testjy
  println "changeString: $changeString"

  return changeString
}
