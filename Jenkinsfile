node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      dir ('jenkinsproject') { 
         //commitId = sh(returnStdout: true, script: 'git rev-parse HEAD')
         def scmAction = currentBuild?.rawBuild.actions.find { action -> 
            action instanceof jenkins.scm.api.SCMRevisionAction
         }
         print ">>>"+ scmAction?.revision?.hash
      }
      git 'https://github.com/jglick/simple-maven-project-with-tests.git'
      mvnHome = tool 'myMaven'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
}
