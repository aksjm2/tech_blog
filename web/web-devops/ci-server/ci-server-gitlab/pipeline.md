---
description: 여러 환경에 배포하기위한 파이프라인 작업 기록.
---

# GitLab :: Pipeline

#### 01. Overview

운영 gitlab 환경에서 테스트 중 authentication error로 보이는 경우가 발생하였다. 방화벽이 의심되어 해당내용 검증을위해 방화벽이 뚫려있는 로컬에서 작업해본다.

추가로, pipeline 구성도 코드를 수정하여 확인한다.

#### 02. Work Log

```yaml
stages:
  - common_jobs
  - openstack_deploy
  - aws_deploy

validation:
  stage: common_jobs
  script: echo "common jobs - validtaion check"

001-image-build:
  stage: openstack_deploy
  script: echo "001-image-build"
  when: on_success
002-source-process:
  stage: openstack_deploy
  script: echo "002-source-process"
  when: on_success
003-container-deploy:
  stage: openstack_deploy
  script: echo "003-container-deploy"
  when: on_success

aws-001-image-build:
  stage: aws_deploy
  script: echo "aws-001-image-build"
  when: manual
aws-002-source-process:
  stage: aws_deploy
  script: echo "aws-002-source-process"
  when: manual
aws-003-container-deploy:
  stage: aws_deploy
  script: echo "aws-003-container-deploy"
  when: manual
```

![](https://wiki.simplexi.com/download/attachments/1442551871/image2019-6-19_15-26-25.png?version=1&modificationDate=1560925586000&api=v2)

보이는 순서는 보장되나\(JOB 이름으로 정렬..\)/ 실행되는 순서가 같은 stage에서는 병렬로 처리되기 때문에 이와같은 구조로 pipeline을 작성하면 안된다.

&gt;&gt; stage를 묶을수 있는 무언가가 있는지를 확인해야 한다.

Envrionment name setting으로 수정

```yaml
stages:
  - common_jobs
  - image-build
  - source-process
  - container-deploy

validation:
  stage: common_jobs
  script: echo "common jobs - validtaion check"
  tags :
    - dev

001-image-build:
  stage: image-build
  environment:
    name: IDC
  script: echo "001-image-build"
  tags :
    - dev
  when: on_success
002-source-process:
  stage: source-process
  environment:
    name: IDC
  script: echo "002-source-process"
  tags :
    - dev
  when: on_success
003-container-deploy:
  stage: container-deploy
  environment:
    name: IDC
  script: echo "003-container-deploy"
  tags :
    - dev
  when: on_success

aws-001-image-build:
  stage: image-build
  environment:
    name: AWS
  script: echo "aws-001-image-build"
  tags :
    - dev
  when: manual
aws-002-source-process:
  stage: source-process
  environment:
    name: AWS
  script: echo "aws-002-source-process"
  tags :
    - dev
  when: manual
aws-003-container-deploy:
  stage: container-deploy
  environment:
    name: AWS
  script: echo "aws-003-container-deploy"
  tags :
    - dev
  when: manual
```

![](https://wiki.simplexi.com/download/attachments/1442551871/image2019-6-19_15-42-10.png?version=1&modificationDate=1560926531000&api=v2)

이런식으로 작성해야, 각 JOB별 진행 순서를 확정할 수 있다.

뒤쪽으로 늘어놓은 AWS 파이프라인 JOB에 대해 stage 통합과, environment:name 태그를 이용한 구분, JOB name을 이용한 구분을 통해 둘로 나누어야 한다.

