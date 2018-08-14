def getGitAuthor = {
    def commit = sh(returnStdout: true, script: 'git rev-parse HEAD')
    author = sh(returnStdout: true, script: "git --no-pager show -s --format='%an' ${commit}").trim()
}

def getLastCommitMessage = {
    message = sh(returnStdout: true, script: 'git log -1 --pretty=%B').trim()
}

def populateGlobalVariables = {
    getLastCommitMessage()
    getGitAuthor()
}

node {
  properties([
    parameters([
      string(defaultValue: '', description: '', name: 'TEST_VAR')
    ])
  ])

  try {
    stage("Setup Dependencies") {
      checkout scm
      populateGlobalVariables()

      echo(env.getEnvironment().collect({environmentVariable ->  "${environmentVariable.key} = ${environmentVariable.value}"}).join("\n"))

      // echo "test var: ${test_var}";
      // echo "pr number: ${pr_number}";

      // echo "env test var: ${env.test_var}";
      // echo "env pr number: ${env.pr_number}";

      echo "params test var: ${params.TEST_VAR}";
      // echo "params pr number: ${params.pr_number}";
    }
  } catch(e) {
    currentBuild.result = "FAILED"
    throw e
  } finally {
  }
}

