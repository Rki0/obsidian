# Relation between image and container

- JS로 따지자면 `image`는 `class`에 작성 코드이고, `container`는 그를 사용하여 만든 `instance`이다.
- 즉, `image`는 소스 코드, 환경 설정(e.g. node 백엔드 코드 및 환경 설정) 등이 들어 있으며, `container`는 그 것들을 실행하는 것이다.
- 즉, `image`를 기반으로 `container`를 실행하는 것!
- 또한, 하나의 `image`로 여러 개의 `container`를 만들 수 있다는 것도 알 수 있다.

# Dockerfile

- `Docker`의 설정을 위해 사용하는 파일이다.
- 이 파일은 `image`에 대한 설정을 하기 위한 것임을 잊어서는 안된다!
- 즉, 이 파일에 적히는 코드는 `image`가 빌드될 때 실행된다는 것이므로, 불필요한 코드를 작성해서 `image`가 빌드 될 때마다 실행되는 불상사가 없도록 하자(e.g. `RUN node sever.js`를 작성해서 `image` 빌드마다 node 서버 코드를 실행히버리는 일. 이는 `container`를 실행하는 경우에 실행할 코드이기 때문.)
- 또한, `container`는 생성하는 것만으로는 그 포트가 외부와 연결되지 않기 때문에 고립된 상태로 존재하는데, 이를 외부와 연결하여 다른 사람들이 접근할 수 있도록 하기 위해 외부 포트와 연결해줘야한다.

```
docker run -p 3000:[port number of image] [container ID]
```

- `image`는 읽기 전용이다! 코드에 변경이 있더라도 이를 `DOc`