#!groovy
// Set OCP_PROJECT to the name of the OpenShift Project
// that contains Bookstore
// env.OCP_PROJECT = 'wkappdevocp'

node {
   stage 'Checkout'

   // Get Code from a GitHub repository
   // git url: 'https://github.com/wkulhanek/ocp-appdev-bookstore.git'

   env.JAVA_HOME = tool 'JDK8'
   
   // Get the maven tool.
   def mvnHome = tool 'Maven3'

   // Mark the code build 'stage'....
   stage 'Build war file'
   // Run the maven build
   // sh "${mvnHome}/bin/mvn clean package -DskipTests"
   // archive 'target/*.war'
   
   echo '*************'
   echo ${OCP_PROJECT}
   echo '*************'

   stage 'OpenShift Build'
   openshiftBuild bldCfg: 'bookstore', checkForTriggeredDeployments: 'false', namespace: ${OCP_PROJECT}, verbose: 'false'
   
   openshiftVerifyBuild apiURL: '', authToken: '', bldCfg: 'bookstore', checkForTriggeredDeployments: 'false', namespace: $OCP_PROJECT, verbose: 'false'

   stage 'OpenShift Deployment'
   openshiftDeploy apiURL: '', authToken: '', depCfg: 'bookstore', namespace: $OCP_PROJECT, verbose: 'false', waitTime: ''

   openshiftScale apiURL: '', authToken: '', depCfg: 'bookstore', namespace: $OCP_PROJECT, replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false'

   oopenshiftVerifyDeployment apiURL: '', authToken: '', depCfg: 'bookstore', namespace: $OCP_PROJECT, replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false', waitTime: '6000'

   stage 'Verify Service'
   openshiftVerifyService apiURL: '', authToken: '', namespace: $OCP_PROJECT, svcName: 'bookstore', verbose: 'false'
}

