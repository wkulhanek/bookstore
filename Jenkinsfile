#!groovy

node {
   stage 'Build war file'

   env.JAVA_HOME = tool 'JDK8'
   
   // Get the maven tool.
   def mvnHome = tool 'Maven3'

   // Run the maven build
   sh "${mvnHome}/bin/mvn clean package -DskipTests"
   archive 'target/*.war'
   
   stage 'OpenShift Build'
   openshiftBuild bldCfg: 'bookstore', checkForTriggeredDeployments: 'false', namespace: OCP_PROJECT, verbose: 'false'
   
   openshiftVerifyBuild apiURL: '', authToken: '', bldCfg: 'bookstore', checkForTriggeredDeployments: 'false', namespace: OCP_PROJECT, verbose: 'false'

   stage 'OpenShift Deployment'
   openshiftDeploy apiURL: '', authToken: '', depCfg: 'bookstore', namespace: OCP_PROJECT, verbose: 'false', waitTime: ''

   openshiftScale apiURL: '', authToken: '', depCfg: 'bookstore', namespace: OCP_PROJECT, replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false'

   openshiftVerifyDeployment apiURL: '', authToken: '', depCfg: 'bookstore', namespace: OCP_PROJECT, replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false', waitTime: '6000'

   stage 'Verify Service'
   openshiftVerifyService apiURL: '', authToken: '', namespace: OCP_PROJECT, svcName: 'bookstore', verbose: 'false'
}

