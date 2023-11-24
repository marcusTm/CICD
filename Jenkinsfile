// Declarative pipeline
pipeline { // "pipeline" must be top-level
  agent any // Agent specifies where to execute 

  parameters { // parameters for the build. These can be used in all the stages using the defined name
    string(name: 'VERSION', defaultValue: '0.0.0', description: 'verison to deploy on prod')
    choice(name: 'ENV', choices: ['DEV', 'TEST', 'PROD'], description: '')
    booleanParam(name: 'executeTests', defaultValue: true, description: '')
  }

  tools { // Used to access build tools for the project, e.g. maven, gradle..
    maven 'Maven' // This makes maven avaible in all the stages
  }

  environment { 
    // this block contains custom environment variables that can be used in all the stages in the pipeline
    NEW_VERSION = '1.0.1'
    SERVER_CREDENTIALS = credentials('server-credentials') // This binds the credential to the environment variable
  }

  stages {
    stage("build") {
      steps {
        echo 'building the application..'
        echo "building version ${NEW_VERSION}"
      }
    }
    stage("test") {
      when { // the when-block can be used to determine when this stage should be executed
        expresion { // BRANCH_NAME is a environment variable
          BRANCH_NAME == 'dev' && params.executeTests
        }
      }
      steps {
        echo 'testing the application..'
        script {
          // this is just a groovy script
          def hey = "abc"
          println(hey)
          def gv = load "script.groovy" // It is als possible to import whole groovy files
          gv.someFunction()
        }
      }
    }
    stage("deploy") {
      steps {
        echo 'deploying the application..'
        echo "deploying with ${SERVER_CREDENTIALS}"
        echo "deploying with version ${params.VERSION}"
      }
    }
    stage("deploy1") {
      steps {
        echo 'deploying the application using credentials wrapper'
        withCredentials([
          usernamePassword(credentials: 'server-credentials', usernameVariable: USER, passwordVariable: PWD)
        ]) {
          echo "${USER} ${PWD}"
        }

      }
    }

  }

  post { // Contains logic that is executed after all stages have executed
    always {
      // always runs nomatter the build status
    }
    success {
      // only when the build has succeeded
    }
    failure {
      //  
    }
  }
}

// The availble environment variables from Jenkins can be found at: /env-vars.html


