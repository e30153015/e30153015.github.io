---
title: Hexo+Github setting
date: 2019-08-28 12:23:01
tags: Hexo
categories: Hexo
---

## 建製Hexo

### 環境配置
* 安裝 [Git](https://git-scm.com/)
* 安裝 [Node.js](https://nodejs.org/en/download/)
  * 檢測Node是否安裝成功
  ``` bash
    $ node -v
  ```
<!-- more -->
### 安裝Hexo
``` bash
$ npm install -g hexo-cli
```
安裝成功後，輸入以下指令可查看版本
``` bash
$ hexo version
```
### 建立Hexo資料夾
``` bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```
完成後的檔案結構如下
![](/images/init-folder.jpg)

`_config.yml`：網站 配置 檔案，可以在此配置大部分的設定。
`package.json`：套件版本控制
`scaffolds`：鷹架 資料夾。建立新文章時，會根據 scaffold 來建立檔案。
`source`：原始檔案資料夾是放置內容的地方。
`themes`：主題 資料夾。Hexo 會根據主題來產生靜態檔案。

## Hexo常用指令

### 創建文章
```bash
$ hexo new [layout] <title> 
```
### 刪除文章
進入到 source / _post 資料夾中，xxx.md 文件，在本地直接執行刪除。
### Clean
刪除緩存文件`db.json`以及生成的public目錄，當你修改了某些樣式或者配置時，如果發現`hexo g`後也沒有反應，就可以執行一下這個命令。
```bash
$ hexo clean
```
### 佈署
```bash
hexo generate # (hexo g) 產生靜態檔案，會在目錄下產生public 資料夾。
hexo server # (hexo s) 啟動伺服器，預設是 http://localhost:4000/。
hexo deploy # (hexo d) 部署網站。（ 比如 github, heroku 等平台 ）
```
#### 常用組合
```bash
$ hexo d -g # 產生靜態檔後部署
$ hexo s -g # 產生靜態檔後預覽
```
### 移除foot版權宣告
開啟`theme/next/_config.yml` 
將`powered`及`theme`的`enable`修改為`false`即可
## 更換主題Next
Hexo 預設的主題是landscape，其中最熱門的為NextT

安裝NexT
```bash
$ git clone https://github.com/theme-next/hexo-theme-next.git themes/next
```
修改網站設定_config.yml
```bash
theme: next
```
重新啟動server
```bash
$ hexo server
```
Scheme 設定
在NexT 中有多種Scheme 可選擇，其中的預設主題為Muse。大部份看到蠻多人選擇使用Pisces當主題。
在主題設定theme/next/_config.yml裡找到schema，把註解去掉即可開啟。
```bash
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```
## 佈署至GitHub
在GitHub上新增GitHub Pages
1. 在GitHub 創建一個名稱為\<username>\.github.io 的專案
2. 安装 deploy git 插件
```bash
npm install hexo-deployer-git --save
```

3. 在主題配置文件 _config.yml 中修改 repository 名稱
```bash
deploy:
  type: git
  repo: https://github.com/<userName>/<userName>.github.io.git
  branch: master
```
4. 執行 hexo d 即可發佈到 GitHub 上
接下來就可以到你的網站\<username>\.github.io

## 不同設備同步方式
clone git資料庫並切換branch至`hexo`
```bash
git clone -b hexo [email protected].com:yourname/yourname.github.io.git  //將Github中hexo分支clone到本地
cd  yourname.github.io  //切換到剛剛clone的資料夾內
npm install    //注意，這裡一定要切換到剛剛clone的資料夾內執行，安裝必要的所需元件，不用再init
hexo new post "new blog name"   //新建一個.md檔案，並編輯完成自己的部落格內容
git add source  //經測試每次只要更新sorcerer中的檔案到Github中即可，因為只是新建了一篇新部落格
git commit -m "XX"
git push origin hexo  //更新分支
hexo d -g   //push更新完分支之後將自己寫的部落格對接到自己搭的部落格網站上，同時同步了Github中的master
```