pipeline {
    agent any
    stages {
        
        stage ("Code Clone")
        {
            steps{
                
                sh "rm -rf spring3hibernate; git clone git@gitlab.com:arunangshu_ops/spring3hibernate.git"
            }
        }
        
        stage ("Code Stablity")
        {
            steps{
                
                sh "cd spring3hibernate; mvn compile"
            }
        }
        stage ("Code Quality")
        {
         steps{
                
                sh "cd spring3hibernate; mvn checkstyle:checkstyle; mvn findbugs:findbugs"
           
            }
        }
        stage ("code quality")
        {
            steps{
                sh "cd spring3hibernate; mvn cobertura:cobertura"
            }
        }
        stage('Email') {
  steps {
      script {
          def mailRecipients = 'shridharan.7@gmail.com'
          def jobName = currentBuild.fullDisplayName
          emailext body: '''${SCRIPT, template="groovy-html.template"}''',
          mimeType: 'text/html',
          subject: "[Jenkins] ${jobName}",
          to: "${mailRecipients}",
          replyTo: "${mailRecipients}",
          recipientProviders: [[$class: 'CulpritsRecipientProvider']]
      }
  }
}
 stage ("slack"){
     steps {
         wrap([$class: 'BuildUser']) { script { env.USER_ID = "${BUILD_USER_ID}" } }
       slackSend (color: "${env.SLACK_COLOR_INFO}",
                  channel: "${params.SLACK_CHANNEL_1}",message: "STARTED: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} by ${env.USER_ID}\n More info at: ${env.BUILD_URL}")
     }
 }
 stage ("code build")
        {
            steps{
                sh "cd spring3hibernate; mvn package"
            }
        }
 
 stage ("deploy"){
     steps{
         sh "sudo cp spring3hibernate/target/Spring3HibernateApp.war /opt/apache-tomcat-8.5.47/webapps/" 
     }
         
 }
    }
 post {
        always {

            recordIssues enabledForFailure: true, tools: [checkStyle(pattern: '**/spring3hibernate/target/checkstyle-result.xml')]
            recordIssues enabledForFailure: true, tools: [findBugs(pattern: '**/spring3hibernate/target/findbugs.xml')]
            step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])

      }
        
    }
}
