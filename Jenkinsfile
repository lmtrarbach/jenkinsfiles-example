
   timeout(time: 20, unit: 'MINUTES') {
      def appName="openshift-jee-sample"
      def project="project"
      def dockerUser="rafael"
      def imageName= "petshop"
      def imageVersion= "${env.BUILD_NUMBER}"
      def dockerLogin = "rafael" 
      def dockerPass= "rafael123"
      """
      node("docker"){
               stage("Checkout") {
        
                     git url: "https://github.com/tiaguinholuz10/PetShop.git", branch: "master"
                                 }
 
                stage("build"){
                     sh "docker build . -t ${dockerUser}/${imageName}:${imageVersion}"
                     sh "docker build . -t ${dockerUser}/${imageName}:latest"
                                 }
                stage("doccker login"){
            
                     sh "docker login -u ${dockerLogin}  -p ${dockerPass}"

            
                                 }
                stage("docker push"){
         
                     sh "docker push ${dockerUser}/${imageName}:${imageVersion}"
                     sh "docker push ${dockerUser}/${imageName}:latest"
            
            
                                 }
                     }
      """
      node("maven") {
               stage("Checkout") {
                  git url: "https://github.com/tiaguinholuz10/PetShop.git", branch: "master"
                                 }
               stage("Build WAR") {
                  
                  
                  dir ('pet-shop') {
                     sh 'pwd'

                   
                     //sh "mvn test"
                     sh "mvn clean install"
                     stash name:"war", includes:"pet-shop/pom.xml"
                                   }
                                  }
                     }

     
  
      node {

               stage('deploy Master')
                  script {
        
                      openshift.withCluster() {
                       openshift.withProject("teste") {
                           def aplicacao = openshift.newApp("--name=${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}","--image-stream=openshift/wildfly:10.0~https://github.com/tiaguinholuz10/PetShop.git --allow-missing-images").expose();
                           def objetoapp = aplicacao.object()
                           def deployapp = openshiftDeploy(depCfg: '${env.BRANCH_NAME}-api-${env.BUILD_NUMBER}')
                                                         }
                                               }
                          }

                }

   }

