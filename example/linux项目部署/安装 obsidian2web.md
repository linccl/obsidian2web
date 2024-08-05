## 0. 提前[[安装 git]]
## 1. clone 仓库
```bash
git clone https://github.com/chenxuan520/obsidian2web
```
## 2. 运行 `init.sh` 
下载mdbook和 obsidian-export 这一步默认下载linux版本的,如果需要其他系统版本的,请自行下载
```bash
init.sh
```
## 3. 运行 `create.sh <你的仓库的路径>`
此时生成的book文件夹就是需要的静态文件,你可以自己通过自己的服务器进行部署
```bash
create.sh 
```