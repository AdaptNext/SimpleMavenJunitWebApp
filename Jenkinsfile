pipeline {
  agent any
	  //定义mvn环境
    def mvnHome = tool 'maven3'
    env.PATH = "${mvnHome}/bin:${env.PATH}"
  stages {
    stage('Code') {
      steps {
        git 'https://github.com/xbsoft/SimpleMavenJunitWebApp'
      }
    }

    stage('mvn test'){
        //mvn 测试
    	script {
		if(isUnix() == true) {
        		sh "mvn test"
		}else {
			bat "mvn test"
			}
		}
    }

    stage('mvn build'){
        //mvn构建
	script {
		if(isUnix() == true) {
        sh "mvn clean install -Dmaven.test.skip=true"
		}else {
        bat "mvn clean install -Dmaven.test.skip=true"
			}
		}
    }

    stage('deploy'){
        //执行部署脚本
		script {
		if(isUnix() == true) {
        echo "deploy ......" 
		}else {
        echo "deploy ......" 
			}
		}
    }
	  
    stage('Build and Test') {
      parallel {
        stage('Build') {
          steps {
            readFile 'pom.xml'
            script {
					if(isUnix() == true) {
            sh 'mvn install -DskipTests'
					}else {
            bat 'mvn install -DskipTests'
					}
				}
          }
        }
        stage('UnitTest') {
          steps {
            script {
					if(isUnix() == true) {
            sh 'mvn package'
					}else {
            bat 'mvn package'
					}
				}
          }
        }
      }
    }
    stage('Check') {
      parallel {
        stage('Check') {
          steps {
            script {
					if(isUnix() == true) {
            sh 'mvn -e -B sonar:sonar -Dsonar.host.url=http://106.14.68.164:9000 -Dsonar.sources=src\\main -Dsonar.scm.provider=git -Dsonar.java.binaries=target && exit %%ERRORLEVEL%%'
					}else {
            bat 'mvn -e -B sonar:sonar -Dsonar.host.url=http://106.14.68.164:9000 -Dsonar.sources=src\\main -Dsonar.scm.provider=git -Dsonar.java.binaries=target && exit %%ERRORLEVEL%%'
					}
				}
          }
        }
        stage('checkx') {
          steps {
            script {
					if(isUnix() == true) {
                     sh 'ls -l'
         	}else {
                     bat 'ls -l'
         }
				}
          }
        }
      }
    }
    stage('NexusUpload') {
      steps {
        sleep 10
        build 'NexusUpload'
      }
    }
    stage('Selenium') {
      steps {
        build 'SeleniumDemo'
      }
    }
    stage('Jmeter') {
      steps {
        build 'JMeterDemo'
      }
    }
  }
  post {
    always {
      junit '**/reports/junit/*.xml'

    }

  }
}
