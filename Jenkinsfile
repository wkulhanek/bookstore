#!groovy

node {
   stage 'Build bookstore'
   build 'bookstore'

   stage 'OpenShift Build'
   openshiftBuild apiURL: '', authToken: '', bldCfg: 'bookstore', checkForTriggeredDeployments: 'false', namespace: OCP_PROJECT, verbose: 'false'
   openshiftVerifyBuild apiURL: '', authToken: '', bldCfg: 'bookstore', checkForTriggeredDeployments: 'false', namespace: OCP_PROJECT, verbose: 'false'

   stage 'OpenShift Deployment'
   openshiftDeploy apiURL: '', authToken: '', depCfg: 'bookstore', namespace: OCP_PROJECT, verbose: 'false', waitTime: ''
   openshiftScale apiURL: '', authToken: '', depCfg: 'bookstore', namespace: OCP_PROJECT, replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false'
   openshiftVerifyDeployment apiURL: '', authToken: '', depCfg: 'bookstore', namespace: OCP_PROJECT, replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false', waitTime: '6000'

   stage 'Verify Service'
   openshiftVerifyService apiURL: '', authToken: '', namespace: OCP_PROJECT, svcName: 'bookstore', verbose: 'false'
}

