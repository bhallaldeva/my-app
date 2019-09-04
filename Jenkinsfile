pipeline {
     agent any
stages {
stage(' ---clean--') {
steps {
sh "mvn clean"
}
}
stage ('--test--') {
steps{
sh "mvn test"
}
}
stage('---package---') {
steps {
sh "mvn package"
}
}
     stage ('---deploy---'){
          steps{
               sh "scp -o StrictHostKeyChecking=no Target/*.war ec2-user@3.17.147.184:/opt/apache-tomcat-8.5.45/webapps/'
          }
}
}
