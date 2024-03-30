pipeline {
    agent any

    environment {
        SONARQUBE_LOGIN = credentials('f861bec6-fef9-462c-b5ae-0ef24474e8b6').username
        SONARQUBE_PASSWORD = credentials('f861bec6-fef9-462c-b5ae-0ef24474e8b6').password
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Getting project from Git"
                
                timestamps {
                    checkout([$class: 'GitSCM', 
                              branches: [[name: '*/master']], 
                              doGenerateSubmoduleConfigurations: false, 
                              extensions: [[$class: 'CleanCheckout']], 
                              submoduleCfg: [], 
                              userRemoteConfigs: [[url: 'https://github.com/OmarEssidd/devops.git']]])
                }
            }
        }

        stage('Clean Project') {
            steps {
                timestamps {
                    sh 'mvn clean'
                }
            }
        }

        stage('Compile Project') {
            steps {
                timestamps {
                    sh 'mvn compile'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MonInstanceSonarQube') {
                    timestamps {
                        sh """
                        mvn sonar:sonar \
                            -Dsonar.login='${SONARQUBE_LOGIN}' \
                            -Dsonar.password='${SONARQUBE_PASSWORD}'
                        """
                    }
                }
            }
        }
    }
}
