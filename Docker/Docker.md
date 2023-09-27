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
docker push [image name]
docker pull [image name]
```
- 만약 `Private Registry`에 `push`, `pull`을 하려면 호스트를 표시해야하기 때문에, 명령어에 해당 공급자의 `URL`이 포함되어야한다.
- 가령, `image name`을 `HOST:NAME` 이런 식으로 한다던지 말이다.

- 아래는 1번 방법으로 진행하는 경우에 대한 것이므로 참고하자.
- 레포지토리가 곧 `image`이다.
```
docker push [image name]

[image name] === Docker Hub 가입 시 설정한 닉네임/레포지토리 이름 의 형태이다.

ex)
rki0/udemy_node
```
- 위 명령어를 통해 `Docker Hub`에 만들어둔 레포지토리에 `image`를 공유한다.

- 사용 중인 `image`를 `Docker Hub`애 공유한 `image`로 바꾸려면?
- 두 가지 방법이 있다.
1. 해당 `image`를 사용하고 있는 디렉토리로 가서, `Docker Hub`의 `image`로 다시 `build`하면 된다.
```
cd udemy_docker(소스 코드가 있는 로컬 폴더)
docker build -t rki0/udemy_docker .
```

2. 사용하려는 `image`를 이미 가지고 있다면, 이를 재사용할 수 있다.
	- `image`의 이름을 바꿔주면 된다!
```
docker tag [old name] [new name]

ex)
docker tag goals:latest rki0/udemy_docker
```
- `name`만 써도 되고, `tag`까지 써도 된다.
- 기존의 `image`는 삭제되지 않고 그대로 있고, 복제되어 새로운 `image`가 생성된다!
- 새로 생성된 `image`를 `push`하면 `Docker Hub`에 공유할 수 있다....

- 그런데, `push`를 해봤더니 거절 에러가 뜬다.
```
denied: requested access to the resource is denied
```
- 로컬에서 실행하는 `Docker` 명령은 내가 `Docker Hub`의 레포지토리와 어떤 관계가 있는지 알지 못하기 때문이다.
- 즉, 레포지토리에 변경을 가할 수 있는 권한이 있는지 모르기 때문에 그런 것이다.
- 따라서 로그인을 한 뒤 진행해야한다. 한번만 해주면 된다.
```
docker login
```
- 로그인을 한 뒤, `push`를 하면 정상적으로 작동하는 것을 확인할 수 있다.

- 그런데, 모든 것을 다 `push`하지는 않는다.
- `FROM`에 작성하는, `Docker Hub`에 있는 `image`는 연결해서 필요한 추가 코드만 `push`한다.

- `push`를 했으니 `pull`을 해서 공유받는 것도 진행해보자.
- 앞서 `docker login`을 해놨기 때문에, 완전히 레포지토리에 대해 모르는 상황을 가정하기 위해 `docker logout`을 한 뒤 진행하였다.
```
docker pull [image name]

