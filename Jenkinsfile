pipeline {
    agent any

    tools {
        maven "MAVEN3.9"
        jdk "JDK17"
    }

    environment {
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin123'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '172.31.3.162'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-repo'
        NEXUS_LOGIN = 'nexuslogin'
        JAVA_HOME = tool name: 'JDK17', type: 'hudson.model.JDK'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -U -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving..."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn -U -s settings.xml test'
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn -U -s settings.xml checkstyle:checkstyle'
            }
        }
    }
}
