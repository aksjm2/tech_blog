# Create Instance

#### 01. Overview

Instance를 신규 추가 및 setting 하는 과정을 남긴다.

첫 달 무료

#### 02. Work log.

**인스턴스 생성**

![](https://wiki.simplexi.com/download/attachments/1461302195/image2019-7-11_17-31-48.png?version=1&modificationDate=1562834734000&api=v2)

-. 첫 화면에서 Instance location을 변경할 수 있다. → 제일 가까운 서울로 잡혔다.

-. platform은 OS의 종류를 선택한다. Linux / Window == Linux 로 간다.

-. 다양한 App + OS 설치된 내용이 존재한다. 선택해서 사용 가능하나, Os Only, Centos로 설치했다.

![](https://wiki.simplexi.com/download/attachments/1461302195/image2019-7-11_17-34-21.png?version=1&modificationDate=1562834734000&api=v2)

-. SSH key를 관리할 수 있다.

-. 인스턴스 구동에 들어가는 월간 비용이다.

-. 인스턴스를 구분할 수 있는 이름을 입력한다.

-. Tag 기능이 있는데, 이것도 필터링을 위해 사용하는 듯 하다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>SSH Key &#xB4F1;&#xB85D;&#xD558;&#xACE0;, &#xBE44;&#xC6A9;&#xC740; &#xC81C;&#xC77C;
          &#xB0AE;&#xC740;&#xAC70;&#xB85C; &#xC120;&#xD0DD;.</p>
        <p>&#xC778;&#xC2A4;&#xD134;&#xC2A4; &#xBA85; &#xC785;&#xB825;&#xD558;&#xACE0;</p>
        <p>&#xD0DC;&#xADF8;&#xB294; &#xB4F1;&#xB85D;&#xD558;&#xC9C0; &#xC54A;&#xC558;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>-. 마지막으로 Create Instance 클릭하면 뜬다.

![](https://wiki.simplexi.com/download/attachments/1461302195/image2019-7-11_17-36-20.png?version=1&modificationDate=1562834734000&api=v2)

**Network를 선택해 보자.**

![](https://wiki.simplexi.com/download/attachments/1461302195/image2019-7-11_17-36-44.png?version=1&modificationDate=1562834734000&api=v2)

-. 네트워킹 탭에서 5개까지 무료인 공인 IP를 받을 수 있다.

-. create static IP를 선택한다.

![](https://wiki.simplexi.com/download/attachments/1461302195/image2019-7-11_17-37-49.png?version=1&modificationDate=1562834734000&api=v2)

-. 공인 아이피는 해당 region에 등록되는 듯 하다.

-. 이미 생성해 버려서 매핑할 대상이 없다고 나온다. == 신규 생성하면 select box가 나오고, 거기서 생성한 Instance를 골라주면 된다.

-. static ip에 대한 identification. 식별자를 등록하고 create를 클릭하면 끝.

**DB server setting.**

DB에 들어가는 비용이 꽤 된다. 제일 낮은게 15불.

&gt;&gt; 실제로 서비스 할 때, 규모가 커지면 인스턴스를 더 높은 사양으로 가지 않을까 싶다.

추가하는 옵션은 크게 어려움이 없어 보인다.

**접속 방법**

![](https://wiki.simplexi.com/download/attachments/1461302195/image2019-7-11_17-44-10.png?version=1&modificationDate=1562834734000&api=v2)

가운데 떡하니 있는 Connect using SSH 를 클릭하면, 웹 콘솔에 붙어서 접근 가능하다.

왠만하면 Instance 생성에서 등록한 SSH key를 이용하고, putty 등으로 port 변경해서 붙는 것이 좋을 듯 하다. security 설정은 좀 더 찾아봐야겠다.

