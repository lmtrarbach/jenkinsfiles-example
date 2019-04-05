try {
   timeout(time: 20, unit: 'MINUTES') {
      def appName="openshift-jee-sample"
      def project=""

      node {
        stage("Initialize") {
          project = env.PROJECT_NAME
        }
      }
      
      
      node("nodejs"){
          
          stage("Subindo node"){
              
              sh "echo hello Node"
              
              
              
              
              
          }
          
          
      }

      node("maven") {
        stage("Checkout") {
          git url: "https://github.com/tiaguinholuz10/PetShop.git", branch: "master"
        }
        stage("Build WAR") {
          sh "mvn clean install"
          stash name:"war", includes:"pet-shop/pom.xml"
        }
      }

      node {
        stage("Build Image") {
          unstash name:"war"
          sh "oc start-build ${appName}-docker --from-file=target/ROOT.war -n ${project}"
          timeout(time: 20, unit: 'MINUTES') {
            openshift.withCluster() {
              openshift.withProject() {
                def bc = openshift.selector('bc', "${appName}-docker")
                echo "Found 1 ${bc.count()} buildconfig"
                def blds = bc.related('builds')
                blds.untilEach {
                  return it.object().status.phase == "Complete"
                }
              }
            }  
          }
        }
        stage("Deploy") {
          openshift.withCluster() {
            openshift.withProject() {
              def dc = openshift.selector('dc', "${appName}")
              dc.rollout().status()
            }
          }
        }
      }
   }
} catch (err) {
   echo "in catch block"
   echo "Caught: ${err}"
   currentBuild.result = 'FAILURE'
   throw err
}
  
node {

   stage('deploy App')
    script {
        
        openshift.withCluster() {
        openshift.withProject(namespace) {
                           def aplicacao = openshift.newApp("--name=${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}","--image-stream=openshift/wildfly:10.0~${repositorioGit} --allow-missing-images").expose();
                           def objetoapp = aplicacao.object()
                           rocketSend channel: "jenkins-promote", message: 'Novo app  criado   com o nome  ${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}'
                           def deployapp = openshiftDeploy(depCfg: '${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}')
                        }
                      }
                    }

                }



                




