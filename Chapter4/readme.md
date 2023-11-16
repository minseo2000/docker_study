# 애플리케이션 소스 코드에서 도커 이미지까지!

1. "Dockerfile"이 있는데 빌드 서버가 필요할까?
2. 애플리케이션 빌드 실전 예제: 자바 소스 코드
3. 애플리케이션 빌드 실전 예제 : Node.js 소스 코드
4. 애플리케이션 빌드 실전 예제 : Go 소스 코드
5. 멀티 스테이지 Dockerfile 스크립트 이해하기
6. 연습 문제

## "Dockerfile"이 있는데 빌드 서버가 필요할까?

- 개발에 필요한 모든 도구를 배포하는 Dockerfile 스크립트를 작성한 다음 이를 이미지로 만든다.
- 애플레키이션 패키징을 위한 Dockerfile 스크립트에서 이 이미지를 이용해 소스 코드를 컴파일함으로써 애플리케이션을 패키징하는 것이다.
- 간단한 Dockerfile

```
FROM diamol/base AS build-stage
RUN echo 'building...' > /build.txt

FROM diamol/base AS test-stage
COPY --from=build-stage /build.txt /build.txt
RUN echo 'Testing..' >> /build.txt

FROM diamol/base
COPT --from=build-stage /build.txt /build.txt
CMD cat /build.txt
```

## 애플리케이션 빌드 실전 예제: 자바 소스 코드
- 자바 스프링부트를 사용해 구현한 것.
- 자바 파일을 빌드하기 위해 필요한 모든 도구를 도커로 불러온다.
- 표준적인 자바 빌드 도구인 메이븐과 OpenJDk를 사용₩
- 자바 스프링 빌드 간단한 예제파일

```
FROM diamol/maven as builder

WORKDIR /usr/src/iotd
COPY pom.xml .
RUN mvn -B dependency:go-offline

COPY . .
RUN mvn package

FROM diamol/openjdk

WORKDIR /app
COPY --from=builder /usr/src/iotdtarget/iotd-service-0.1.0.jar .

EXPOSE 80
ENTRYPOINT ['java', '-jar', '/app/iot-service-0.1.0.jar']
```

## 애플리케이션 빌드 실전 예제 : Node.js 소스 코드
- 이번에는 Node.js 애플리케이션을 빌드하는 스크립트
- 서로 다른 빌드 방식을 살펴보는 것도 도움이 될 것
- 자바와는 다른 스크립트 언어 많이 채택되는 기술이다.
- Node.js는 빌드가 필요 없음 따라서 Node.js 런타임 파일과 소스코드만 있으면 실행이 가능하다.

## 멀티 스테이지 Dockerfile 스크립트 이해하기
- Dockerfile 스크립트의 동작원리와 컨테이너 안에서 애플리케이션을 빌드하는 것이 왜 유용한지를 설명.
- 장점1 : 표준화 ? -> 어떤 운영체제든, 그리고 로컬 컴퓨터에 어떤 도구를 설치했는지와 상관없이 모든 빌드 과정은 도커 컨테이너 내부에서 이뤄진다.
- 그리고 이들 컨테이너는 모든 도구를 정확한 버전으로 갖추고 있다.
- 장점 2 : 성능 향상 -> 멀티 스테이지 빌드의 각 단계는 자신만의 캐시를 따로 갖는다. 
- 도커는 빌드 중 각 인스트럭션에 해당하는 레이어 캐시를 찾는다. 
- 해당되는 캐시를 찾지 못하면, 남은 인스트럭션이 모두 실행되지만, 그 범위가 해당 단계 안으로 국한된다.
- 