#!groovy

properties([gitLabConnection(base.gitlabConnectionName),
	parameters([
		string(defaultValue: '', description: 'Test param 1', name: 'Testparam1'),
		string(defaultValue: '', description: 'Test param 2', name: 'Testparam2')
	])
])


node("${base.dockerHeliumNode}") {

	stage("checkout") {
		checkoutClean()
	}

	stage("create iso list") {

		isoList = []
		
		if(params.ProjectName == "ct-recon-deploy") {
			isoList = ctReconDeployList	
		}
		else if(params.ProjectName == "ct-dlir-deploy") {
			isoList = ctDlirList
		}
		else if(params.ProjectName == "ct-maxfov-deploy") {
			isoList = ctMaxfovList
		}
		else if(params.ProjectName == "ct-smartmar-deploy") {
			isoList = ctSmartMarList
		}
		else if(params.ProjectName == "ct-asirv-deploy") {
			isoList = ctAsirvList
		}
		else if(params.ProjectName == "ct-common-deploy") {
			isoList = ctCommonList
		}
		else {
			error("Invalid ProjectName")
		}
	}

	stage("update iso version") {
		
		branch = "Updated-${params.ProjectName}-${params.Version}"
		msg = "Updated iso version to ${params.Version}"

		sshagent([base.gitlabCredentials]) {
			sh	"""
				git config user.email 'service.ct-haifa-artifactory@ge.com'
				git config user.name 'Haifa CT Service Account'
				git checkout -b ${branch}
				"""
			for(iso in isoList) {
				sh "scripts/update_version.py -i ${iso} -v ${params.Version}"
			}
			sh "git commit -am '${msg}'"
		}
		

	}

	stage("create merge request") {

		target = "master"
		title = "Updated iso version of ${params.ProjectName} ISOs"
		description = " Dear team, the iso version has been updated for ISOs under ${params.ProjectName} project. Please review the changes and approve the merge request."
		sshagent([base.gitlabCredentials]) {
			sh "git push -o merge_request.create -o merge_request.target='${target}' -o merge_request.remove_source_branch -o merge_request.title='${title}' -o merge_request.description='${description}' origin ${branch}"
		}
		
	}
	
}
