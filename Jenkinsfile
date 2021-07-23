#!groovy

properties( 
	parameters([
		string(defaultValue: '', description: 'param1', name: 'param1')
	])
])


node("") {

	stage("stage1") {
		println params.param1
	}

	stage("stage2") {

		 println "stage2"
	}

	stage("stage3") {
		
		 println "stage3"
	}

}
