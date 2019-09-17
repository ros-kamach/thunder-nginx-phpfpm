// define Logrotation and scm Polling
            properties(
                [
                    [
                        $class: 'BuildDiscarderProperty',
                        strategy: [$class: 'LogRotator', numToKeepStr: '10']
                    ],
                      git url: 'https://github.com/ros-kamach/thunder_nginx_phpfpm.git',      
                ]
            )

            try{
                node('') {

                    stage('Checkout'){
                        checkout scm
                    }

                    // trigger Build on OpenShift
                    stage('buildOnOpenShift'){
                        openshiftBuild(buildConfig: 'thunder-alpine', showBuildLogs: 'true')
                    }

                }
            } catch (Exception e) {
                // Notify
                echo "send error mail to ros.kamach@gmail.com"
                mail subject: "Build failed with ${e.message}", to: "jenkins@appuio.ch", body: "Job failed: ${env.BUILD_URL} \n\n${e.stackTrace}"
                throw e;
            }