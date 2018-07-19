pipeline{

	agent {label "master"}

	options{
		disableConcurrentBuilds()
		buildDiscarder(logRotator(numToKeepStr: '200'))
		timeout(time:5400, unit:'SECONDS')
	}

	stages{

		stage("Clone0"){
                steps{
                    deleteDir()
                    script{
                        REPOSITORY_PATH = "https://github.com/yuzp1996/maven-test.git"
                        def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: "master"]],
                        extensions: [[
                            $class: 'SubmoduleOption',
                            recursiveSubmodules: true,
                            reference: '',
                        ]],
                        userRemoteConfigs: [[
                            credentialsId: "yuzhipenggithub", url: 'https://github.com/yuzp1996/maven-test.git'
                        ]]
                        ])

                        GIT_COMMIT = scmVars.GIT_COMMIT
                        GIT_BRANCH = scmVars.GIT_LOCAL_BRANCH
                        env.CODE_COMMIT = GIT_COMMIT
                        env.GIT_COMMIT = GIT_COMMIT
                        env.CODE_BRANCH = GIT_BRANCH
                        env.GIT_BRANCH = GIT_BRANCH

                    }
                    stash "CloneCode"
                }
            }



        stage("CodeScan"){
            agent{
                label "java"
            }
            options{
                timeout(time:3600, unit:'SECONDS')
            }
            steps{
                unstash "CloneCode"
                script{
                    dir("./"){
                        sh "ls"
                        sh "cd example"
                        sh "mvn clean install"
                    }
            }
        }
    }


	}

	post{
		always{
			echo "clean up workspace"
			deleteDir()
		}
	}
}
