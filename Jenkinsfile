@Library('freelance') _

pipeline {
   agent any
   
    tools { 
        maven 'freelance' 
    }
	
   stages {
        stage('library groovy') {
			steps {
				yaml("config.yml")	
		}				
	}

        stage('build') {
			steps {
				sh """

					cd $env.buildProjectFolder
					
					$env.buildCommand
				"""
		}				
	}

        stage('database') {
			steps {
				sh """
					cd $env.databaseProjectFolder
					$env.databaseCommand
				"""
		}				
	}

        stage('deploy') {
			steps {
				sh """
					cd $env.buildProjectFolder
					$env.deployCommand
				"""
		}				
	}

        stage('test') {
			parallel {
				stage('Performance test'){
					steps {
						sh """
							cd $env.testFolder
							$env.performanceTestCommand
						"""
					}
				}
				
				stage('Regression test'){
					steps {
						sh """
							cd $env.testFolder
							$env.regressionTestCommand
						"""
					}
				}
				
				stage('Integration test'){
					steps {
						sh """
							cd $env.testFolder
							$env.integrationTestCommand
						"""
				}
			}
		}
	}
}


   post {
		always {
			cleanWs ()
		}	
	}
}
