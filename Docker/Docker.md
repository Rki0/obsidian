# Relation between image and container

- JS로 따지자면 `image`는 `class`에 작성 코드이고, `container`는 그를 사용하여 만든 `instance`이다.
- 즉, `image`는 소스 코드, 환경 설정(e.g. node 백엔드 코드 및 환경 설정) 등이 들어 있으며, `container`는 그 것들을 실행하는 것이다.
- 즉, `image`를 기반으로 `container`를 실행하는 것!
- 또한, 하나의 `image`로 여러 개의 `container`를 만들 수 있다는 것도 알 수 있다.
- `container`는 `image` 위에 `layer`를 추가하는 것이므로, `image`에 있는 코드들을 `container`에 또 복사하는 일은 없다. `container`는 `image`를 기반으로 작동한다!!
- `container`는 여러 개가 될 수 있다고 했는데, 각 `container`는 서로 독립적이라는 것을 잊지말자! 하나의 `image`를 기반으로 했더라도, `container` 간 상태의 공유 같은 것은 발생하지 않는다.

# Dockerfile

```dockerfile
FROM node

WORKDIR /app

COPY . /app

RUN npm install

EXPOSE 80

// 이 부분은 container가 실행할 명령을 적는 것이다.
CMD [ "node", "server.js" ]
```

- `Docker`의 설정을 위해 사용하는 파일이다.
- 이 파일은 `image`에 대한 설정을 하기 위한 것임을 잊어서는 안된다!
- 즉, 이 파일에 적히는 코드는 `image`가 빌드될 때 실행된다는 것이므로, 불필요한 코드를 작성해서 `image`가 빌드 될 때마다 실행되는 불상사가 없도록 하자(e.g. `RUN node sever.js`를 작성해서 `image` 빌드마다 node 서버 코드를 실행히버리는 일. 이는 `container`를 실행하는 경우에 실행할 코드이기 때문.)
- 또한, `container`는 생성하는 것만으로는 그 포트가 외부와 연결되지 않기 때문에 고립된 상태로 존재하는데, 이를 외부와 연결하여 다른 사람들이 접근할 수 있도록 하기 위해 외부 포트와 연결해줘야한다.

```
docker run -p 3000:[port number of image] [container ID]
```

- `image`는 읽기 전용이다! 즉, 한번 `build`된 `image`는 잠겨있으며 스스로 업데이트를 하지 못한다.
- 기본적으로 `image`는 복사(`COPY`) 시점에 소스 코드의 스냅샷을 만든다.
- 따라서, `COPY`가 진행된 후에 변경된 코드는 `image`에 반영되지 않는다.
- 즉, 코드에 변경이 있더라도 `image`를 다시 `build`하지 않으면, 반영이 되지 않는다는 것이다.

# Image is a layer-based
- `image`는 `layer` 기반이다.
- `image`를 다시 `build`할 때, 변경된 부분의 명령과 그 이후의 모든 명령이 재평가 된다.
- 변경되지 않은 코드를 다시 `build`하는 경우 `CACHED`라는 문구가 함께 나오고, 훨씬 빨리 `build`가 완료되는 것을 볼 수 있다.
- 즉, `Docker`는 `Dockerfile`의 명령어를 다시 실행 했을 때의 결과가 이전과 동일하다는 것을 인식 할 수 있다는 것이다.
- `Docker`는 각 명령의 결과를 `cache`하고 `image`를 다시 `build`할 때, 명령을 다시 실행할 필요가 없으면 `cache`된 결과를 사용한다.
- 즉, 모든 명령은 `Dockerfile`의 `layer`를 나타낸다!
- 그러나, 변경이 생기면 변경 이후의 `layer`가 무효화 된다는 것을 잊지말자!

# Let's optimize "npm install"
- 그런데 소스 코드의 변경이 `dependencies`의 변경을 의미하지는 않는다.
- 따라서, `npm install`을 매 `build`마다 실행하는 것은 명백히 낭비라고 생각된다.

```dockerfile
FROM node

WORKDIR /app

COPY package.json /app

RUN npm install

COPY . /app

EXPOSE 80

CMD [ "node", "server.js" ]
```

- `package.json`을 `container`의 `app` 디렉토리에 `COPY`한 다음
- `npm install`을 `RUN`하고
- 다른 코드를 `COPY`한다.
- 이러면 다른 코드를 `COPY`하기 전에 `npm install`의 `layer`가 먼저 오게 되면서
- 소스 코드에 변경이 있더라도, 소스 코드 `COPY` 명령 앞의 `layer`는 무효화되지 않게 된다.
- 즉, `npm install`이 다시 실행되지 않는다는 것이다!
- 최적화 이전과 비교했을 때, `build` 속도가 유의미하게 빨라진 것을 확인 할 수 있다.

# Managing images & containers

## Restart stopped container!
- 지금까지는 `docker stop [container name]`으로 중지한 `container`를 재사용하려면 `docker run -p .....`를 사용해서 다시 시작을 했었다. 그리고 이 방법은 새로운 `container`를 생성하여 실행하는 방법이었다.
- 그런데 `image`가 변경되지 않은 경우라면 굳이 완전히 새로운 `container`를 생성할 필요가 없다.
- `docker ps -a`를 통해 모든 `container`를 검색할 수 있었다. 여기엔 중지한 `container`도 포함되어 있다.
- `docker start [container name]`을 통해 중지했던 `container`를 다시 시작할 수 있다!
- `docker run ...`은 터미널이 입력 불가능한 상태로 유지되었던 반면, `docker start ....`는 `container`를 실행하고 터미널에 다른 입력이 가능한 상태로 다시 돌아온다.

## Attached & Detached container
- `docker run`으로 시작하는 경우, `attached` 모드가 디폴트이다.
- `docker start`로 시작하는 경우, `detached` 모드가 디폴트이다.
- `docker run`의 경우, 터미널이 계속 유지되고 있기 때문에 `console.log()` 같은 것들을 확인할 수 있다.
- 그러나, `docker start`의 경우, 터미널이 입력 가능 상태로 돌아오기 때문에 `console.log()` 같은 것들을 확인할 수 없다.
- 즉, `attached` 모드는 그 `container`의 출력 결과를 수신한다.

- 그러면 `docker run`은 `detached` 모드로 할 수 없나요?
```
docker run -p 3000:80 -d [container name or ID]
```
- `-d` 커맨드를 사용하면 `docker run`도 `detached` 모드로 실행할 수 있다.
- `detached`된 `container`를 다시 연결하고 싶다면
```
docker container attach [container name or ID]
```

## tagged(naming for image)
