# docker_description

### docker가 궁금할 때

### docker : https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html

-------------------------------------------------------------------------------

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
var optionAddDocker = new Docker({host: 'HOST IP', port: 'PORT', protocol: 'https'});

var container = docker.getContainer('Container ID'); // docker에서 id에 해당하는 container를 가져온다.
```

### create api

```javascript
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

### get container list api

```javascript

// 기동중인 Container의 data만 조회된다.
docker.listContainers(function(err, containers) {
  for (container of containers){
    //container의 이름만 조회
    console.log(container['Names']);
  }
})
```

### start container api

```javascript
container.start(function(err, data) {
  console.log(`err: ${err}, data: ${data}`);
})
```

### stop container api

```javascript
container.stop(function(err, data) {
   console.log(`err: ${err}, data: ${data}`);
 })
```

### rm container api

```javascript
container.remove(function(err, data) {
   console.log(`err: ${err}, data: ${data}`);
 })
```

### get container's log api

```javascript
container.logs({follow: true , stdout : true , stderr : true },function(err, data) {
   data.setEncoding('utf8')
   data.on('data', info => console.log(info));
 })
```
