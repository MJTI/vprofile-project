pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "JDK17"
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
		NEXUS_USER = 'admin'
		NEXUS_PASS = 'admin'
		RELEASE_REPO = 'vprofile-release'
		CENTRAL_REPO = 'vpro-maven-central'
		NEXUSIP = '172.31.47.168'
		NEXUSPORT = '8081'
		NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
    }

    stages {
        stage('Clone The Project'){
            steps {
                git (
                    url: "https://github.com/MJTI/vprofile-project.git",
                    branch: "jenkins-ci",
                    poll: true
                )
            }
        }
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }
}