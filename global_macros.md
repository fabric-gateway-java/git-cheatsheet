# Global Macros

## SCM

**gerrit-trigger-scm**

  - This macro is used to get the GERRIT_REFSPEC on every freestyle verify job. Below are the required and option parameters to pass on to this macro. This macro is used to get the GERRIT_REFSPEC of the patchset on top of the parent commit of the patchset.
  
     **Required Parameters:**
     
     base-dir: $BASE_DIR (gopath/src/github.com/hyperledger/$PROJECT) or specify the directory path to clone the repository.
     
     **Optional Parameters:**
     
     GERRIT_BRANCH - Gerrit_branch is a jenkins environment variable contains the branch name to build against
     GERRIT_REFSPEC - RefSpec number ex: refs/changes/32/30032/1
     
 **gerrit-trigger-scm-merge**
 
   - This macro is used to get the GERRIT_REFSPEC on every freestyle merge job. Below are the required and optional parameters to pass on to this macro. This macro used to get the latest commit from the repository instead of GERRIT_REFSPEC of the patchset.
  
     **Required Parameters:**
     
     base-dir: $BASE_DIR (gopath/src/github.com/hyperledger/$PROJECT) or specify the directory path to clone the repository.
     
     **Optional Parameters:**
     
     GERRIT_BRANCH - Gerrit_branch is a jenkins environment variable contains the branch name to build against
     GERRIT_REFSPEC - RefSpec number ex: refs/changes/32/30032/1 (default: blank)
     
     when manually building the job replace the GERRIT_REFSPEC parameter with the Gerrit patchset reference number of the patch         
     you would like to test and GERRIT_BRANCH with the branch you would like perform tests on.
 
 ## Wrappers
 
 **build-timeout**
 
   - This wrapper is used to set the build timeout in freestyle jobs.
     ```
     wrappers:
       - build-timeout:
           timeout: '{build_timeout}'
      ```
     See example here https://github.com/hyperledger/ci-management/blob/master/jjb/fabric-sdk-py/fabric-sdk-py-jobs.yaml#L46-L47
    
     **Required Parameters:**
    
     - timeout: Set timeout in mins from job configuration. ex: build_timeout: 50
       
     Pass below value from job configuration. See example here https://github.com/hyperledger/ci-management/blob/52addb694a114dd2cb74e3690831934e4b9251fa/jjb/fabric-sdk-go/fabric-sdk-go-jobs.yaml#L20
   
 **hyperledger-infra-wrappers**
 
   - This wrapper is required for all jobs as it configures the wrappers needed by all Hyperledger infra. This sets the `timestamps`, `mask-passwords`, `ansicolor`, `openstack: single-use: true` and set `ssh-agent-credentials user to hyperledger-jobbuilder`
   
      ```
      wrappers:
        - hyperledger-infra-wrappers
      ```
      
## Triggers
      
 **gerrit-trigger-patch-submitted**
 
   - This trigger is to trigger the Jenkins jobs when a patchset is created.
   
      This macro triggers a jenkins job when a change-merged-event is triggered
      It won't trigger the job when a commit message is updated in the gerrit patchset.
      Triggers the jobs when a comment is posted in the gerrit patchset. Comments are specified in the job configuration.
      It triggers the job on a branch pattren specified in the job configuration.
      
      ```
      triggers:
        - gerrit-trigger-patch-submitted:
            name: '{project}'
            branch: 'master'
            trigger-comment1: 'reverify-x$'
            trigger-comment2: 'reverify$'
       ```
       
       **Required Parameters:**
       
       project: Provide project name from the job configuration.
       
 **gerrit-trigger-patch-merged**
 
   - This macro is to trigger the Jenkins jobs when a patchset is merged.
       
       **Required Parameters:**
       
       name: Project Name ex: fabric or $PROJECT environment variable
       
       branch: Provide the branch name. Provide blank value if you would like trigger jon on any branch.
       
       Sample job configuration. See this example https://github.com/hyperledger/ci-management/blob/2967fce27cb941156b982fe43a59547fd707c8c5/jjb/fabric-sdk-java/fabric-sdk-java-jobs.yaml#L118-L121
       ```
       triggers:
         - gerrit-trigger-patch-merged:
             name: '{project}'
             branch: ''
       
 **gerrit-comment-trigger**
 
   - This macro is to trigger builds based on the comment provided in the gerrit patchset. Meaning when you use this macro in a job configuration, this won't trigger a job until you post a comment in gerrit patchset to trigger a job. Also, this macro skips the gerrit vote for any build result.
   
         triggers:
           - gerrit-comment-trigger:
               name: '{project}'
               branch: ''
               trigger-comment: 'Run UnitTest'
               
      **Required Parameters:**
      
      name: Project Name ex: fabric
      
      branch: Provide the branch name. Provide blank value if you would like trigger jon on any branch.
      
      trigger-comment: Provide the comment for which you would like to trigger the job.

**gerrit-trigger-tag**

  - This macro is to trigger the jenkins job when a tag is created in the gerrit repository.
  
        triggers:
          - gerrit-trigger-tag:
              name: '{project}'
              branch: 'master'
              
    **Required Parameters:**
      
      name: Project Name ex: fabric
      
      branch: Provide the branch name. Provide blank value if you would like trigger jon on any branch.
      
## Publishers:

**archive-artifacts**
 
  - This macro is to publish the artifacts provided in the <artifacts> on the jenkins console.
  
        publishers:
          - archive-artifacts:
              artifacts: '.tox/**/*.log'
              
    The above macro archives the `.log` files and display on the jenkins console.
     
**fabric-email-notification**
  
  -  This macro is to publish the build notifications through email to the list of email-ids sp
  
         publishers:
           - fabric-email-notification:
               email-ids: 'sambhavdutt@gmail.com, vijaypunugubati@gmail.com'
               output_format: ''
               developer-email: 'developers'
               
     **Required Parameters:**
      
      email-ids: Provide the email-ids list here to send the email notification to.
      output_format: provide the log file type
      
