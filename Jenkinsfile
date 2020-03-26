#!groovy

@Libary('jenkinslib')

def tools = new org.devops.tools()

String workspace = "/opt/jenkins/workspace"

//Pipeline
pipeline {
    agent {
        node {
            label "master"  //指定运行节点标签或者名称
            customWorkspace "${workspace}"  //指定运行工作目录（可选）
        }
    }
    
    parameters { string(name: "DEPLOY_ENV", defaultValue: 'staging', description: '')}

    options {
        timestamps()    //日志时间戳
        skipDefaultCheckout()   //删除隐藏式checkout scm语句
        disableConcurrentBuilds()   //禁止并行
        timeout(time: 1, unit: 'HOURS') //流水线超时设置1h
    }

    stages {
        
        //下载代码
        stage("GetCode"){   //阶段名称
            when {
            environment name: 'test', value: 'Arthur_Liu'
            }
            steps{
                timeout(time:5, unit:"MINUTES"){    //步骤超时时间
                    script{ //填写运行代码
                        println('获取代码')
                        println("$test")
                        
                        input id: 'Selection', message: '是否继续执行？', ok: '继续吧！', parameters: [choice(choices: ['a', 'b'], description: '', name: 'test1')], submitter: 'liuyz,admin'
                        
                    }
                }
            }
        }

        stage("01"){
            failFast true
            parallel {
                //构建
                stage("Build"){
                    steps{
                        timeout(time:20, unit:"MINUTES"){
                            script{
                                println('应用打包')
                                
                                mvnHome = tool "m2"
                                println(mvnHome)
                                
                                sh "${mvnHome}/bin/mvn --version"
                                
                            }
                        }
                    }
                }

                //代码扫描
                stage("CodeScan"){
                    steps{
                        timeout(time:30, unit:"MINUTES"){
                            script{
                                println('代码扫描')

                                tools.printMes("This is my Libary!")

                            }
                        }
                    }
                }
            }
        }
    }

    //构建后操作
    post {
        always {
            script{
                println("always")
            }
        }

        success {
            script{
                currentBuild.description = "\n 构建成功！"
            }
        }

        failure {
            script{
                currentBuild.description += "\n 构建失败！"
            }
        }

        aborted {
            script{
                currentBuild.description += "\n 构建取消！"
            }
        }
    }
}
