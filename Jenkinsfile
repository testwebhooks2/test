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

// class MatcherResult {
//   Boolean isSuccess;
//   String pullRequestId;
//   String pullRequestStatus;

//   def setPullRequestStatus(status) {
//     this.pullRequestStatus = status;
//   }

//   MatcherResult(matcher = null) {
//     if (matcher == null) {
//       this.isSuccess = false;
//     } else {
//       this.isSuccess = true;
//       this.pullRequestId = matcher.group(1);

//       if (matcher.groupCount() == 2) {
//         this.pullRequestStatus = matcher.group(2);
//       }
//     }
//   }
// }

// def detectPullRequest(build) {
//   def cause = build.rawBuild.getCauses().last().getShortDescription()

//   def prReplayedEventMatcher = (cause =~ /Replayed \#(\d+)/)
//   def prMergedEventMatcher = (cause =~ /Push event to branch (\w+)/)
//   def prChangedEventMatcher = (cause =~ /Pull request \#(\d+)\s(\w+)/)

//   if (prReplayedEventMatcher.matches()) {
//     def pbMatcherResult = new MatcherResult(prReplayedEventMatcher);
//     pbMatcherResult.setPullRequestStatus("replayed");

//     return pbMatcherResult;
//   } else if (prMergedEventMatcher.matches()) {
//     def pbMatcherResult = detectPullRequest(build.getPreviousBuild());

//     if (pbMatcherResult.isSuccess) {
//       pbMatcherResult.setPullRequestStatus("merged");
//     }

//     return pbMatcherResult;
//   } else if (prChangedEventMatcher.matches()) {
//     return new MatcherResult(prChangedEventMatcher);
//   }

//   return new MatcherResult(null);
// }

class MatcherResult {
  Boolean isSuccess;
  String pullRequestId;
  String pullRequestStatus;

  def setPullRequestStatus(status) {
    this.pullRequestStatus = status;
  }

  def setPullRequestId(id) {
    this.pullRequestId = id;
  }

  MatcherResult(matcher = null) {
    if (matcher == null) {
      this.isSuccess = false;
    } else {
      this.isSuccess = true;
      this.pullRequestId = matcher.group(1);
    }
  }
}

def detectPullRequest(build) {
  def cause = build.rawBuild.getCauses().last().getShortDescription()

  def prReplayedEventMatcher = (cause =~ /Replayed \#(\d+)/)
  def prMergedEventMatcher = (cause =~ /Push event to branch (\w+)/)
  def prChangedEventMatcher = (cause =~ /Pull request \#(\d+)\s(\w+)/)

  def matcherResult =  new MatcherResult(null);

  if (prChangedEventMatcher.matches()) {
    matcherResult = new MatcherResult(prChangedEventMatcher);
    matcherResult.setPullRequestStatus(prChangedEventMatcher.group(2));
  }

  // if (prReplayedEventMatcher.matches()) {
  //   matcherResult = new MatcherResult(prReplayedEventMatcher);
  //   matcherResult.setPullRequestStatus("replayed");
  // } else if (prMergedEventMatcher.matches()) {
  //   def lastCommitMatcher = parseLastCommitMessage();

  //   if (lastCommitMatcher.matches()) {
  //     matcherResult.setPullRequestId(lastCommitMatcher.group(1));
  //     matcherResult.setPullRequestStatus("merged");
  //   }
  // } else if (prChangedEventMatcher.matches()) {
  //   matcherResult = new MatcherResult(prChangedEventMatcher);
  //   matcherResult.setPullRequestStatus(prChangedEventMatcher.group(2));
  // }

  return matcherResult;
}

def parseLastCommitMessage() {
  def lastCommitMsg = sh (script: "git log -1 | grep git log -1 | grep -o --regexp='Merge pull request #\\d*'", returnStdout: true);
  echo "lastCommitMsg = ${lastCommitMsg}"
  def matcher = (lastCommitMsg =~ /Merge pull request \#(\d+)/);
  return matcher;
}

def getBuildDesc(build) {
  return build.rawBuild.getDescription()
}

// def desc11 = getBuildDesc(currentBuild)
def causes = currentBuild.rawBuild.getCauses().collect { it.getShortDescription() }.join(' | ')

node {
  try {
    stage("Setup Dependencies") {
      
      checkout scm
      
      populateGlobalVariables()
        
      echo "getLastCommitMessage: ${message}"

      def prDetection = detectPullRequest(currentBuild);

      echo "all causes: ${causes}"

      if (prDetection.isSuccess) {
        echo "pull request id = ${prDetection.pullRequestId}";
        echo "pull request status = ${prDetection.pullRequestStatus}";

        // def bbb = getBuildDesc(currentBuild)
        echo "all causes: ${causes}"
        // echo "prev build desc: ${desc11}"
      } else {
        echo "this build is not pull request related";
      }
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
