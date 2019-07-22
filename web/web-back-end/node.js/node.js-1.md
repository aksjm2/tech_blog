# Node.js :: 설치

#### 00. Latest Version Information

[최신\_release\_정보](https://github.com/nodejs/Release#release-schedule)

#### 01. Ubuntu/ Mac

01.01. source 컴파일 방법. → 소스를 컴파일해서 바이너리로 만든다.

* wget [https://nodejs.org/dist/v4.5.0/node-v4.5.0.tar.gz](https://nodejs.org/dist/v4.5.0/node-v4.5.0.tar.gz)
* tar zxvf node-v4.5.0.tar.gz
* cd node-v4.5.0
* sudo ./configure
* sudo make
* sudo make install

01.02. apt-get\(package 설치방법\)

* apt-get install nodejs
* apt-get install npm

01.03. 버전을 지정하여 설치하는 방법.

* curl -sL [https://deb.nodesource.com/setup\_10.x](https://deb.nodesource.com/setup_10.x) \| sudo -E bash -
* sudo apt-get install -y nodejs

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>RHEL &#xACC4;&#xC5F4;&#xC758; &#xACBD;&#xC6B0;.</p>
        <p>apt-get &#xB300;&#xC2E0; yum&#xC774;&#xB098;, rpm&#xC73C;&#xB85C; &#xC124;&#xCE58;&#xD558;&#xBA74;
          &#xB428;.</p>
        <p>source &#xCEF4;&#xD30C;&#xC77C;&#xC758; &#xACBD;&#xC6B0; &#xBA85;&#xB839;&#xC5B4;
          &#xB3D9;&#xC77C;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 02. Windows

* nodejs 홈페이지 접속, 윈도우 버전에 맞게 설치파일을 다운받는다. \([http://nodejs.org/en/download/](http://nodejs.org/en/download/)\)
* 설치 진행\(Next Next... Next..\)
* 설치이후, PowerShell이나 CMD에서 다음 명령어를 통해 버전을 확인한다. node -v
* 노드가 정상적으로 설치되었다면 패키지 관리 NPM의 버전도 확인할 수 있다. npm -v

