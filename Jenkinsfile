// pipeline {
//   agent {
//     node {
//       label 'master'
//       customWorkspace '/var/lib/jenkins/workspace/CI_LOOP3_MASTER'
//     }
//   }
//   triggers {
//     cron('H */4 * * 1-5')
//   }

//   stages {
//     stage('CI_LOOP3_MASTER') {
//       parallel {
//           stage('CI_LOOP3_5.1_SOLID_179.12') {
//             agent any
//             options {
//               timeout(time: 180, unit: 'MINUTES')
//             }
//             steps {
//               build (job: 'CI_LOOP3_5.1_SOLID_179.12/master', propagate: false)
//               sh "cp -p /var/lib/jenkins/workspace/CI_LOOP3_5.1_SOLID_179.12/*.xml /var/lib/jenkins/workspace/CI_LOOP3_MASTER"
//             }
//           }

//           stage('CI_LOOP3_5.1_SOLID_182.143') {
//             agent any
//             options {
//               timeout(time: 180, unit: 'MINUTES')
//             }
//             steps {
//               build (job: 'CI_LOOP3_5.1_Solid_182.143/master', propagate: false)
//               sh "cp -p /var/lib/jenkins/workspace/CI_LOOP3_5.1_SOLID_182.143/*.xml /var/lib/jenkins/workspace/CI_LOOP3_MASTER"
//             }
//           }

//         }
//       }

//     stage('publish junit results') {
//       steps {
//         junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
//       }
//     }

//   }
// }

def BuildJob(projectName) {
    timeout(time: 180, unit: 'MINUTES') {   
      try {
         stage(projectName) {
           // node {      
             def res = build job:projectName, propagate: false
             sh '''cp -p /var/lib/jenkins/workspace/$projectName/*.xml /var/lib/jenkins/workspace/CI_LOOP3_MASTER'''
             result = res.result

             if (result.equals("SUCCESS")) {
             } else {
                error 'FAIL' // this fails the stage
             }
           // }
         }
      } catch (e) {
          currentBuild.result = 'UNSTABLE'
          result = "FAIL" // make sure other exceptions are recorded as failure too
      }
    }
}

node {
  ws('/var/lib/jenkins/workspace/CI_LOOP3_MASTER') {

  // agent { 
  //   label 'master'
  //   customWorkspace '/var/lib/jenkins/workspace/CI_LOOP3_MASTER'
  // }

    // stages {
      // stage('CI_LOOP3_MASTER') {
      parallel '179.12': {
          BuildJob('CI_LOOP3_5.1_SOLID_179.12/master')},
        {
        '182.143':  BuildJob('CI_LOOP3_5.1_SOLID_182.143/master')
        }
      // }  

      stage('publish junit results') {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }

  // }
}