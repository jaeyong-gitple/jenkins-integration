#!groovy

pipeline {
  agent any
  stages {
    stage('Get Changed') {
      steps {
        getChangeString()
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
  println "changeLogSets: $changeLogSets.toString()"
  // print 'changeLogSets.size:'
  // print changeLogSets.size()

  for (int i = 0; i < changeLogSets.size(); i++) {
    def entries = changeLogSets[i].items
    for (int j = 0; j < entries.length; j++) {
      def entry = entries[j]



      truncated_msg = entry.msg.take(MAX_MSG_LEN)
      changeString += " - ${truncated_msg} [${entry.author}]\n"
    }
  }

  if (!changeString) {
    changeString = " - No new changes"
  }
  return changeString
}
