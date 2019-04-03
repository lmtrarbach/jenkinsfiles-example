#!/usr/bin/groovy
def repositorioGit = "https://github.com/bparees/openshift-jee-sample.git"
def mavenMirror ="MAVEN_MIRROR_URL='The nexus repo url'"
def namespace="deploy"
//On this node you can build tests with maven
//node('maven') {
//  def mvnHome = "/usr/share/maven/"
//  def mvnCmd = "${mvnHome}bin/mvn"
//  def mvnCmd = 'mvn'
  //String pomFileLocation = env.BUILD_CONTEXT_DIR ? "${env.BUILD_CONTEXT_DIR}/pom.xml" : "pom.xml"


 

 
//Use the node master because the oc client
node('master') {
  
   stage('SCM Checkout') {
    checkout scm
  }

  
  
   stage('deploy App')
    script {
        
        openshift.withCluster() {
        openshift.withProject(namespace) {
                           def aplicacao = openshift.newApp("--name=${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}", "--image-stream=ddddd~${repositorioGit}#${env.BRANCH_NAME} --allow-missing-images ","--context-dir=/", "--source-secret=token-openshift","showBuildLogs=true").expose();
                           def objetoapp = aplicacao.object()
                           rocketSend channel: "jenkins-promote", message: 'Novo app  criado   com o nome  ${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}'
                           def deployapp = openshiftDeploy(depCfg: '${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}')
                        }
                      }
                    }

                }



                




