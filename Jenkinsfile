#!groovy
@Library("jenkinsLibary") _
def tools = new org.devops.tools()
hello()
String workSpace = "/opt/jenkins/workSpace"
pipeline {
	agent { node { label "master"
					//customWorkspace("${workSpace}")
		 } 
	}
	// 初始化参数
	// name:定义参数名
	// defaultValue:输入默认值
	// description:参数的描述信息
	parameters {
		string(name: "DEPLOY_ENV",defaultValue: "string",description: "")
	}
	options {
		timestamps() //日志
		skipDefaultCheckout() //删除隐式checkout scm语句
		disableConcurrentBuilds() //禁止并行 
		//timeout(time: 1,util: 'HOURS') //设置流水线超时一小时
	}
	stages {
		stage("01") {
			failFast true
			parallel {
				stage("getCode"){
					// 判断是否符合要求，不符合则跳过步骤
					steps{
						timeout(time: 1,unit:"HOURS"){
							script{
								tools.PrintMsg("下载源码","blue")
								tools.PrintMsg("$param","green1")
								checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '4a1ba6aa-49c9-4c48-9d40-fe9c3de8a9f2', url: 'http://10.24.24.17:8888/root/uyw-java.git']]])
							}
						}
					}
				}
				stage("build"){
					steps{
						timeout(time: 1,unit:"HOURS"){
							script{
								tools.PrintMsg("源码构建","blue")
								tools.PrintMsg("env.CC","green1")
								mvnHome = tool "mvn"
								println(mvnHome)
								println(WORKSPACE)
								sh """
								${mvnHome}/bin/mvn --version
								${mvnHome}/bin/mvn clean install
								"""
								tools.PrintMsg("this is my libary","red")
							}
						}
					}
				}
			}
		}
	}
	post {
		always {
			echo "无论流水线执行成功与否都会运行"
		}
		changed {
			echo "与上一个流水线结构不同时才会运行，例如上一次成功本次失败则会运行"
		}
		failure {
			echo "只有当流水线为failure状态时才会运行"
		}
		success {
			echo "流水线执行成功之后才会运行"
		}
		unstable {
			echo "只有当流水线为unstable状态时才会运行，例如测试失败"
		}
		aborted {
			echo "只有当流水线为aborted状态时才会运行，例如手动取消"
		}
	}
}
