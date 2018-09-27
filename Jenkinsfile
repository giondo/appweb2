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

                           def created = openshift.newApp("https://github.com/giondo/appweb2.git","php")

                           // This Selector exposes the same operations you have already seen.
                           // (And many more that you haven't!).
                           echo "new-app created ${created.count()} objects named: ${created.names()}"
                           created.describe()

                           // We can create a Selector from the larger set which only selects
                           // the build config which new-app just created.
                           def bc = created.narrow('bc')

                           // Let's output the build logs to the Jenkins console. bc.logs()
                           // would run `oc logs bc/ruby-hello-world`, but that might only
                           // output a partial log if the build is in progress. Instead, we will
                           // pass '-f' to `oc logs` to follow the build until it terminates.
                           // Arguments to logs get passed directly on to the oc command line.
                           def result = bc.logs('-f')

                           // Many operations, like logs(), return a Result object (even a Selector
                           // is a subclass of Result). A Result object contains detailed information about
                           // what actions, if any, took place to accomplish an operation.
                           echo "The logs operation require ${result.actions.size()} oc interactions"

                           // You can even see exactly what oc command was executed.
                           echo "Logs executed: ${result.actions[0].cmd}"

                           // And even obtain the standard output and standard error of the command.
                           def logsString = result.actions[0].out
                           def logsErr = result.actions[0].err

                           // And if after some processing within your pipeline, if you decide
                           // you need to initiate a new build after the one initiated by
                           // new-app, simply call the `oc start-build` equivalent:
                           def buildSelector = bc.startBuild()
                           buildSelector.logs('-f')

			   created.narrow("svc").expose();
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
