# 도커 운용
 
Portainer를 기반으로 운용

## 도커 구성

#### 10.0.12.194

메인 운용환경 + 개발환경

**설치항목**

* ★ portainer : docker web gui

* ★ gitlab-runner : gitlab pipeline 기동 데몬

* mariadb

#### 10.0.12.213

서브 개발환경

## Portainer

Docker Web UI 관리툴로 오픈소스로 배포되고 있어 무료로 사용이 가능하며, 쉘프롬프트에서 Docker 명령을 일일이 수행할 필요가 없이 Web UI 로 손쉽게 관리가능
!picture921-1.png!

#### 계정정보

* ID/PW : root / xhdgkqroqkf2xla

#### 접속정보

* url : http://10.0.12.194:9000

## 특이사항

#### repository 구성

* 10.0.35.10:5000, 10.0.35.10:5001
* id/pw : dev2 / sniper<21>

#### Portainer 구성

* 2개의 entrypoint로 구성, 10.0.12.194(local), 10.0.12.213(docker api)
* local entrypoint 구성간 selinux 비활성화 시켜야됨

<pre>
[root@gitlab ~]# setenforce 0
[root@gitlab ~]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   permissive             <--------- 원래 기동시 enforcing이며 변경시 permissive
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31
</pre>

* docker api는 docker service파일 옵션 수정, iptable 포트오픈, 방화벽오픈 작업 필요
** service 파일 수정 

<pre>
[root@localhost ~]# vi /etc/sysconfig/docker
# /etc/sysconfig/docker

# Modify these options if you want to change the way the docker daemon runs
OPTIONS='--selinux-enabled --log-driver=journald --signature-verification=false -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375'
if [ -z "${DOCKER_CERT_PATH}" ]; then
    DOCKER_CERT_PATH=/etc/docker
fi
</pre>

** iptables 포트오픈

<pre>
iptables -I INPUT -i docker0 -j ACCEPT
</pre>

** 방화벽 해제

<pre>
service firewalld stop
</pre>

** 위 작업후 도커 서비스 restart

<pre>
[root@localhost ~]# netstat -tnlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1199/sshd
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1466/master
tcp6       0      0 :::22                   :::*                    LISTEN      1199/sshd
tcp6       0      0 ::1:25                  :::*                    LISTEN      1466/master
tcp6       0      0 :::2375                 :::*                    LISTEN      13085/dockerd-curre    <-- 도커 api 포트확인
</pre>

** portainer web ui에서 해당 서버 ip:port로 endpoint 추가
