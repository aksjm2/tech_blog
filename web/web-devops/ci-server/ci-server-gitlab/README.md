# CI Server :: GitLab

이후 파이프라인 작업에 참고할 내용을 정리한다.

참고 : [Multi-project pipeline](https://zerotoprod.com/posts/gitlab-ci-advanced/)

현재 소스 배포 파이프라인을 작성해 두었으나,

전체 배포 프로세스 상에서 극히 일부분에 지나지 않는다.

별도의 배포 프로세스 관리 파이프라인\(upstream\)이 존재하고,

SQL 배포, 선적용 등 특정 프로세스가 존재하는 파이프라인이 개별로 존재해야 한다.

해당 내용을 dependency를 통해, 연결하고 이를 전체 관리 파이프라인에서 확인할 수 있도록 하는것이 맞지 않을까 싶다.

