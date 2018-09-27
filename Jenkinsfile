pipeline {
    agent any
    stages {

        stage('Deploy APP from Github') {
          steps {
            script {
              openshift.withCluster() {
                openshift.withProject("ticforum2018-php") {
		  def app2 = openshift.selector( "bc", "appweb2")
		  def app2Exists = app2.exists()
                  if (!app2Exists) {
                           def app = openshift.newApp("https://github.com/giondo/appweb2.git","php")
                           app.narrow("svc").expose();
			   logs('-f')
		           //def dc = app.object()
                  	   //echo "new-app created a ${dc.kind} with name ${dc.metadata.name}"
                  	   //echo "The object has labels: ${dc.metadata.labels}"
                      } else {
			   def buildSelector = app2.startBuild()
			   buildSelector.logs('-f')

                      }
                }
              }
            }
          }
        }
    }
}
