pipeline {
    agent any
		stages {
        stage ('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Rameshagwd/Multi_Repo.git'
            } 
        }
        stage ('MVN Clean') {
            steps {
                sh 'mvn clean'
            }
        }
        stage ('SonarQube Scan') {
            steps {
              sh 'mvn sonar:sonar -Dsonar.host.url=http://10.32.39.109:9000 -Dsonar.login=b5a5f44290d4f8969b04bfdec7eaaf155ebc5849'  
            }
        }
        stage ('MVN Validate') {
            steps {
                sh 'mvn validate'
            }
        }
        stage ('MVN Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage ('MVN Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('MVN Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage ('Upload JAR to NEXUS') {
            steps {
                script {

                    pom = readMavenPom file: 'pom.xml'
                    def nexuRepoName = pom.version.endsWith("SNAPSHOT") ? "Snapshot_repo_nexus" : "release-repo"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'Release_Snapshot_Repo',
                            classifier: '',
                            file: "target/Release_Snapshot_Repo-${pom.version}.jar",
                            type: 'jar'
                        ]
                    ],

                    credentialsId: 'Nexus',
                    groupId: 'com.javatpoint.application1',
                    nexusVersion: 'nexus3',
                    nexusUrl: '10.32.39.203:8081',
                    protocol: 'http',
                    repository: nexuRepoName,
                    version: "${pom.version}"
                }
            }
        }   
    }
}