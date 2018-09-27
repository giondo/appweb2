pipeline {
    agent any
    stages {

        stage('Deploy APP from Github') {
          steps {
            script {
              openshift.withCluster() {
                openshift.withProject("ticforum2018") {
		  def app1 = openshift.selector( "bc", "appweb1")

                  if (!app1) {
                           def app = openshift.newApp("https://github.com/giondo/appweb1.git","httpd")
                           app.narrow("svc").expose();
		           def dc = app.object()
                  	   echo "new-app created a ${dc.kind} with name ${dc.metadata.name}"
                  	   echo "The object has labels: ${dc.metadata.labels}"
                      } else {
			   def buildSelector = app1.startBuild()
			   buildSelector.logs('-f')

                      }
                }
              }
            }
          }
        }
    }
}
