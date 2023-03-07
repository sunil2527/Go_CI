@Library('test') _
pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }
    stages {
        stage('checkout') {
            steps {
              script{
              checkout.repo_clone("Repo_Url": "https://github.com/Snatak-Fantastic4/Salary.git", "Repo_Branch": "main", "Repo_Credential": "newtoken")
              }
            
            }
        }
        stage('Parallel') {
            parallel {
              stage('credScanner') {
                steps {
                 script {
                  credScanner.cred("repourl": "https://github.com/Snatak-Fantastic4/Salary.git" "branch": "main" "access-token": "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" )
                }
            }
        }
              stage('lisenceFinder') {
                 steps {
                   dir('salaryv2'){
                  catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    licenseFinder()
                }
            }
          }
        }
              stage ('Test') {
                steps {
                 dir('salaryv2'){
                  script {
                    java.java("command": "cobertura:cobertura")
                }
              }
            } 
          }   
              // stage('DependencyCheck') {
        //     steps {
        //         dependencyCheck()
        //     }
        // }
                
        }
      }
       
        stage('Parallel Jobs') {
            parallel {
                stage('Sonar') {
                    steps {
                           sh '''
                cd salaryv2
                mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=123 -Dsonar.host.url=http://43.205.151.128:9000/ -Dsonar.projectKey=f4 -Dsonar.java.binaries=target
                '''
                    }
                }
                stage('Code coverage'){
                    steps {
                        dir('salaryv2'){
                        codeCoverage()
                    }
                }
            }
        }
    }
        stage('Build') {
            steps {
                 dir('salaryv2'){
        script {
            java.java("command": "package")
                }
            }
        }
      }
    }
}

