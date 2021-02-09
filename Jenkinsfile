pipeline {
    agent any 
    parameters {
      string(name: 'flowPipelineName', defaultValue: 'RunPipelineRunAndWaitPipeline')
      string(name: 'flowReleaseName', defaultValue: 'TriggerReleaseRunAndWait')
      string(name: 'procedureOutcome', defaultValue: 'success')
      string(name: 'sleepTime', defaultValue: '1')
      string(name: 'flowConfigName', defaultValue: 'electricflow')
      string(name: 'flowPipelineName', defaultValue: 'runProcedureRunAndWait')
      string(name: 'flowProjectName', defaultValue: 'pvNativeJenkinsProject01')
      string(name: 'runAndWaitInterval', defaultValue: '5')
      string(name: 'dependOnCdJobOutcomeCh', defaultValue: 'true')
      string(name: 'type', defaultValue: 'associate')
      string(name: 'flowRuntimeId', defaultValue: '77327893-6ac5-11eb-9c1b-0242ac120002')
    }
    stages {
        stage('Build') { 
            steps {
                echo "Build Step"
            }
        }
        stage('Test'){
            steps {
                echo 'Test step'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy step'
            }
        }
    }
    post {
        always {
            script {
              if (params.type == 'pipeline') {
                cloudBeesFlowRunPipeline addParam: '{"pipeline":{"pipelineName":"$flowPipelineName","parameters":[{"parameterName":"procedureOutcome","parameterValue":"$procedureOutcome"},{"parameterName":"sleepTime","parameterValue":"$sleepTime"}]}}', configuration: "$flowConfigName", pipelineName: "$flowPipelineName", projectName: "$flowProjectName", runAndWaitOption: [checkInterval: "$runAndWaitInterval", dependOnCdJobOutcome: "$dependOnCdJobOutcomeCh"]
              }
              else if (params.type == 'release') {
                cloudBeesFlowTriggerRelease configuration: "$flowConfigName", parameters: "{'release':{'releaseName':'$flowReleaseName','stages':[{'stageName':'Stage 1','stageValue':''}],'pipelineName':'pipeline_$flowReleaseName','parameters':[{'parameterName':'procedureOutcome','parameterValue':'$procedureOutcome'},{'parameterName':'sleepTime','parameterValue':'$sleepTime'}]}}", projectName: "$flowProjectName", releaseName: "$flowReleaseName", runAndWaitOption: [checkInterval: "$runAndWaitInterval", dependOnCdJobOutcome: "$dependOnCdJobOutcomeCh"], startingStage: "Stage 1"
              }
              else if (params.type == 'associate') {
                echo "${flowRuntimeId}"
                cloudBeesFlowAssociateBuildToRelease configuration: "$flowConfigName", flowRuntimeId: "$flowRuntimeId", projectName: "$flowProjectName", releaseName: "$flowReleaseName"                     
              }
            }
        }
    }

}

