pipeline {
    agent any

    stages {
        stage('Test Kubernetes Connection') {
	     steps {
	         withCredentials([
		     file(
		         credentialsId: 'kind-kubeconfig',
			 variable: 'KUBECONFIG'

	             )
		 ]) {
		     bat 'kubectl get nodes'
		 }
	      }
	 }
    }
}
