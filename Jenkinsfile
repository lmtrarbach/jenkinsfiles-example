#!/usr/bin/groovy
def repositorioGit = "https://github.com/AuthorizeNet/sample-code-java.git"
def canalRocket = "jenkins-promote"
def mavenMirror ="MAVEN_MIRROR_URL='The nexus repo url'"
def namespace="Openshift project name"
//On this node you can build tests with maven
"""
node('maven') {
//  def mvnHome = "/usr/share/maven/"
//  def mvnCmd = "${mvnHome}bin/mvn"
  def mvnCmd = 'mvn'
  //String pomFileLocation = env.BUILD_CONTEXT_DIR ? "${env.BUILD_CONTEXT_DIR}/pom.xml" : "pom.xml"

"""
  stage('SCM Checkout') {
    checkout scm
  }
"""
  stage('SonarQube testes') {
 //       sh "mvn sonar:sonar -Dsonar.host.url=http://sonar-url.com -Dsonar.login=sdadas"
   }



  try {
      //Build Step
  stage('build') {
    //${pomFileLocation}
    sh "${mvnCmd} clean package spring-boot:run -Popenshift -B -DskipTests=true -f pom.xml "
    //Sending a rocket message reporting sucess or failure
    rocketSend channel: "jenkins-promote", message: 'Build executada com sucesso'
}
    } catch (e) {
        rocketSend channel: "jenkins-promote", message: 'Build executada com erros '
        throw e
    }

}
"""
//Use the node master because the oc client
node('master') {
   stage('deploy App')
    script {
        try {
        openshift.withCluster() {
        openshift.withProject(namespace) {
                           def aplicacao = openshift.newApp("--name=${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}", "--image-stream=openshift/wildfly:10.0~${repositorioGit}#${env.BRANCH_NAME} --allow-missing-images ","--context-dir=/cpe-api", "--source-secret=token-openshift","--build-env ${mavenMirror}","showBuildLogs=true").expose();
                           def objetoapp = aplicacao.object()
                           rocketSend channel: "jenkins-promote", message: 'Novo app  criado   com o nome  ${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}'
                           def deployapp = openshiftDeploy(depCfg: '${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}')
                        }
                      }
                    }
        catch (err) {

    }

                }

stage('deploy APP 2')
    script {
        try {
        openshift.withCluster() {
        openshift.withProject(namespace) {
                           def aplicacao = openshift.newApp("--name=${env.BRANCH_NAME}-web-${env.BUILD_NUMBER}", "--image-stream=openshift/wildfly:10.0~${repositorioGit}#${env.BRANCH_NAME} --allow-missing-images ","--context-dir=/cpe-web", "--source-secret=git-api-secret","--build-env ${mavenMirror}","showBuildLogs=true").expose();
                           def objetoapp = aplicacao.object()
                           rocketSend channel: "jenkins-promote", message: 'Novo app  criado   com o nome  ${env.BRANCH_NAME}-web-${env.BUILD_NUMBER}'
                           def deployapp = openshiftDeploy(depCfg: '${env.BRANCH_NAME}-web-${env.BUILD_NUMBER}')
                        }
                      }
                    }
        catch (err) {


    }

                }


}
