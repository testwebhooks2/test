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
  try {
    stage("Setup Dependencies") {

      
      checkout scm
      
      populateGlobalVariables()

      echo "test var: ${params.test_var}";
      echo "pr number: ${params.pr_number}";

    }
  } catch(e) {
    currentBuild.result = "FAILED"
    throw e
  } finally {
  }
}

// 1233321
// 1233321
// 1233321
// 1233321
// 1233321
// 1233321
// 1233321
// 1233321
// 1233321
// 1233321
