pipeline {
  agent any
  triggers {
    cron('H */4 * * 1-5')
  }

  stages {
    stage('CI_LOOP3_MASTER') {
      parallel {
       
          stage('CI_LOOP3_5.1_SOLID_179.12') {
            agent any
            options {
              timeout(time: 180, unit: 'MINUTES')
            }
            steps {
              build (job: 'CI_LOOP3_5.1_SOLID_179.12/master', propagate: false)
            }
          }

          stage('CI_LOOP3_5.1_Solid_182.143') {
            agent any
            options {
              timeout(time: 180, unit: 'MINUTES')
            }
            steps {
              build (job: 'CI_LOOP3_5.1_Solid_182.143/master', propagate: false)
            }
          }

        }
      }

    stage('copy xmls') {
      steps {
        sh '''cp -p ../CI_LOOP3_5.1_SOLID_182.143/branches/master/*.xml .
cp -p ../CI_LOOP3_5.1_SOLID_179.12/branches/master/*.xml .'''
      }
    }

    stage('publish junit results') {
      steps {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }

  }
}
