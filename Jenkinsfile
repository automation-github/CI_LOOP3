#!groovy

def BuildJob(jobName) {
    timeout(time: 180, unit: 'MINUTES') {   
      try {
        stage(jobName) {    
            def res = build job:jobName, propagate: false
            def ws_name = jobName.split("/")[0]
            sh "cp -p /var/lib/jenkins/workspace/$ws_name/*.xml /var/lib/jenkins/workspace/CI_LOOP3_MASTER"
            result = res.result

            if (result.equals("SUCCESS")) {
            } else {
               error 'FAIL' // this fails the stage
            }
         }
      } catch (e) {
          currentBuild.result = 'UNSTABLE'
          result = "FAIL" // make sure other exceptions are recorded as failure too
      }
    }
}

node {
  dir('/var/lib/jenkins/workspace/CI_LOOP3_MASTER') {
      stage('CI_LOOP3_MASTER') {
        parallel (
          'CI_LOOP3_5.1_SOLID_179.12':
          {
            BuildJob('CI_LOOP3_5.1_SOLID_179.12/master')
          },
          'CI_LOOP3_5.1_SOLID_182.143':  
          {
            BuildJob('CI_LOOP3_5.1_SOLID_182.143/master')
          }
        )
      }

      stage('publish junit results') {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }
}