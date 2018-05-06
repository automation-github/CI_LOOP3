pipeline {
  agent any
  environment {
    target_cluster = '10.65.182.11'
  }
  stages {
    stage('CI_LOOP3_MASTER') {
      
      parallel {

        stage('CI_LOOP3_5.1_SOLID_179.12') {
          agent any
          steps {
            build (job: 'CI_LOOP3_5.1_SOLID_179.12/master', propagate: false)
          }
        }

        stage('CI_LOOP3_5.1_Solid_182.143') {
          agent any
          steps {
            build (job: 'CI_LOOP3_5.1_Solid_182.143/master', propagate: false)
          }
        }

      }
    }

    stage('copy xmls') {
      steps {
        sh '''cp -p ../CI_LOOP3_5.1_SOLID_182.143/*.xml .
cp -p ../CI_LOOP3_5.1_SOLID_179.12/*.xml .'''
      }
    }

    stage('publish junit results') {
      steps {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }

  }
}