ex)
docker pull rki0/udemy_docker
```
- `pull`을 성공하는 것으로, public에 공개된 `image`는 누구든 사용할 수 있다는 것을 확인했다.

- `docker run` 만으로는 `Docker Hub`에 있는 최신 `image`를 보장할 수 없다.
- 따라서, `docker pull`을 통해 `image`를 최신화한 뒤, `docker run`을 할 필요가 있다.
- `docker run`은 `image`가 로컬에 없다면, 히스토리를 검색해 자동으로 `image`를 `pull`하지만, 로컬이 `image`를 가지고 있다면 그게 최신 버전인지 아닌지 상관없이 `pull`하지 않고 그걸 그대로 사용한다.

# Managing Data in images & containers
- 음? `image`와 `container`를 관리하는게 곧 데이터를 관리하는거 아닌가요? 소스 코드와 환경 설정 등은 데이터잖아요...!
- 다른 종류의 데이터가 있다!

## Data
1. Application(Code + Environment)
	- Written & provided by you(= the developer)
	- Added to image and container in build phase
	- "Fixed" : Can't be changed once images is built
	- Read-only, hence stored in **image**
2. Temporary App Data(e.g. entered user input)
	- Fetched/Produced in running container
	- Stored in memory or temporary files
	- Dynamic and changing, but cleared regularly
	- Read + Write, temporary, hence stored in **containers**
	- `image`가 `Read-only`인거지, `image` 위에 생성되는 `layer`인 `container`는 `Read-only`가 아니다!
	- 따라서 동적인 값들은 `container layer`에 저장된다.
3. Permanent App Data(e.g. user accounts)
	- Fetched/Produced in running container
	- Stored in files or a database
	- Must not be lost if container stops/restarts
	- Read + Write, permanent, stored with **containers** & **volumes**

## Volumes of images & containers
- `container`에서 실행한 애플리케이션에서 저장한 파일이나 데이터들은 `container`에 저장되는 것이지, 로컬에 저장되는 것이 아니다.
- 가령, `node.js`를 활용해서 유저의 입력을 텍스트 파일로 변환하는 코드를 작성했다고 해보자.
- 이를 `container`에서 실행하면 그 파일은 `container`에서 확인할 수 있지, 로컬(i.e. 각자의 IDE) 폴더에서 확인할 수 없다.
- 즉, `image`와 `container`는 그들의 코드가 있는 로컬 폴더를 기반으로 하는 **자체 파일 시스템**이 있다는 것을 의미한다!!
- `container`를 중지하는 것은 데이터를 삭제시키지 않는다.(`--rm`으로 `docker run`한 경우에는 중지가 곧 `container`의 삭제이므로 당연히 데이터도 사라진다는 점에 주의하자.)
- 따라서 `docker start`로 재시작을 해도 데이터가 그대로 `container`에 남아있는다.
- 같은 `image`를 사용했더라도 `container`가 다르면 당연히 데이터가 사라진다.
- 즉, `container`에서 생성된 데이터는 `image`에 write 되는게 아니라는 것이다! `image`가 `Read-only`라는 것에서 유추할 수 있는 부분이기도 하다.
- 한마디로, 동일한 `image`에 기반한 다수의 `container`는 서로 완전히 독립된, 격리된 개체로 작동한다.

- `container`가 삭제되었을 때, 그 내부에 존재하던 데이터가 전부 사라지는 것은 치명적일 수 있다.
- `container`는 지웠으나 데이터는 계속 유지되기를 바라는 것이 보통이니까.
- 가령, 코드를 수정하고 `image`를 다시 `build`하고 실행하는 `container`는 새로운 것이기 때문에, 데이터가 삭제되는 것은 상당히 귀찮은 문제를 불러올 수 있다.
- 이 문제에 대한 해결책이 바로 `volume`이다!

## Introduction of Volumes
- `volume`은 호스트 머신의 폴더이다.
- 호스트 머신의 하드 드라이브에서 꺼내쓰거나, `container`에 맵핑되는 것을 의미한다.
- 즉, `Docker`가 인식하는 호스트 머신인 나의 컴퓨터에 있는 폴더로서, `container` 내부의 폴더에 맵핑된다.
- 즉, `image`나 `container`에 존재하는 것이 아니다!
- `Dockerfile`에서 `COPY` 명령어를 통해 소스 코드의 스냅샷을 만들어서 사용했던 것은 `container`와 지속적인 관계를 가지지 않는다. 그러나! `volume`은 `container` 내부의 폴더를 호스트 머신 상의, `container` 외부 폴더에 연결할 수 있다!!
- 게다가 두 폴더의 변경 사항은 다른 폴더에 반영된다! 즉, 호스트 머신에서 파일을 추가하면 `container` 내부에서 엑세스할 수 있고, `container`가 맵핑된 경로에 파일을 추가하면 호스트 머신에서도 사용할 수 있다는 것이다.
- `volume`은 `container`가 종료된 후에도 계속 유지된다.
- `container`에 `volume`을 추가하면 해당 `volume`은 제거되지 않으며, `container`가 제거되더라도 `volume`은 유지되기 때문에 다른 곳에서 다시 사용할 수 있다.
- `container`는 `volume`에 데이터를 Read, Write 할 수 있다.

## Add volumes into the container
- `Dockerfile`에 `VOLUME` 명령어를 사용해서 `volume`을 추가할 수 있다.

```dockerfile
FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

