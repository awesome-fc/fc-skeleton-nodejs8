
# {{ projectName }}

## 准备

1. 安装 node

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.5/install.sh | bash
nvm install 8
```

2. 安装 yarn

```bash
npm install -g yarn
```

## 安装依赖

```bash
yarn install
```

## 编译

```bash
yarn build
```

## 本地运行函数

```bash
fun local invoke {{ projectName }}/{{ projectName }}
```

## 部署函数到云端

```bash
fun deploy
```