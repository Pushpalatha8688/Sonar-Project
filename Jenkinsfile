pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-tomcat-8.0.52/bin"
    }
    stages{
       stage('Checkout'){
            steps{
                git 'git@github.com:NagiReddyDEVOPS/Sonar-Project.git'
            }
         }        
       stage('Package'){
            steps{
                sh 'mvn clean package'
            }
         }
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('Sonarqube') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn clean verify sonar:sonar -Dsonar.projectKey=sonar -Dsonar.host.url=http://13.232.164.77:9000 -Dsonar.login=sqp_156b51299a00cf8e411a9b64a482c12b6570b025"
    }
        }
        }
       
    stage ('Deploy to Nexus') {
            steps {
      nexusArtifactUploader(
      nexusVersion: 'nexus3',
      protocol: 'http',
      nexusUrl: '13.48.129.106:8081',
      groupId: 'myGroupId',
      version: '1.0-SNAPSHOT',
      repository: 'maven-snapshots',
      credentialsId: 'nexuscredentials',
      artifacts: [
      [artifactId: 'maven-project',
      classifier: '',
      file: 'webapp/target/webapp.war',
      type: 'war']
      ])
      }
        }
        stage ('Deploy to Prod'){
     steps {
        sh 'scp -o StrictHostKeyChecking=no webapp/target/webapp.war root@16.170.241.116:/opt/apache-tomcat-8.0.52/webapps'
           }
   }
}    
}