VOLUME [ "/app/feedback" ]

CMD [ "node", "server.js" ]
```

- `volume`으로 사용하고자하는 폴더를 연결해주면 된다.
- 위 예시에서는 소스 코드의 `feedback`이라는 폴더를 `volume`으로 사용하고자 하는 것이다.
- `WORKDIR`가 `app`이기 때문에 `app` 내의 `feedback` 폴더의 접근하는 것을 확인할 수 있다.
- 이는 `container` 외부 폴더에 맵핑되어질 `container` 내부 위치이다.

- 그러나, `feedback` 폴더가 `volume`으로 명시된 것이 확인되지만, `container`를 삭제하고 다시 실행하면 `volume`으로 사용되는 `feedback` 폴더가 데이터를 유지하고 있을거라는 예상과는 다르게, 데이터는 삭제된 채로 다시 실행된다.
- 무엇이 문제일까?

## Two Types of External Data Storages
- `Docker`에는 여러 가지의 외부 데이터 저장 메커니즘이 있다.
- `Volumes`와 `Bind Mounts`가 있다.

1. `Volumes`(Managed by Docker)
	1. `Anonymous Volume`
		- 앞서 작동하지 않았던 `Dockerfile`의 `VOLUME`이 바로 `Anonymous Volume`이다.
		- `Docker` 내부의 경로만 지정했지, 호스트 머신의 경로는 지정하지 않았었다.
		- 따라서, 미러링된 폴더가 어디에 있는지 모르는 상태였던 것이다.
		- `Anonymous Volume`은 `Docker`가 관리한다.
		- `container`가 존재하는 동안에만 실제로 존재하게 된다. 즉, `container`를 삭제하면 데이터가 사라지게 된다.
		- 하나의 `container`에 밀접하게 연관되어 있다.
		- 물론, `--rm` 옵션을 사용하지 않고 `run`하면 `container`가 삭제된다고 하더라도, 여전히 `Anonymous Volume`이 남아있다.
		- 이런게 반복되면 계속 쌓이게 되므로, `docker volume rm VOL_NAME`, `docker volume prune` 등을 사용하여 이를 제거할 수 있다.
		- `container`에 이미 존재하는 특정 데이터를 잠그는데 유용하다. 다른 모듈에 의해 덮여쓰여지는 것을 방지하는데 유용하다. 아래에서 실제 사례와 함께 살펴보게 될 것이다.
	2. `Named Volume`
		- `container`가 종료된 후에도 `volume`이 유지된다.
		- 즉, 하드 드라이브의 폴더가 그대로 유지된다.
		- 따라서, 새로운 `container`를 시작하더라도 `volume`이 복구되고, 폴더가 복구되어 해당 폴더에 저장된 모든 데이터를 계속 사용할 수 있다.
		- 영구적이어야하거나, 편집할 필요가 없는, 직접 볼 필요가 없는 중요한 데이터에 적합한 방식이다.
		- `-v` 커맨드를 사용하여 `Named Volume`을 생성할 수 있다.
		- 저장하려는 `container` 파일 시스템 내부의 경로를 여전히 지정해야한다.
		- 따라서, `feedback:/app/feedback`의 형태로 사용한다. `:` 앞은 외부 경로, 뒤는 `container` 내부 경로이다.
		- 이제 관리되는 `volume`에 `/app/feedback`을 저장한다.
		- 즉, 호스팅 머신에  폴더를 만들어 `container` 내부의 이 폴더에 연결하지만, 이 `volume`은 우리가 지정한 이름으로 저장되는 것이다!!
		- `Anonymous Volume`과 다른 점은, `container`가 삭제되었을 때, `Docker`에 의해 `volume`이 삭제되지 않는다는 것이다.
		- 하나의 `container`에만 연결되는 것이 아니다.
		- 다른 `container`를 생성할 때도 마찬가지로 `-v`를 사용하여 사용하고자하는 `volume`을 연결해주면 그대로 데이터가 남아있는 것을 확인할 수 있다.

	- 두 경우 모두 `Docker`는 일부 폴더와 경로를 호스트 머신에 설정한다.
	- `docker volume` 명령어를 통해 접근할 수 있다.
	- `docker volume ls`를 통해 `volume`인 것들의 리스트를 확인할 수 있는데, `Anonymous Volume`의 경우 `VOLUME NAME`이 무작위로 암호화된 문자열로 나타나게 된다.
	- `container`에 정의된 경로는 어떠한 생성된 `volume`에 맵핑된다. 그래서 호스트 머신 상의 생성된 경로로 연결된다.
	- 가령, 앞서 `VOLUME`에 작성했던 `/app/feedback`은 호스트 머신의 어떤 폴더에 매핑된다는 것이다. 물론, `Anonymous Volume`이었기 때문에 우리는 그 경로를 모르는 상태였고, `Docker`가 관리하는 상태였으며, 내 컴퓨터의 어딘가에 숨겨저 있었고, 내가 액세스 할 수 없도록 되어 있었다...
	- 아래는 `Named Volume`의 실행 코드 예시이다.

```
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes
```

2. `Bind Mounts`(Managed by you)
	- 소스 코드에 변경이 있으면 `image`를 다시 `build`해야했다.
	- 그러나, 프로젝트를 진행하다보면 소스 코드의 변경이 매우 잦기 때문에, 매번 `build`를 다시 하는 것은 굉장히 번거롭다.
	- 이를 도와주는 것이 `Bind Mounts`이다.
	- `Volumes`와 비슷한 점이 몇 가지 있지만, 한가지 큰 차이점이 있는데, 바로 `Docker`에 의해 관리되는 `volume`의 위치, 즉, 호스트 머신의 파일 시스템 상의 `volume`이 어디 있는지를 알고 있다는 것이다.
	- 일반적인 `volume`이라면 우리는 그게 어디 있는지 몰랐었다.
	- 이는 우리가 호스트 머신 상에 맵핑될 `container`의 경로를 설정하기 때문에 가능한 것이다!
	- 호스트 머신 상에서의 경로를 우리가 완전히 인식할 수 있게 되기 때문에, `container`는 `volume`에 Write뿐만 아니라 Read도 가능해져서, 소스 코드를 `Bind Mounts`에 넣을 수 있게 된다!
	- 즉, 스냅샷을 계속 갱신할 필요가 없게 된다는 것이다.
	- 소스 코드를 `Bind Mounts`에 넣으면 `container`가 이를 인식하여, 소스 코드를 스냅샷에서 복사하는 것이 아니라 `Bind Mounts`에서 복사하게 된다. 이는 호스트 머신의 어떤 폴더에 연결된 것이기 때문에`container`가 항상 최신의 소스 코드에 액세스할 수 있는 것이다!!
	- 영구적이고 나에 의해 편집 가능한 데이터에 적합하다.(e.g. 소스 코드)
	- `image`가 아니라, 실행 중인 `container`에만 적용되는 것이기 때문에 `Dockerfile`에서 설정할 수는 없다. 즉, `image`에는 영향을 주지않고 `container`에만 영향을 준다.
	- 호스트 머신의 경로를 이용하기 때문에 호스트의 파일 시스템, 즉, 로컬에서 지워야한다.

```
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v /Users/pakkiyoung/udemy_docker:/app feedback-node:volumes 


