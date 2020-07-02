node {
   // def server = Artifactory.server 'Artifactory2'  * commented by umang and preasad
 

    //def mvnHome 
    stage('SCM Checkout'){
        git 'https://github.com/PRASAD4CNET/hello-world-spring-boot.git'
    //mvnHome = tool 'Maven'
   }
   
    stage("build & SonarQube analysis") {
         
        withSonarQubeEnv('Sonarqube') {
            // sh 'mvn sonar:sonar '
           sh 'mvn sonar:hello-world-spring-boot '
        }    
         
    }
      
   /*   stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
                  mail bcc: '', body: '''Hi Welcome to jenkins email alerts...... Your sonarqube code analysis has failed .
                  Thanks
                  Hari''', cc: '', from: '', replyTo: '', subject: 'Jenkins Job', to: 'umang1985@gmail.com'
              }
              
          }
      }*/
    stage('Compile-Package'){
      // Get maven home path
      sh "mvn clean package"
   }
   stage('Artifactory upload') {
        def uploadSpec = """
        { "files": [ { "pattern": "/var/lib/jenkins/workspace/DeployDocker2/target/*.war",
                    "target": "giri" } ] }""" 
            server.upload(uploadSpec) 
       
        }
        
    stage('downloading artifact') { 
        def downloadSpec="""
        {
            "files":[ { "pattern":"giri/FinalAssignment.war", "target":"/var/lib/jenkins/workspace/DeployDocker2/" } ] }""" 
        server.download(downloadSpec)
    }  
   stage('Tommy') {
     //  sh 'scp /var/lib/jenkins/workspace/Springmvc1/*.war minduser@kiran1.eastus.cloudapp.azure.com:/opt/tomcat/webapps/'
     sh 'sudo cp /var/lib/jenkins/workspace/DeployDocker2/FinalAssignment.war /opt/tomcat/apache-tomcat-8.5.37/webapps/'
     sh 'sudo docker stop tom3 '
     sh 'sudo docker rm tom3'
     sh 'sudo docker run -d --name tom3 -v /opt/tomcat/apache-tomcat-8.5.37/webapps/:/usr/local/tomcat/webapps -p 8086:8080  tommy'
   }
}
