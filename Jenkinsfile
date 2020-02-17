#!groovy

pipeline {
  agent any
  stages {
    stage('Get Changed') {
      steps {
        getChangeString()

        script {
          stage('NewOne') {
            echo('new one echo')
          }
        }
      }
    }

    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            echo 'Building..'
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
