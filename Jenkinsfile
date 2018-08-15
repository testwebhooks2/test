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
      string(name: 'REVIEW_APP_CLEANING', defaultValue: 'false'),
      string(name: 'PR_NUMBER', defaultValue: ''),
      string(name: 'PR_STATUS', defaultValue: ''),
    ])
  ])

  try {
    stage("Setup Dependencies") {
      checkout scm
      populateGlobalVariables()

      if (params.REVIEW_APP_CLEANING == 'true') {
        echo "PR NUMBER: ${params.PR_NUMBER}";
        echo "PR STATUS: ${params.PR_STATUS}";
      } else {
        echo "NOT FROM SERVICE";
      }
    }
  } catch(e) {
    currentBuild.result = "FAILED"
    throw e
  } finally {
  }
}

