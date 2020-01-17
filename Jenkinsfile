def msBuildSolutionFile = "Enterprises.Jenkins.Demo.sln";

 
pipeline {
    agent { label 'master' }
    options {
        timeout(time: 120, unit: 'MINUTES')
    }
    stages {
        
        stage('Build') {
            steps {
                script {
                    def versionNew = 120.52.52.1
                    env.VERSION_NEW = versionNew
					echo "Start Build Branch: ${env.GIT_BRANCH} ${env.GIT_BRANCH.indexOf("hotfix")}"
					
                    echo "New version computed: ${versionNew}, set in enviroment env.VERSION_NEW: ${env.VERSION_NEW}"
                }
                script {
                    bat "dotnet msbuild /m /verbosity:minimal /t:Restore /t:Rebuild /p:Configuration=Release /p:PublishDir=./bin/PublishTemp/ src/${msBuildSolutionFile}"
                }
            }
        }       
        
    }
    post {
        success {
            script {
                echo "Build success"
                echo "currentResult: ${currentBuild.currentResult} result: ${currentBuild.result}"
            }
        }
        always {
            script {
                emailext subject: "${currentBuild.currentResult} : ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                         body: """<p>EXECUTED: Job <b>\'${env.JOB_NAME}: #${env.BUILD_NUMBER}\' ${currentBuild.currentResult}
                                  </b></p><p>View console output at "<a href="${env.BUILD_URL}">
                                  ${env.JOB_NAME}: #${env.BUILD_NUMBER}</a>"</p> """,
                         recipientProviders:  [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider'], [$class: 'CulpritsRecipientProvider']]
            }
        }
    }
}
