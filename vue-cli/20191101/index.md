#   环境变量和模式
####  &emsp;&emsp;可以替换项目根目录中的文件来修改环境变量
```go
.env        //  在所有的环境中被载入
.env.local  //  在所有的环境中被载入，但是会被git忽略
.env.[mode] //  只在指定的环境中被载入
.env.[mode].local   //  只在指定的环境中被载入，但是会被git忽略
```
####  &emsp;&emsp;一个环境文件中只包含环境变量的键值对: 键=值
```go
VUE_APP_ENV=pro
```
####  &emsp;&emsp;一个为指定环境准备的文件优先级要比一般的环境文件优先级高，如 .env.pro 的优先级比 .env的优先级高

## 1. 模式
#### &emsp;&emsp;假设在package.json中我们这样定义
```go
"build": "npm run build:dll && vue-cli-service build  --mode staging"
```
#### &emsp;&emsp;这样项目就在 staging 模式下，根据这个模式，项目会去加载可能存在的 .env、.env.staging和.env.stagin.local文件，然后构建出生产环境应用
## 2. 变量
#### &emsp;&emsp;如果此时在.env.staging 文件中如下定义
```go
VUE_APP_TITLE=My App
```
#### &emsp;&emsp;那么就可以在客户端代码中使用这个环境变量，注意，必须以VUE_APP_开头
```javascript
console.log(process.env.VUE_APP_TITLE)
```