// shortcuts version(경로를 매번 복사하기 귀찮은 경우)
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v $(pwd) feedback-node:volumes 
```

- 아까의 명령어에서 `-v`가 또 추가된 것을 확인할 수 있다.
- 로컬에서 프로젝트 폴더의 경로를 `:` 앞에 붙여준 것이다.
- 만약, 공백 같은 특수 문자가 경로에 포함되어 있다면 `"blabla:blabla"`처럼 `""`로 감싸서 사용하도록 하자.
- 또한 소스 코드 전체를 사용하는 것이기 때문에 `:` 뒤에는 `/app`만 적어주었다. 특정 폴더만 사용하는 것이라면 더 깊게 들어가겠지만 말이다.
- 마지막으로, 공유 중인 폴더에 `Docker`가 액세스 할 수 있는지 반드시 확인해줘야한다.
- `Docker`의 `Settings` 탭의 `Resources` 탭의 `File sharing` 탭에 공유 중인 폴더의 경로(상위 경로)가 들어있는지 체크해줘야한다.
- 체크해본 결과 `/Users`가 잘 표시되고 있는 것이 확인되었다.

- 그렇게...위 명령어를 실행했더니..!! 에러가 발생한다. 웹 페이지가 열리지않는다.
- 심지어 `docker ps -a`로 확인해봤더니 `container` 자체가 보이지 않는다.
- 아무래도 실행과 동시에 종료되어 `--rm`으로 인해 삭제되는 것으로 보인다...
- `--rm`을 지우고 `logs`를 확인해본 결과, 실행과 동시에 종료되는 것은 확인이 되었고, 그 이유는 `express`가 설치되지 않았기 때문이라고 한다.
- `dependencies`가 제대로 `npm install`이 안된 것인데...이전까지는 잘됐다는게 문제다...
- 왜 갑자기 안된다고 하는걸까?
- 방금 추가한 `Bind Mounts`와 관련이 있다.
- 무엇이 문제였을까?

- `/app` 폴더에 모든 소스 코드를 덮어쓰기를 했는데, 이게 문제였다.
- `RUN npm install` 등, `Dockerfile`의 명령들을 전부 수행한 뒤, 소스 코드를 덮어쓰기를 했기 때문에 `Dockerfile`에서 `image`를 생성하기 위해 했던 명령어들이 전부 의미없게 되버린 것이다.
- 그래서 `npm install`이 적혀있었음에도 실행되지 않은 것 처럼 된 것이다. 실행된 소스 코드에 실행 전 소스 코드를 덮어쓰기 해버린 꼴이 된 것이니 말이다.(로컬 폴더에서는 `npm install`을 실행하지 않은 상태였다)
- 이를 해결하기 위해 내부 파일 시스템에 외부에서 덮어쓰지 않아야하는 것이 있다는 것을 `Docker`에게 알려줘야한다!!!
- 그 방법은 바로 `Anonymous Volume`을 사용하는 것이다.

```
docker run -d -p 3000:80 --name feedback-app -v feedback:/app/feedback -v "/Users/pakkiyoung/udemy_docker:/app" -v /app/node_modules feedback-node:volumes
```

- `Dockerfile`에서 `VOLUME [ "/app/node_modules" ]`를 한 것과 같은 효과를 낸다.
- 그래서, 이게 어떤 도움을 주는걸까?
- `Docker`는 항상 `container`에 설정하는 모든 `volume`을 평가하며, 충돌이 있는 경우에는 더 긴 내부 경로를 우선시한다.
- 여전히 `/app`에 `Bind`를 하지만, `app` 폴더 내부의 `node_modules` 폴더에 `Bind`를 하게 되는 것이다.
- 그 결과 `node_modules`가 살아남게 되어서, 이 폴더가 외부에서 들어오는 유입, 즉, `Bind Mounts`로 인한 충돌을 `Anonymous Volume`으로 덮어쓰게 되는 것이다.
- 그런데 `Bind Mounts`에서는 `node_modules`가 없는데요...? 그렇다. 그냥 존재하지 않는 `node_modules` 폴더를 덮어쓰는 것이다.
- 정리하자면, `npm install`을 통해 `image` 생성 중에 생성된 `node_modules` 폴더는 살아남아 실제로 여전히 작동하는 `Bind Mounts`와 함께 공존한다. 즉, `node_modules`는 일종의 예외라고 볼 수 있다. `node_modules`는 `Bind Mounts` 폴더의 내용물로 덮어쓰여지지 않게된다.
- 즉, 외부(로컬) 경로보다 내부(`container`)의 경로를 우선시 할 수 있게 만들어준 것이다.
- 이제 소스 코드의 변경 사항을 즉각 반영해주는 것을 확인할 수 있다!

- 그런데....html 파일을 수정하는 것은 반영이 되는데, js 파일을 수정하는 것은 반영이 안된다.
- 왜?????
- `server.js`는 `node.js` 런타임에 의해 실행된다.
- 따라서 서버를 다시 실행해주면 변경 사항이 반영이 된는데...
- `container`가 실행 중일 때, 서버만 다시 시작하는 것은 간단하지 않다.
- 좋은 방법은 아니지만 가장 간단하게 생각해볼 수 있는 방법은 `container`를 중지하고, 다시 시작하는 것이 있겠다.

- 로컬에서 개발할 때 주로 `nodemon`을 설치해서 이를 해결했었다. `Docker`에서도 그렇게 해보자!
- `package.json`에 수동으로 라이브러리를 추가해준다.
- 이제 `nodemon`으로 `server.js`가 실행되도록 `scripts`도 변경해주자.
- `Dockerfile`의 `CMD`도 그에 맞춰 `CMD [ "npm", "start" ]`로 변경해주자.
- 그리고 `image`를 다시 `build`해주자.
- 이제 js 파일의 변경도 즉각 반영해주는 것을 확인할 수 있다!!

## Overview Volumes & Bind Mounts

```
// Anonymous Volume
docker run -v /app/data...

