# Chapter 2. Docker Basic Usage


## Docker Commands
| Command                                                       | Description                                     | Output |
|---------------------------------------------------------------|-------------------------------------------------|--------|
| docker container rm --force $(docker container ls --all)      | Delete all Created Container                    |        |
| docker container run ( docker image )                         | Run Docker Image from Docker Image              ||
| docker container run --interactive --tty (docker image)       | Run Docker Image and connect to container in CLI ||
| docker container ls                                           | Show Container List                             ||
| docker container top (container name)                         | show Process running in Container               ||
| docker container logs (container name)                        | show Logs runngin Container                     ||
| docker container run --detach --publish 8088:80 (docker image) | Run Container on background for web server      ||
| docker container stats (container id)                         | show container's Info                           ||

## Answer about Exercise



1. create docker image

```commandline
docker container run --detach --publish 8088:80 (docker image)
```

2. Check the Index.html Files

```commandline
docker container exec ls /usr/local/apache2/htdocs
```

3. create index.html files

```commandline
vi index.html
```

4. overwrite index.html files to Cotainer

```commandline
docker container cp index.html (container id):/usr/local/apache2/htdocs/index.html
```

## 도커 개념

1. 도커의 3가지 과정 (빌드, 공유, 실행)
- 빌드 : 애플리케이션을 컨테이너 실행할 수 있도록 패키징 하는 것
- 공유 : 다른 사람이 해당 애플리케이션을 사용할 수 있도록 공유
- 실행 : 패키지를 다운 받아 컨테이너를 통해 애플리케이션을 실행 함.

2. 컨테이너란?

- 격리와 밀집을 제공하는 것.
- 격리란 ? : 애플리케이션이 사용하는 자바나 닷넷 등 필요로 하는 런타임의 버전이 달라 서로 다른 가상환경을 제공해 주어야 하는 것을 의미.
- 밀집이란 ? : 컴퓨터에 CPU와 메모리가 허용하는 한 되도록 많은 수의 애플리케이션을 실행하는 것을 의미한다.

-> 따라서 각각의 컨테이너는 호스트 컴퓨터의 운영체제를 공유하므로 리소스를 줄이고, 실행도 빠르고 호스트 컴퓨터에서 가상 머신에 비해서 더 많은 수의 애플리케이션을 실행 시킬 수 있다.
또한 외부와 독립된 환경을 제공하므로, 밀집과 격리가 동시에 달성되는 것이다.

3. 도커의 내부 동작

- 도커를 구성하는 컴포넌트는 다음과 같다. 
1. 도커 엔진 : 도커의 관리 기능을 맡는 컴포넌트이다. (항시 동작하는 백그라운드 프로세스이다.)
2. 도커 API : 표준 HTTP 기반 REST API이다. 
3. 도커 CLI Interface : 도커 API의 클라이언트이다. 


