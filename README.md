# setup_mojo
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

#### Locally
- 기본 파이썬 환경을 따른다. 
- 그런데 기본 파이썬 대신 특화된 환경을 쓰고 싶다면?

##### Conda 환경 
https://github.com/modularml/mojo/issues/1085#issuecomment-1771403719

- 여기서 바꿀 것은 `$CONDA_PREFIX/lib` 대목이다. 
    - 예를 들어 콘다를 macos 실리콘 버전의 brew에 설치했다면 brew 아래 콘다가 깔여 있을 것이다. 따라서 찾아야 하는 기본 디렉토리는 `/opt/homebrew/Caskroom/` 아래 miniconda가 설치된다. 

```bash
> export MOJO_PYTHON_LIBRARY="$(find /opt/homebrew/Caskroom/miniconda/base/lib -iname 'libpython*.[s,d]*' | sort -r | head -n 1)"
> echo "export MOJO_PYTHON_LIBRARY=$MOJO_PYTHON_LIBRARY" >> ~/.zshrc
```

- `.zshrc`에 마지막 줄에 해당 내용이 잘 들어 있는지 확인해보자. 

##### 핵심은 `dylib` 혹은 `so` 파일의 위치 

- 위의 내용을 기계적으로 따라할 필요는 없다. 핵심은 `MOJO_PYTHON_LIBRARY`에 파이썬 라이브러리가 있는 경로를 넣는 것이다.
- 직접 찾아도 되겠다. 파이썬이 설치된 경로 아래 `lib`를 찾아서 그 폴더 안에 ` libpython3.11.so` 혹은 `libpython3.11.dylib` 파일이 있는지 확인한다. 해당 파일이 있는 위치의 절대 경로를 파악해서 이 녀석을 직접 `.zshrc`에 심어주면 된다. 

##### pixi 환경 
- 사용하고 싶은 pixi 환경을 `MOJO_PYTHON_LIBRARY`에 넣어주면 그만이다. 
- 일례로 macos에서 pixi를 이용해 다음과 같이 주소를 설정했다. 
    - pixi 환경은 `~/mojo_python` 아래 깔려 있다. 절대 경로는 `/Users/anari/mojo_python`이다. 
    - `~/.zshrc` 마지막 줄은 다음과 같다. 

```zshrc
echo "export MOJO_PYTHON_LIBRARY=/Users/anari/mojopython/.pixi/env/lib/libpython3.11.dylib" > ~/.zshrc # for macos 

echo "export MOJO_PYTHON_LIBRARY=/home/anari/mojo_python/.pixi/env/lib/libpython3.11.so" >> ~/.zshrc # for linux
```