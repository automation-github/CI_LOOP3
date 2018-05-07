pipeline {
  agent {
    node {
      label 'master'
      customWorkspace '/var/lib/jenkins/workspace/CI_LOOP3_MASTER'
    }
  }
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
              sh "cp -p /var/lib/jenkins/workspace/CI_LOOP3_5.1_SOLID_179.12/*.xml /var/lib/jenkins/workspace/CI_LOOP3_MASTER"
            }
          }

          stage('CI_LOOP3_5.1_SOLID_182.143') {
            agent any
            options {
              timeout(time: 180, unit: 'MINUTES')
            }
            steps {
              build (job: 'CI_LOOP3_5.1_Solid_182.143/master', propagate: false)
              sh "cp -p /var/lib/jenkins/workspace/CI_LOOP3_5.1_SOLID_182.143/*.xml /var/lib/jenkins/workspace/CI_LOOP3_MASTER"
            }
          }

        }
      }

    stage('publish junit results') {
      steps {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }

  }
}
