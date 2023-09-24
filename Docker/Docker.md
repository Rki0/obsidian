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

- `detached`된 `container`를 다시 연결하고 싶다면 `attach` 명령어를 사용하면 된다.
```
docker attach [container name or ID]
```

- 그러면 `detached` 모드에서는 `console.log()`를 어떻게 확인하죠?
```
docker logs [container name or ID]
```
- `logs` 명령어를 사용하면 `detached` 모드에서도 `console.log()`로 출력되는 값들을 확인할 수 있다.
- 위 방법은 `docker logs`를 입력했을 때만 `console.log()`를 출력하므로, 매번 하기 귀찮을 수 있다.
```
docker logs -f [container name or ID]
```
- `-f` 명령어를 같이 사용해주면 `attached` 모드로 전환하여 콘솔을 계속 확인할 수 있다.

- `docker start`를 사용하더라도 `attached` 모드로 실행할 수 있는데,
```
docker start -a [container name or ID]
```
- `-a` 명령어를 함께 사용해주면 된다.

## Interactive mode
- 만약 사용자에게서 입력을 받아야하는 상황이라면 어떻게 해야할까?
- `docker run`을 사용하면 되지 않을까?
- 안된다! 만약 그렇게한다면 사용자에게 입력을 받아야하는 코드를 실행하는 부분에서 에러가 발생하게 된다.
- 왜? `container`나 `container`로 실행되는 애플리케이션에는 입력을 할 수 없기 때문이다.
- 따라서 상호작용이 가능하도록 하는 `-i` 명령어와 터미널을 사용할 수 있게 해주는 `-t` 명령어를 사용해주면 된다.
```
docker run -it [container name or ID]
```
- 터미널에 사용자 입력을 받아내는 코드가 실행되는 것을 확인할 수 있을 것이다.
- 로직을 수행한 뒤에는 `container`가 종료된다.

- 그리고 `container`를 다시 시작하기 위해서 `docker start`를 실행하면..`detached` 모드이기 때문에 `container`와 통신 할 수 없게 된다.
- `attached` 모드에서 시작하면 해결되지 않을까??
```
docker start -a [container name or ID]
```
- 그러나 이 방법은 입력을 단 한번만 받을 수 있기 때문에 문제가 발생할 수 있다.(여러 입력을 받는 경우)

- 따라서 `-i` 명령어를 추가해서 사용해주면 된다.
```
docker start -a -i [container name or ID]
```
- `-t` 명령어의 경우 처음 `docker run`을 실행했을 때 적용되었으므로, 여기서는 하지않아도 된다.

## Delete unused image & container
```
docker rm [container name or ID] [container name or ID] ...
```
- 만약 실행 중인  `container`를 지우려고 한다면 에러가 발생한다.
- 따라서 `container`를 중지하고 삭제를 진행하자.
- 여러 `container`를 나열해서 지울 수도 있다.
- 물론, `stop`된 `container`를 한번에 모두 지울 수도 있다.
```
docker container prune
```

- `image`를 지우는 방법은 무엇일까?
```
docker rmi [image ID] [image ID] ...
```
- 이를 통해 `image`와 그 내부의 모든 `layer`를 삭제할 수 있다.
- 만약, `image`를 사용 중인 `container`가 있다면 `image`를 삭제할 수 없다는 점에 주의하자!
- 즉, `image`를 사용하는 모든 `container`를 `rm`해줘야지, 해당 `image`를 삭제할 수 있다.
- 물론, 삭제할 수 있는 모든 `image`를 지우는 방법도 있다.
```
docker image prune
```

- 그런데 이 방법들...너무 귀찮다. `container`를 `stop`하면 매번 이렇게 `rm`을 일일이 해줘야하는걸까?
- 자동으로 하는 방법이 있다!
- `docker run`을 통해 `container`를 만들 때, `--rm` 커맨드를 같이 써주면 `stop` 시 자동으로 `container`가 삭제된다.
```
docker run -p 3000:80 --rm [container name or ID]
``` 
- 보통 소스 코드가 변경되어 `image`를 다시 `build`하는 상황이면, 새로운 `container`를 만들어야 하기 때문에 `container`를 삭제한다.
- 이런 경우 유용하게 사용될 수 있다.

## image inspection
- `image`의 정보를 살펴보면 용량이 상당히 큰데, 이게 곧 `container`의 용량을 의미하는 것이 아니다.
- `container`는 `image` 위에 추가된 `layer`이기 때문이다. 따라서, 용량은 `image`보다 작다.
- `image`는 소스 코드나 환경 설정을 담고 있기 때문에 용량이 큰 것이다.
- `image`를 검사하는 방법은 다음과 같다.
```
docker images inspect [image ID]
```
- 이 명령을 실행하면 `image`에 대한 정보가 나오게 된다.
- 운영체제, `layer`, `image`를 사용 중인 `container` 등등..
- `layer`는 `Dockerfile`에서 알 수 있는 것들과 개수가 다를 수 있는데, 이는 `Dockerfile`에 정의된 `layer`에만 국한되는 것이 아니기 때문이다.
- 위 예시들에서는...`node image`로 인한 `layer`도 있기 때문!

