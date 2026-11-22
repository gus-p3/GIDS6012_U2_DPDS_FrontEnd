pipeline {
    agent any
    tools {
        maven 'M3'
        jdk 'JDK21'
    }
    environment {
        PATH = "${env.JENKINS_HOME}/.railway/bin:${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/gus-p3/GIDS6012_U2_DPDS_FrontEnd.git'
            }
        }
        stage('Ver Directorio') {
            steps {
                sh 'ls -la'
                sh 'java -version'
            }
        }
        stage('Build & Test') {
            steps {
                sh '''
                    bash "$MAVEN_HOME/bin/mvn" clean package jacoco:report -Dsurefire.useFile=false
                '''
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarServer') {
                    sh '''
                        bash "$MAVEN_HOME/bin/mvn" \
                        org.sonarsource.scanner.maven:sonar-maven-plugin:5.5.0.6356:sonar \
                        -Dsonar.projectKey=Ejercicio5Cliente \
                        -Dsonar.projectName="Ejercicio5Cliente" \
                        -Dsonar.host.url=http://host.docker.internal:9000 \
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
                    '''
                }
            }
        }
        stage('Security Scan (SAST) - Semgrep') {
            steps {
                sh '''
                    mkdir -p reportes
                    docker run --rm \
                    --network bridge \
                    -v "${WORKSPACE}":/src \
                    -w /src \
                    semgrep/semgrep \
                    semgrep scan \
                    --config=auto \
                    --json \
                    --output=reportes/semgrep.json \
                    .
                '''
            }
        }
        stage('Deploy to Railway') {
            when { expression { return env.GIT_BRANCH in ['origin/main', 'main'] } }
            steps {
                withCredentials([string(credentialsId: 'railway-project-token',
                                        variable: 'RAILWAY_TOKEN')]) {
                    sh '''
                        railway --version
                        railway redeploy --service frontend --from-source --yes
                    '''
                }
            }
        }
        stage('Verify Deployment') {
            when { expression { return env.GIT_BRANCH in ['origin/main', 'main'] } }
            steps {
                sh '''
                    curl -fsS --retry 18 --retry-delay 10 --retry-all-errors \
                         https://gestor-web-bgma.up.railway.app/actuator/health
                '''
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'reportes/semgrep.json',
            allowEmptyArchive: true,
            fingerprint: true
        }
    }
}
