---
description: Api Gateway에 대한 생각 정리.
---

# API Gateway

#### 01. Overview

시스템\(micro service architecture에서 더 많이 사용.\)간 Interface로 API를 많이 사용하는 듯하다.

DB to DB, EAI 등 다른 방법으로 중앙화 하여 처리하는 경우를 많이 보았는데..

API로만 통신하는 상황에서 서로 다른 언어, 서로 다른 방식의 Input, HTTP Method 등을 관리하는데 API gateway의 역할과, 필요성을 정리한다.

#### 02. Definition

API gateway란, API\(application programming interface\)의 앞단에서 다른 여러 Micro service 그룹에 대한 통합된 입력 지점?\(entry point\)를 제공하는 것이다.

Protocol 변환을 API gateway에서 제어하기 때문에, 더 다양한 Micro service, 세분화된 API에서 유용하게 사용한다.

API gateway 사용의 주된 장점은 개발자로 하여금 application을 여러 layer로 구성하여 encapsulation 할 수 있다는 것이다.

일례로, API gateway로 하나의 요청이 들어왔을 때, API gateway는 여러 back-end service를 호출하여 받은 데이터를 재 가공하여 전달할 수 있다.

API gateway를 update 하는 부분을 가볍게 하는것이 중요하다.

새로운 API의 추가/ 삭제/ 수정이 빈번하게 일어나기 때문.

Micro service를 외부에 노출시키기 위해, 유명한 API gateway는 아래와 같은 기능을 포함한다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <ul>
          <li>autnentication : &#xC778;&#xC99D;</li>
          <li>security policy enforcement : &#xBCF4;&#xC548;&#xC815;&#xCC45; &#xAC15;&#xD654;</li>
          <li>load balancing : &#xB85C;&#xB4DC; &#xBC38;&#xB7F0;&#xC2F1;</li>
          <li>cache management : &#xCE90;&#xC26C; &#xAD00;&#xB9AC;</li>
          <li>dependency resolution : &#xC758;&#xC874;&#xC131; &#xD574;&#xACB0;</li>
          <li>contract and service level agreement(SLA) maangement : &#xACC4;&#xC57D;&#xACFC;
            SLA &#xAD00;&#xB9AC;.</li>
        </ul>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>참고 : [넷플릭스 API gateway](https://medium.com/netflix-techblog/optimizing-the-netflix-api-5c9ac715cf19), [Java api gateway example](https://github.com/cer/event-sourcing-examples)