## Copy file(or folder) from(or to) the container
- 실행 중인 `container`로 혹은 `container` 밖으로 파일 또는 폴더를 복사할 수 있다.
```
docker cp [target source] [destination of copied source]

[target source] === copy할 파일 혹은 폴더
[destination of copied source] === copy된 것이 저장될 곳. container_name:folder_name의 형식으로 작성

ex)
docker cp dummy/. boring_vaughan:/test
dummy 폴더에 있는 내용이 test 폴더에 복사된다.
```
- `container`에서 로컬 환경으로 가져오는 것 또한 동일하다. 실제 코드 예시로 확인해보자.
```
ex)
docker cp boring_vaughan:/test dummy
boring_vaughan라는 container의 test 폴더를 로컬의 dummy 폴더로 복사한다.
```

- 그래서, 이 기능은 어디에 쓰이는 걸까?
- `container`를 다시 시작하고 `image`를 수정하지 않고도, `container`에 무엇을 추가할 때 사용할 수 있다.
- 가령, 소스 코드가 변경되었다면 일반적으로는 `image`를 다시 `build`하고 `container`를 다시 시작해야한다.
- 그러나, 변경된 코드를 `container`에 복사할 수도 있다는 것이다!
- 단, 에러가 발생할 수도 있고, 변경한 파일을 잊어버리기 쉽기 때문에 일반적으로 사용되는 방법은 아니다.
- 더 나은 방법이 존재하므로 뒤에서 살펴보도록 하자.
- 그럼에도 이런 방법을 써야하는 경우는...`config` 같은 파일을 변경해야할 때가 있겠다.
- 혹은 `container`에서 발생한 로그를 로컬로 복사해서 직접 접근하는 경우에도 활용할 수 있다.

## tagging(naming) image & container
- `image`와 `container`는 각각 `TAG`, `NAMES`라는 속성에 자신의 이름을 가지고 있다.
- 별도로 설정하지 않으면 `image`의 경우 `<none>`, `container`의 경우 자동으로 생성된 이름이 할당된다.
- 이런 경우, 본인이 사용하는 것의 이름을 일일이 찾아서 복사하고 붙여넣기를 통해 조작해야한다...
- 아래는 `container`에 이름을 부여하는 방법이다.
```
docker run --name [container's name] [image ID]
```

- `image`의 `TAG`는 두 부분으로 구성된다.
- `TAG` === `name : tag`
- `name` === `image`의 `REPOSITORY`. `image`의 일반적인 이름을 설정할 수 있다. 정확히는 여러 개의 더 전문화될 수 있는 `image`의 그룹을 만들 수 있다.(e.g. `node image`)
- `tag` === 옵션. `image`의 보다 특정화된 버전을 정의할 수 있다.(e.g. `node:14`의 `14`)
- `Dockerfile`의 `FROM`에 작성했던 것을 생각하면 이해가 편하다. 외부의 이미지를 가져온 것이지만.
- 커스텀 `image`에 대해서도 가능하다. 아래 명령어를 살펴보자.
```
docker build -t goals:latest .
```
- `tag` 부분에 꼭 숫자를 써야하는 것은 아니며, `name`만 적는 것도 가능하다.

## Sharing images & containers
- 같은 팀 혹은 배포하려는 서버에 `iamge`, `container`를 공유하는 것이 `Docker`를 사용하는 이유 중 하나!
- `image`를 가지고 있는 사람은 그 것을 기반으로 `container`를 만들 수 있다.

- `image`를 공유하는 방법은 크게 두 가지가 있다.
	1. `Dockerfile`을 공유한다.
		- `Dockerfile`과 애플리케이션이 속한 소스 코드를 제공하면 자체 `image`를 생성하고, `container`를 실행할 수 있게 된다.
		- `docker run build .`를 통해 실행.
		- `Dockerfile`은 소스 코드(폴더, 파일 등등)이 필요할 수 있다.
	2. `build`된 전체 `image`를 공유한다.
		- `Dockerfile`이 아니라 `build`가 완료된 `image`를 공유하는 것이다.
		- `image`를 다운로드하면, 즉시 `container`를 실행할 수 있다.
		- `build`할 필요도 없고, 소스 코드가 필요하지도 않다.
		- 이미 `image`에 다 포함되어 있기 때문이다!
		- 보통 팀 간 공유나 배포를 하는 경우 이 방법을 사용한다.

- 개인적으로 이해한 바로는, 1번은 `image`의 생성 방법을 공유하는 것이고, 2번은 `image`를 공유하는 것이 아닌가싶다.

- 방법이 여러 가지 있다는 것을 알겠는데...그래서 어떻게 공유하는건데?
- `image`를 공유하기 위해 `Docker`는 내장 명령어가 존재한다.

- `image`를 공유하기 위해 `push`(업로드)할 수 있는 위치는 주로 두 가지가 있다.
- `Docker Hub` 혹은 `Private Registry`
	1. `Docker Hub`
		- 공식 `Docker` 이미지 레지스트리이다.
		- public, private, official `image` 등 다양하게 존재한다.(e.g. `node`의 경우 `official`)
	2. `Private Registry`
		- 많은 공급자가 있다.
		- 내가 사용하고자 하는 레지스트리라면 어떤 것이든!
		- 나만의(팀만의) `image`를 둘 수 있다.

- 어찌됐든, `push`를 통해 `image`의 공유가 가능하다!
- 공유된 `image`는 `pull`을 통해 팀원이든, 다른 사람이든 사용할 수 있게 된다.
```

```