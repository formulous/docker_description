# docker_description

## docker : 부두에서 컨테이너를 다루는 노동자라는 뜻

### 한 대의 컴퓨터 안에서 각각의 앱을 실행시킨다. 격리된 환경에서. 운영체제가 설치된 컴퓨터를 host, host에서 image를 실행하면 만들어지는 환경을 container라고 한다.

* `docker images` : install 된 images 출력

* docker hub 에서 image 를 pull 해서 container 에서 run

* `docker pull [image name]` : image install

** apache web server's image name : httpd

** `docker images` : install check

* `docker run [OPTIONS] [Image Name]` : make container

* `docker stop [Container name]
