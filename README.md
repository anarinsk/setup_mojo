# test_mojo
mojo를 올려보자. 

## Install

### First 

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
`~/.modular` 환경의 절대 경로를 입력하자. 해당 폴더에서 `pwd`를 치면 절대 경로를 알 수 있다. 