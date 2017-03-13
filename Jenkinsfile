#!groovy

node {
  // Invoke Bookstore Build to create the bookstore.war file
  stage('Build bookstore') {
    build 'bookstore'
  }
  // Invoke OpenShift Build to package the bookstore.war file
  // with the S2I image.
  stage('OpenShift Build') {
    openshiftBuild apiURL: '', authToken: '', bldCfg: 'bookstore', checkForTriggeredDeployments: 'false', namespace: OCP_PROJECT, verbose: 'false'
    openshiftVerifyBuild apiURL: '', authToken: '', bldCfg: 'bookstore', checkForTriggeredDeployments: 'false', namespace: OCP_PROJECT, verbose: 'false'
  }
  // Deploy the created S2I Image
  stage('OpenShift Deployment') {
    openshiftDeploy apiURL: '', authToken: '', depCfg: 'bookstore', namespace: OCP_PROJECT, verbose: 'false', waitTime: ''
    openshiftScale apiURL: '', authToken: '', depCfg: 'bookstore', namespace: OCP_PROJECT, replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false'
    openshiftVerifyDeployment apiURL: '', authToken: '', depCfg: 'bookstore', namespace: OCP_PROJECT, replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false', waitTime: ''
  }
  stage('Verify Service') {
    // Starting the bookstore application takes a long time.
    // Retrying 5 times to contact the service to make sure
    // we give enough time for the container to be fully ready.
    retry(5) {
      openshiftVerifyService apiURL: '', authToken: '', namespace: OCP_PROJECT, svcName: 'bookstore', verbose: 'false'
    }
  }
}
