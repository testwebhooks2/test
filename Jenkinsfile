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

def isPullRequest = env.CHANGE_ID != null
def isMasterBranch = env.BRANCH_NAME == 'master'
def isCleaningUp = params.REVIEW_APP_CLEANING == 'true'

def autoCancelled = !isPullRequest && !isMasterBranch && !isCleaningUp

node {
  properties([
    parameters([
      string(name: 'REVIEW_APP_CLEANING', defaultValue: 'false'),
      string(name: 'PR_NUMBER', defaultValue: ''),
      string(name: 'PR_STATUS', defaultValue: ''),
    ])
  ])
// 22222222222
  try {
    if (autoCancelled) {
      error('Auto cancelling redundant build')
    }

    stage("Setup Dependencies") {
      checkout scm
      populateGlobalVariables()
    }

    if (isCleaningUp) {
      stage("Clean up review app") {
        echo "Let's clean up this build! PR-${params.PR_NUMBER}, status: ${params.PR_STATUS}"
      }
    }
  } catch(e) {
    if (autoCancelled) {
      currentBuild.result = 'ABORTED'
      return
    } else {
      currentBuild.result = "FAILED"
      throw e
    }
  } finally {
  }
}

