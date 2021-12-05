def pipelin_var = "test_var"
properties([pipelineTriggers([githubPush()])])

pipeline{
    agent {label 'slave--1'}
    parameters {
        choice (name: 'git_branch', choices: ["master", "dev", "UAT"])
    }
    stages{
        stage('clone'){
            steps{
                git 'https://github.com/dineshmadivada55/applogin.git'
            }
        }
        stage("build"){
            steps{
                sh "mvn clean install"
            }
        }
        stage("test"){
            steps{
                script{
                    try {
                        println "this is for test"
                        println "${pipelin_var}"
                    }
                    catch(err) {
                        println "tere is some error"
                     }
                }
            }
        }
        stage("publish"){
            steps{
                script{
                    rtUpload(
                        serverId: 'my-jfrog',
                        spec: '''{
                            "files":[
                                {
                                "pattern": "target/*.war",
                                "target": "applogin"
                                }
                            ]
                        }''',
                        buildName: 'applogin',
                        buildNumber: "${BUILD_NUMBER}"
                    )
                }

            }
        }
        stage("deploy"){
            steps{
                script{
                    if ( "${git_branch}" != "master") {
                        println "this is in master"
                    }
                    else {
                        "println this is wrong"
                    }                    
                    
                }
            }
        }
    }
}
