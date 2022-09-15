# dockerode [docker-engine-api npm package]

## 1. dockerode
* docker-engine-api를 이용할 수 있는 npm package

## 2. installation
`$ npm install dockerode`

## 3. usage

### Instantiate Code

```javascript
var Docker = require('dockerode');
var docker = new Docker({socketPath: '/var/run/docker.sock'});

// defaults
var defaultDocker = new Docker();

// option add (protocol defaults value: http)
var optionAddDocker = new docker({host: 'HOST IP', port: 'PORT', protocol: 'https'});
```

### create api

```javascript
var container = docker.getContainer('Container ID');

docker.createContainer({
  Image: '이미지 이름',
  AttachStdin: 'true',  // stdin에 연결할 지 여부
  AttachStdout: 'true', // stdout에 연결할 지 여부
  AttachStderr: 'true', // stderr에 연결할 지 여부
  Tty: 'true',          // 닫혀 있지 않은 경우, stdin 포함하여 std streams를 TTY에 연결
  Cmd: '/bin/bash',     // 명령어
  OpenStdin: 'true',    // stdin 열기
  StdinOnce: 'true'     // 연결된 클라이언트 중 하나의 연결이 끊긴 후 stdin을 닫음
})
```
