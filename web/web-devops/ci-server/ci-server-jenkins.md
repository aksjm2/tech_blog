# CI Server :: Jenkins

#### 01. What is Jenkins?

젠킨스는 소프트웨어의 building, testing, delivering 또는 deploying에 관련된 작업들을 자동화 할 수 있는 오픈소스 자동화 서버입니다.

Jenkins는 네이티브 시스템 패키지, Docker 또는 JRE가 설치된 어떤 기기에서도 동작이 가능합니다.

jeknins 설치시 고려해야되는 사항중에 하나가, jenkins에 사용할 plugin 버전, jre version, jenkins version 세개가 다 맞는 경우로 사용해야한다. 타 시스템 연동시 특히 짜증났던 부분. 몇 번 재설치 할 수 있음.

아무것도 없고 새로 설치해야할 때는 bitnami 패키지에서 설치하면 쉬움.

기존에 DB라던지, Tomcat이던지 jenkins가 동작하는데 필요한 middle ware가 존재한다면 버전 잘 살펴서 war만 포팅하는 것으로 처리해도 됨.

#### 02. Pipeline

**02.01. What is Jenkins Pipeline**

젠킨스의 파이프라인은 지속적 배포를 위한 통합, 구축을 도와주는 플러그인의 집합입니다.

Continuous delivery pipeline\(지속적 배포 파이프라인\)은 version control 시스템\(git, svn, mecurial, cvs...\)부터 고객에게 전달\(deploy to production environment\)되는 프로세스를 정의한 자동화된 표현입니다.

젠킨스의 파이프라인은 복잡한 배포 파이프라인을 간단하게 모델링\(코드\) 할 수 있는 확장가능한 도구를 제공합니다.

젠킨스 파이프라인의 정의는 JenkinsFile이라는 텍스트파일에 작성합니다.

JekninsFile은 source version control repository에 추가하여 파이프라인을 관리합니다.

**02.02. Quick start example**

* Java

```markup
pipeline {
	agent { docker { image 'maven:3.3.3' } }
 	stages {
		steps {
			sh 'mvn --version'
		}
	}
}
```

* Node.js/ Javascript

```markup
pipeline {
	agent { docker { image 'node:6.3' } }
	stages {
		steps {
			sh 'npm --version'
		}
	}
}
```

* Ruby

```markup
pipeline {
	agent { docker { image 'ruby' } }
	stages {
		steps {
			sh 'ruby --version'
		}
	}
}
```

* Python

```markup
pipeline {
	agent { docker { image 'python:3.5.1' } }
	stages {
		steps {
			sh 'python --version'
		}
	}
}
```

* PHP

```markup
pipeline {
	agent { docker { image 'php' } }
	stages {
		steps {
			sh 'php --version'
		}
	}
}
```

* Go

```markup
pipeline {
	agent { docker { image 'golang' } }
	stages {
		stage('build') {
			steps {
				sh 'go version'
			}
		}
	}
}
```

**02.03. Running Multiple Steps**

파이프라인은 여러개의 step들로 만들어진다. 각 step은 빌드, 테스트, 배포가 될 수 있다.

Jenkins의 파이프라인은 여러개의 step을 조립할 수 있도록 도와주며, 이는 자동화 프로세스를 쉽게 모델링 할 수 있도록 한다.

Jenkins의 파이프라인에서 step은 action의 최소 단위로 생각한다.

**02.03.01. Linux, BSD, Mac**

Linux와 BSD, Mac, Unix와 유사한 OS 시스템에서는 sh 명령으로 스크립트를 실행한다.

```markup
pipeline {
	agent any
	stages {
		stage('build') {
			steps {
				sh 'echo "hello world"'
				sh '''
					echo "Multi-line shell steps works too"
					ls -lah
				'''
			}
		}
	}
}
```

**02.03.02. Windows**

Windows에서는 bat 명령을 통해 각각의 batch command를 수행한다.

```markup
pipeline {
	agent any
	stages {
		stage('build') {
			steps{
				bat 'set'
			}
		}
	}
}
```

  
**02.03.03. Timeouts, retries and more**

timeout, retry은 다른 step을 wrapping 할 수 있는 step 이다.

```markup
pipeline {
	agent any
	stages {
		stage('deploy') {
			steps {
				retry(3) {
					sh './flakey-deploy.sh'
				}


				timeout(time:3, unit: 'MINUTES') {
					sh './health-check.sh'
				}
			}
		}
	}
}
```

Deploy stage의 flakey-deploy.sh는 3회 재수행 한다. health-check.sh 스크립트가 수행되기를 3분까지 기다린다.

health-check.sh 스크립트가 3분내에 완료되지 않으면, Deploy stage는 fail 상태로 표시된다.

timeout과 retry를 nested 형태로 작성할 수 도 있다.

**02.03.04 Finishing up**

파이프라인이 수행을 마쳤을때, step들을 clean up 하는 작업이나 다른 pipeline에 trigger를 보내는 등의 작업이 필요할 수 있다. 모든 작업이 끝난 후에는 post 구역에 작성하여 수행하도록 한다.

```markup
pipeline {
	agent any
	stages {
		stage('Test') {
			steps {
				sh 'echo "Fail!"; exit 1'
			}
		}
	}


	post {
		always {
			echo 'This will always run // 항상'
		}
		success {
			echo 'This will run only if successful // 성공'
		}
		failure {
			echo 'This will run only if failed // 실패'
		}
		unstable {
			echo 'This will run only if the run was marked as unstable // 불안정 상태'
		}
		changed {
			echo 'This will run only if the state of the Pipeline has changed' //파이프라인 결과가 변경되었을 때
			echo 'For example, if the Pipeline was previously failing but is now successful'
		}
	}
}

```

