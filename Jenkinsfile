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
        SONAR_SERVER = 'sonarserver'
        SONAR_SCANNER = 'sonarscanner'
    }

    stages {
        stage('Install'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
        stage('Test'){
            steps {
                sh 'mvn test'
            }
        }

        stage('Checkstyle Analysis'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Upload Reports To SonarQube'){
            environment {
                scannerHome = tool "${SONAR_SCANNER}"
            }
            steps {
                withSonarQubeEnv("${SONAR_SERVER}") {
                    // This expands the evironment variables SONAR_CONFIG_NAME, SONAR_HOST_URL, SONAR_AUTH_TOKEN that can be used by any script.
                    ///sh '''
                    ///${sonarHome}/bin/sonar-scanner \
                    ///    -Dsonar.projectkey=vprofilemjeed \
                    ///    -Dsonar.sources=./src/ \
                    ///    -Dsonar.host.url=http://172.31.45.194
                    ///'''
                    sh "${scannerHome}/bin/sonar-scanner -X \
                        -Dsonar.projectKey=vprofile \
                        -Dsonar.sources=./src/ \
                        -Dsonar.host.url=http://172.31.45.194"
                }
            }
        }
//        stage("Quality Gate") {
//            steps {
//              timeout(time: 1, unit: 'HOURS') {
//                waitForQualityGate abortPipeline: true
//              }
//            }
//          }
    }
}