// Named Volume
docker run -v data:/app/data...

// Bind Mounts
docker run -v /path/to/code:/app/code...
```

## Read-only Volume

```
docker run -d -p 3000:80 --name feedback-app -v feedback:/app/feedback -v "/Users/pakkiyoung/udemy_docker:/app:ro" -v /app/node_modules feedback-node:volumes
```

- `Bind Mounts` 뒤에 `:ro`를 붙여서 `Read-only`로 만들 수 있다.
- 그러면 `Docker`가 그 폴더나, 그 하위 폴더에 Write를 할 수 없게 된다.
- 물론, 여전히 호스팅 머신에서는 소스 코드 변경이 가능하다.
- `container`와 거기서 실행되는 애플리케이션에만 영향을 주는 것이다.

## Managing Volume
- `Docker`는 `-v`로 지정한 `volume`이 존재하지 않으면 자동으로 생성한다.(지금까지 봐온 방법)
- 그런데 아래 명령어를 통해 수동으로 `volume`을 생성할 수도 있다.

```
docker create volume [VOLUME_NAME]
```

- `inspect`를 통해 `volume`의 정보를 볼 수도 있는데,

```
docker volume inspect [VOLUME_NAME]
```

- `Mountpoint`라는 속성을 통해 실제로 데이터가 저장되는 호스트 머신 상의 경로를 확인할 수 있다!
- 즉, `Docker`가 생성한 `volume`이 있는 곳을 알려주는 것이다!
- 그러나, 이 경로는 우리의 파일 시스템에서 찾을 수 있는 경로가 아니다.
- 실제로 이 경로는 우리 시스템 상에 `Docker`가 설정한 작은 가상 머신 내부에 있기 때문이다.
- 즉, 호스트 머신 파일 시스템에 있는 실제 경로가 아니라는 점에 주의!
- 호스트 머신에서는 저 경로가 또 다르게 맵핑되기 때문에 찾기 어려운 것이다.

```
docker volume rm [VOLUME_NAME]
```
- 위 명령어를 통해 `volume`의 삭제도 가능하다.
- 물론, 사용 중인 `volume`은 삭제할 수 없다!
- 또한, `volume`을 삭제하면 거기에 저장되어 있던 데이터가 사라진다는 점을 기억하자.

```
docker volume prune
```
- 위 명령어를 통해 사용하지 않는 `volume`을 일괄적으로 삭제할 수 있다.

## "COPY" vs "Bind Mounts"

```dockerfile
FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

VOLUME [ "/app/feedback" ]

CMD [ "node", "server.js" ]
```

- 앞서, `Bind Mounts`를 통해 로컬의 소스를 사용하여 변경을 실시간으로 반영할 수 있었다.
- 그런데, `Dockerfile`을 보면 `COPY`가 있다.
- 로컬의 소스 코드를 사용하게 만들었으므로 굳이 `COPY`를 해줄 필요가 없다고 생각되는 부분이다.
- 그러면 `COPY`를 지울 수 있을까?
- 그렇다.