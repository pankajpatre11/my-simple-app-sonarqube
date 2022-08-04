pipeline 
{
    agent any

    environment{

    }
    options {
       buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
       }
    stages
    {

                            
        stage('SonarQube analysis') {
            
             
            steps {
		      	    
               withSonarQubeEnv('SonarQube') {
                  sh "mvn sonar:sonar"
	       }
            }
        }
        


        stage('Upload War To Repo'){
            steps{ 
                script{
                def mavenPom = readMavenPom file: 'pom.xml'
                def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "maven-snapshots" : "maven-releases"
                nexusArtifactUploader artifacts: 
                    [[artifactId: 'maven-project',
                      classifier: '',
                      file: "target/maven-project-${mavenPom.version}.war",
                      type: 'war'
                     ]],
                    credentialsId: 'nexusid',
                    groupId: 'com.example',
                    nexusUrl: '18.208.249.204:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: nexusRepoName ,
                    version: "${mavenPom.version}"
                }
            }
        }
	    

	  	    
    
	    
    }
}



