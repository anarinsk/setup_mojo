# test_mojo
mojo를 올려보자. 

## Install

### First 

- 플랫폼 별도 다르다. 리눅스는 아래와 같다. 깃헙코스에서 아래와 같이 설치하면 된다. 

```bash
curl https://get.modular.com | sh - && \
modular auth mut_c4e7186420c84b86b1f7b5219b4028d9 # modular CLI
modular install mojo
```

### Update 

```bash
sudo apt-get update && \
sudo apt-get install modular && \
modular clean && \
modular install mojo
```

## User Cases 

### Jupyter 

- kernel에서 jupyter server > mojo를 선택하면 된다. 
- 세팅이 완료되면 pip를 사용할 수 있다. 

## Trouble Shooting

### VS Code Extension 

<https://marketplace.visualstudio.com/items?itemName=modular-mojotools.vscode-mojo>

이 확장은 헬퍼, 디버거다. 실행에는 지장이 없다. Home 환경이 없다고 뜨면 Path를 넣어야 한다. 
`~/.modular` 환경의 절대 경로를 입력하자. 해당 폴더에서 `pwd`를 치면 절대 경로를 알 수 있다. 그런데 계속 뭔가 뜬다... 

### Python 백엔드 지정 

이거 골치 아프다. 구별해서 살펴보자. 

#### Github Codespaces 

- 이거 자체가 컨테이너이기 때문에 그냥 쓰면 된다. 기본 파이썬 환경에 어느 정도 기본 패키지가 설치된 것으로 보인다. 

#### With Local 

- 기본 파이썬을 세팅한다. 
- 그런데 기본 파이썬을 세팅하기가 싫다! 

##### Conda 환경 

https://github.com/modularml/mojo/issues/1085#issuecomment-1771403719

- 여기서 바꿀 것은 `$CONDA_PREFIX/lib` 대목이다. 
    - 예를 들어 콘다를 brew에 설치했다면 brew 아래 콘다가 깔여 있을 것이다. 

```bash
> export MOJO_PYTHON_LIBRARY="$(find /opt/homebrew/Caskroom/miniconda/base/lib -iname 'libpython*.[s,d]*' | sort -r | head -n 1)"
> echo "export MOJO_PYTHON_LIBRARY=$MOJO_PYTHON_LIBRARY" >> ~/.zshrc
```

- `.zshrc`에 들어 있는지 확인해보자. 
- 위 방식은 파이썬 호출 모듈의 주소를 변수로 저장하고 이를 `MOJO_PYTHON_LIBRARY`로 저장한다. 
- macos에서 pixi를 이용해 다음과 같이 주소를 설정했다. `~/.zshrc` 마지막 줄은 다음과 같다. 

```zshrc
export MOJO_PYTHON_LIBRARY=/Users/anari/mojopython/.pixi/env//lib/libpython3.11.dylib
```
