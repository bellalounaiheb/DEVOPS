pipeline {
    agent any
    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }
    stages {
        stage('GIT') {
            steps {
                git branch: 'MalekKHELIL-5SAE7-G3',
                    url: 'https://github.com/bellalounaiheb/5SAE7-G3-Kaddem.git'
            }
        }

        stage('MVN CLEAN') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('MVN COMPILE') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Tests - Mockito/JUnit') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Rapport JaCoCo') {
            steps {
                sh 'mvn test'
                sh 'mvn jacoco:report'
            }
        }

        stage('JaCoCo coverage report') {
            steps {
                step([$class: 'JacocoPublisher',
                      execPattern: '**/target/jacoco.exec',
                      classPattern: '**/classes',
                      sourcePattern: '**/src',
                      exclusionPattern: '*/target/**/,**/*Test*,**/*_javassist/**'
                ])
            }
        }

        stage('SonarQube') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=Sonarqube12345# -Dmaven.test.skip=true'
            }
        }

              stage('Nexus Deploy ') {
                    steps {
                        nexusArtifactUploader artifacts: [
                            [
                                artifactId: 'kaddem',
                                classifier: '',
                                file: 'target/kaddem.jar',
                                type: 'jar'
                            ]
                        ],
                         credentialsId: 'nexus3',
                         groupId: 'tn.esprit.spring',
                         nexusUrl: 'localhost:8081',
                         nexusVersion: 'nexus3',
                         protocol: 'http',
                         repository: 'kaddem',
                         version: '0.0.1-SNAPSHOT'
                    }
                }
    }
}
