#!groovy

properties([gitLabConnection("CON"),
	parameters([
		string(defaultValue: '', description: 'Test param 1', name: 'Testparam1'),
		string(defaultValue: '', description: 'Test param 2', name: 'Testparam2')
	])
])


node("basenode") {

	stage("stage1") {
		println "stage1"
	}

	stage("stage2") {

		 println "stage2"
	}

	stage("stage3") {
		
		 println "stage3"
	}

}
