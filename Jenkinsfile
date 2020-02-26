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
          // if (env.BRANCH_NAME != 'master') {
          //   error("Invalid target branch: ${env.GIT_BRANCH}")
          // }

          findTargetPath(buildConfig)
         
          env.GIT_COMMIT_MSG = sh (script: "git log -1 --pretty=%B ${env.GIT_COMMIT}", returnStdout: true).trim()
        }

        echo "GIT_COMMIT: ${env.GIT_COMMIT}"
        echo "GIT_BRANCH: ${env.GIT_BRANCH}"
        echo "GIT_COMMIT_MSG: ${env.GIT_COMMIT_MSG}"
      }
    }
    stage('sh test') {
      steps {
        script {
          def output = """
=================[ init_config ]=================
=================[ user_service ]=================
Configuration on demand is an incubating feature.

> Configure project :
Enabling Kotlin Spring plugin in project auth_service...
Enabling Spring Boot Dependency Management in project auth_service...
Enabling Kotlin Spring plugin in project billing_service...
Enabling Spring Boot Dependency Management in project billing_service...
Enabling Kotlin Spring plugin in project booking_service...
Enabling Spring Boot Dependency Management in project booking_service...
Enabling Kotlin Spring plugin in project butler_service...
Enabling Spring Boot Dependency Management in project butler_service...
Enabling Kotlin Spring plugin in project config_service...
Enabling Spring Boot Dependency Management in project config_service...
Enabling Kotlin Spring plugin in project device_service...
Enabling Spring Boot Dependency Management in project device_service...
Enabling Kotlin Spring plugin in project discovery_service...
Enabling Spring Boot Dependency Management in project discovery_service...
Enabling Kotlin Spring plugin in project entity_user...
Enabling Spring Boot Dependency Management in project entity_user...
Enabling Kotlin Spring plugin in project event_gateway_service...
Enabling Spring Boot Dependency Management in project event_gateway_service...
Enabling Kotlin Spring plugin in project gateway_service...
Enabling Spring Boot Dependency Management in project gateway_service...
Enabling Kotlin Spring plugin in project mongo-sample-module...
Enabling Spring Boot Dependency Management in project mongo-sample-module...
Enabling Kotlin Spring plugin in project object_service...
Enabling Spring Boot Dependency Management in project object_service...
Enabling Kotlin Spring plugin in project product_service...
Enabling Spring Boot Dependency Management in project product_service...
Enabling Kotlin Spring plugin in project ticket_service...
Enabling Spring Boot Dependency Management in project ticket_service...
Enabling Kotlin Spring plugin in project trip_service...
Enabling Spring Boot Dependency Management in project trip_service...
Enabling Kotlin Spring plugin in project turbine_service...
Enabling Spring Boot Dependency Management in project turbine_service...
Enabling Kotlin Spring plugin in project user_service...
Enabling Spring Boot Dependency Management in project user_service...

> Configure project :user_service
Configuring compileKotlin in project user_service...
Configuring compileTestKotlin in project user_service...
Start bootJar...
Start createDockerfile...
Build Env is dev
Start buildDockerImage...
Set created Dockerfile...
Set Input ProjectDir.../home/ubuntu/MSP_DEV/server/msp/user_service
Configuring kaptGenerateStubsKotlin in project user_service...
Configuring kaptGenerateStubsTestKotlin in project user_service...

> Configure project :entity_user
Configuring compileKotlin in project entity_user...
Configuring compileTestKotlin in project entity_user...
Configuring kaptGenerateStubsKotlin in project entity_user...
Configuring kaptGenerateStubsTestKotlin in project entity_user...

> Task :user_service:compileKotlin
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/handler/TestHandler.kt: (98, 13): Variable 'nowepochDay' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/handler/TestHandler.kt: (104, 37): Name shadowed: it
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/repository/TestRepository.kt: (44, 13): Variable 'groupTest' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/CardService.kt: (74, 13): Variable 'updateTime' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/CommonService.kt: (166, 13): Variable 're' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/GroupService.kt: (119, 13): Variable 'updateTime' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/LicenceService.kt: (63, 13): Variable 'updateTime' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberGroupService.kt: (114, 54): 'skip(Int): SkipOperation' is deprecated. Deprecated in Java
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (144, 13): Variable 'updateTime' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (310, 21): Variable 'rs' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (314, 37): Variable 'save' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (318, 21): Variable 'rs' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (322, 37): Variable 'save' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (341, 21): Variable 'rs' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (345, 37): Variable 'save' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (349, 21): Variable 'rs' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (353, 37): Variable 'save' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/MemberService.kt: (426, 13): Name shadowed: check
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/PushTokenService.kt: (49, 13): Variable 'updateTime' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/test/JwtService.kt: (36, 17): Variable 'rs' is never used
w: /home/ubuntu/MSP_DEV/server/msp/user_service/src/main/kotlin/com/msp/user_service/service/test/JwtService.kt: (50, 13): Condition 'jwt != null' is always 'true'

> Task :user_service:buildDockerImage
Building image using context '/home/ubuntu/MSP_DEV/server/msp/user_service'.
Using Dockerfile '/home/ubuntu/MSP_DEV/server/msp/user_service/build/docker/Dockerfile'
Using images 'dev-ciprv.raidea.io:5000/user_service:200226_0616', 'dev-ciprv.raidea.io:5000/user_service:latest'.
Step 1/7 : FROM openjdk:11-slim
 ---> 724512274dbb
Step 2/7 : EXPOSE 8080
 ---> Using cache
 ---> 2dbf33280914
Step 3/7 : WORKDIR /opt/user_service/libs/
 ---> Using cache
 ---> c466a8232bf3
Step 4/7 : RUN pwd
 ---> Using cache
 ---> 8b195bc42dbd
Step 5/7 : COPY ./build/libs/user_service-200226_0616.jar /opt/user_service/libs/user_service-200226_0616.jar
 ---> 2177c3a80be2
Step 6/7 : RUN touch /opt/user_service/libs/user_service-200226_0616.jar
 ---> Running in 07fa60ab0b4f
Removing intermediate container 07fa60ab0b4f
 ---> ce2e0c7d1713
Step 7/7 : ENTRYPOINT ["sh", "-c", "java  -Dspring.profiles.active=dev -Djava.security.egd=file:/dev/./urandom -jar user_service-200226_0616.jar"]
 ---> Running in cb860aa44bb2
Removing intermediate container cb860aa44bb2
 ---> 8290879dd882
Successfully built 8290879dd882
Successfully tagged dev-ciprv.raidea.io:5000/user_service:200226_0616
Successfully tagged dev-ciprv.raidea.io:5000/user_service:latest
Created image with ID '8290879dd882'.

BUILD SUCCESSFUL in 34s
14 actionable tasks: 14 executed
The push refers to repository [dev-ciprv.raidea.io:5000/user_service]
bcc7128da801: Pushed
48e121d6a2de: Pushed
21ebe340a9f9: Layer already exists
5cd1cb67c9e9: Layer already exists
a0efe8c7f4fc: Layer already exists
64d7a6e6c92c: Layer already exists
488dfecc21b1: Layer already exists
200226_0616: digest: sha256:7be74351af2ac418705cd5a1cf8225ba706a6d4d72daf2b4d2d4db6af2c52ddc size: 1793
The push refers to repository [dev-ciprv.raidea.io:5000/user_service]
bcc7128da801: Layer already exists
48e121d6a2de: Layer already exists
21ebe340a9f9: Layer already exists
5cd1cb67c9e9: Layer already exists
a0efe8c7f4fc: Layer already exists
64d7a6e6c92c: Layer already exists
488dfecc21b1: Layer already exists
latest: digest: sha256:7be74351af2ac418705cd5a1cf8225ba706a6d4d72daf2b4d2d4db6af2c52ddc size: 1793

REGISTRY-VERSION=user_service:200226_0616

>>> Entering node: dev-user_service1
make: Entering directory '/home/ubuntu/MSP_DEV/deploy'
=================[ init_config ]=================
=================[ user_service ]=================
=================[ create: user_service at dev-user_service1 image: dev-ciprv.raidea.io:5000/user_service:latest ]=================
dev-ciprv.raidea.io:5000/user_service:latest
WARNING: Published ports are discarded when using host network mode
53fbbab61bf6e90648e88cb55a7286def0b16b950e9bddad32e6b89601619869
make: Leaving directory '/home/ubuntu/MSP_DEV/deploy'
<<< Leaving node: dev-user_service1        
          """

          sh("echo ${output} | grep 'REGISTRY-VERSION=' | tail -1 | awk -F ':' '{print \$2}'")
        }
      }
    }
    stage('Build') {
      parallel {
        stage('Build: app') {
          steps {
            build(job: 'jenkins-integration-app', parameters: [[$class: 'StringParameterValue', name: 'COMMIT_MESSAGE', value: env.GIT_COMMIT_MSG]], wait: true)
          }
        }
        stage('Build: hello') {
          steps {
            build(job: 'jenkins-integration-hello', parameters: [[$class: 'StringParameterValue', name: 'COMMIT_MESSAGE', value: env.GIT_COMMIT_MSG]], wait: true)
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
