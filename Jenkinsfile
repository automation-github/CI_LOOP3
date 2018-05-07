def workspace_179_12 = null

pipeline {
  agent any
  triggers {
    cron('H */4 * * 1-5')
  }

  stages {
    stage('CI_LOOP3_MASTER') {
      parallel {
          stage('CCI_LOOP3_MASTER') {
            agent any
            options {
              timeout(time: 180, unit: 'MINUTES')
            }
            steps {
              script {
                workspace_179_12 = "$WORKSPACE"
                echo workspace_179_12
              }
              build (job: 'CI_LOOP3_5.1_SOLID_179.12/master', propagate: false)
            }
          }
        }
      }

    stage('copy xmls') {
      steps {
        sh "cp -p $workspace_179_12/*.xml ."
      }
    }

    stage('publish junit results') {
      steps {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }

  }
}
