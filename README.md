# docker-jenkins-django-tutorial

實戰 Docker + Jenkins + Django + Postgres  📝

這篇文章主要延續之前的教學文，建議對 Docker 以及 Django 有基礎的認識，可參考

[Docker 基本教學 - 從無到有 Docker-Beginners-Guide](https://github.com/twtrubiks/docker-tutorial)

[Django-REST-framework 基本教學 - 從無到有 DRF-Beginners-Guide 📝](https://github.com/twtrubiks/django-rest-framework-tutorial)

* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#ci--cd-%E4%BB%8B%E7%B4%B9) - [Youtube Tutorial PART 1 - CI ( Continuous Integration ) / CD (Continuous Delivery / Continuous Deployment) 介紹](https://youtu.be/wJlE0aFluY4)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#docker--jenkins--django--postgres-%E8%A8%AD%E5%AE%9A) - [Youtube Tutorial PART 2 - Docker + Jenkins + Django + Postgres 設定](https://youtu.be/fjwIVCywX2A)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#docker--jenkins--django--postgres-%E5%AF%A6%E6%88%B0%E6%95%99%E5%AD%B8) - [Youtube Tutorial PART 3 - Jenkins 基本設定](https://youtu.be/27rmiKGrG2M)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#jenkins--github-integration-plugin) - [Youtube Tutorial PART 4 - Jenkins + GitHub Integration 實戰](https://youtu.be/AYgw5NXAeNY)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#jenkins--github-webhooks-%E5%AF%A6%E6%88%B0) - [Youtube Tutorial PART 5 - Jenkins + GitHub Webhooks 實戰](https://youtu.be/ymfTEPxKRqQ)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#jenkins--bitbucket-private-repo-%E5%AF%A6%E6%88%B0) - [Youtube Tutorial PART 6 - Jenkins + BitBucket private repo 實戰](https://youtu.be/S6Hfcm_xrnE)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#jenkins--notifications---send-email) - [Youtube Tutorial PART 7 - Jenkins + Notifications - Send Email 實戰](https://youtu.be/MWWBleOtqVk)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#jenkins--slack) - [Youtube Tutorial PART 8 - Jenkins + Slack 實戰](https://youtu.be/jmVRb81KpUk)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#job-chaining-in-jenkins) - [Youtube Tutorial PART 9 - Jenkins Job chaining tutorial](https://youtu.be/FOhxViut4cI)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#delivery-pipeline-plugin) - [Youtube Tutorial PART 10 - Jenkins + Delivery Pipeline tutorial](https://youtu.be/kBAAtMOclv8)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#build-pipeline-plugin) - [Youtube Tutorial PART 11 - Jenkins + Build Pipeline tutorial](https://youtu.be/Dk4busLipS0)
* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#remote-access-api) - [Youtube Tutorial PART 12 - Jenkins +  Remote access API tutorial](https://youtu.be/p7uxurX4MnI)

* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#csrf-protection) - CSRF Protection

* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#ssh-plugin---ssh-remote-hosts) - SSH Plugin - SSH remote hosts

* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#%E8%A8%AD%E5%AE%9A%E6%99%82%E5%8D%80) - 設定時區

* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial#%E8%A8%AD%E5%AE%9A%E6%AC%8A%E9%99%90-security) - 設定權限 Security

* [目錄](https://github.com/twtrubiks/docker-jenkins-django-tutorial/tree/master/jenkins_nginx) - jenkins 搭配 nginx

## CI / CD 介紹

在開始介紹前，先帶大家了解幾個名詞，相信大家一定常常聽到別人說 CI / CD，

CI : Continuous Integration ， 又稱 持續整合。

CD : Continuous Delivery / Continuous Deployment ， 又稱 持續交付 / 持續佈署。

![](https://i.imgur.com/s1hwUjr.jpg)

圖片來源 [http://www.code-maze.com/wp-content/uploads/2016/02/ci-4-1024x584.png](http://www.code-maze.com/wp-content/uploads/2016/02/ci-4-1024x584.png)

詳細的定義在這裡我就不另外做介紹，培養大家 google 的能力 :smiley:

### CI / CD 的好處

CI 對團隊來講有非常多的好處，一個良好的 CI 能加速整個團隊的工作流程，而且 developer 不用擔心

push 後整個專案可能會掛掉。也因為持續的佈署與測試，可以在早期就發現程式的漏洞（ 盡早修正 bug

，越晚發現付出的代價越高 :scream: ）。

透過 CI，可以為團隊帶來更好的溝通，也可以更容易的進行 Code Review ，提升整個工作團隊的效率，

減少不必要的手動流程 ( 自動化 )，developer 可以更專注的在開發程式。

CI 不只對 developer 有好處，也對管理者有好處，可以隨時監控目前整個團隊的工作流程以及品質。

### 該選擇哪一款 CI / CD 呢

因為這類工具很多，在這裡我挑幾個

[Jenkins](https://jenkins-ci.org/)

![](https://i.imgur.com/lQefqzU.jpg)

[GitLab CI](https://docs.gitlab.com/)

![](https://i.imgur.com/ThRu34A.jpg)

[Travis CI](https://travis-ci.org/)

![](https://i.imgur.com/vWiOoH4.jpg)

[Drone](https://github.com/drone/drone)

![](https://i.imgur.com/o34VAYY.jpg)

該選擇哪一個呢 ？

基本上，還是要依照自己的的需求、技術、工作流程來選擇最適合你們團隊的。

但這篇文章的主角是 Jenkins :smile:

## Docker + Jenkins + Django + Postgres 設定

使用 [Docker 基本教學 - 從無到有 Docker-Beginners-Guide](https://github.com/twtrubiks/docker-tutorial) 當作範例，

主要是 docker-compose.yml 裡面的設定，有兩種設定方式，

一種是將路徑同步到你的 host 本機，另一種是 Named volume。

可參考官網 volumes 設定方式 [docker-short-syntax](https://docs.docker.com/compose/compose-file/#short-syntax-3)

方法一： 同步到你的 host 本機

可參考 [jenkins_host](https://github.com/twtrubiks/docker-jenkins-django-tutorial/tree/master/jenkins_host)

[docker-compose.yml](https://github.com/twtrubiks/docker-jenkins-django-tutorial/blob/master/jenkins_host/docker-compose.yml)

```yml
version: '3'
services:

    db:
      image: postgres
      environment:
        POSTGRES_PASSWORD: password123
      ports:
        - "5432:5432"
      volumes:
        - pgdata_jenkins_host:/var/lib/postgresql/data/

    api:
      build: ./api
      restart: always
      command: python manage.py runserver 0.0.0.0:8000
      ports:
        - "8002:8000"
      volumes:
        - ./workspace/workspace/demo/api:/docker_api
      depends_on:
        - db

    jenkins:
          image: jenkins/jenkins:lts-jdk11
          restart: always
          ports:
              - "8080:8080"
              - "50000:50000"
          volumes:
              - ./workspace:/var/jenkins_home
volumes:
    pgdata_jenkins_host:
```

方法二： Named volume

可參考 [jenkins_volume](https://github.com/twtrubiks/docker-jenkins-django-tutorial/tree/master/jenkins_volume)

[docker-compose.yml](https://github.com/twtrubiks/docker-jenkins-django-tutorial/blob/master/jenkins_volume/docker-compose.yml)

```yml
version: '3'
services:

    db:
      image: postgres
      environment:
        POSTGRES_PASSWORD: password123
      ports:
        - "5432:5432"
      volumes:
        - pgdata_jenkins:/var/lib/postgresql/data/

    api:
      build: ./api
      restart: always
      command: python /var/jenkins_home/workspace/demo/api/manage.py runserver 0.0.0.0:8000
      ports:
        - "8002:8000"
      volumes:
        - api_data:/docker_api
        - jenkins_data:/var/jenkins_home
      depends_on:
        - db

    jenkins:
          image: jenkins/jenkins:lts-jdk11
          restart: always
          ports:
              - "8080:8080"
              - "50000:50000"
          volumes:
              - jenkins_data:/var/jenkins_home
volumes:
    api_data:
    jenkins_data:
    pgdata_jenkins:
```

## Docker + Jenkins + Django + Postgres 實戰教學

一樣一行指令啟動:relaxed:

> docker-compose up

直接瀏覽 [http://localhost:8080/](http://localhost:8080/)，會看到下圖

![](https://i.imgur.com/nMXQc7Z.jpg)

進入 jenkins container

> cat /var/jenkins_home/secrets/initialAdminPassword

![](https://i.imgur.com/6LNxk8F.png)

將得到的 password 貼到 Administrator password 即可，

再來是安裝套件，請依照自己的需求選擇套件，也可以都不要裝，

需要的時候再補裝套件:smirk:

![](https://i.imgur.com/34d8NHY.jpg)

依照你選擇的套件決定安裝速度

![](https://i.imgur.com/j5W5uKh.jpg)

設定你的帳號密碼

![](https://i.imgur.com/cnV0DYb.jpg)

接著我們就可以開始了

![](https://i.imgur.com/03Lzw5i.jpg)

### Jenkins + GitHub Integration Plugin

整合 Jenkins + GitHub :satisfied:

先點選左側的 管理 Jenkins

![](https://i.imgur.com/G110omM.jpg)

選擇 管理外掛程式

![](https://i.imgur.com/qkPBoJx.jpg)

接著搜尋 GitHub Integration Plugin，如果你已經安裝則會出現在已安裝那邊

![](https://i.imgur.com/j2pgRDP.png)

安裝完成後，點選左側的新增作業

![](https://i.imgur.com/Vaksz9s.jpg)

建立一個專案稱為 demo

![](https://i.imgur.com/xb6mFoA.jpg)

描述可以輸入對這專案的說明

![](https://i.imgur.com/MKRrIti.jpg)

選擇 Git ， 這邊使用 **public** 的 REPO，

[Docker 基本教學 - 從無到有 Docker-Beginners-Guide 教你用 Docker 建立 Django + PostgreSQL](https://github.com/twtrubiks/docker-tutorial)

( 後面會說明如何使用 **private** 的 REPO )

![](https://i.imgur.com/qqXx7j9.jpg)

設定完畢之後，選擇馬上建置，你會發現他開始 build 了 :grin:

![](https://i.imgur.com/xewlYwA.jpg)

接著你可以到工作區看，你會發現已經把剛剛的專案順利 clone 下來了:+1:

![](https://i.imgur.com/ARSYVvm.jpg)

接著建議將 api 重新啟動，然後進入 api container 執行必要的指令

```cmd
python manage.py makemigrations musics
python manage.py migrate
python manage.py createsuperuser
```

![](https://i.imgur.com/ph7o3pU.png)

![](https://i.imgur.com/jZXjdjC.png)

執行完畢之後，我們就可以到 [http://localhost:8002/api/music/](http://localhost:8002/api/music/) 瀏覽。

![](https://i.imgur.com/lzsHjlL.png)

也可以設定遠端觸發建置

![](https://i.imgur.com/Krn70Vi.png)

也就是設定一組 token，例如 twtrubiks，

當我輸入網址 [http://localhost:8080/job/demo/build?token=twtrubiks](http://localhost:8080/job/demo/build?token=twtrubiks) 就會自動開始 build 。

### Jenkins + GitHub Webhooks 實戰

聰明的你現在一定會問，這樣我每次 push 完，都要自己再去 jenkins build ，有夠麻煩 :expressionless:

能不能 push 之後，透過 Webhooks 讓 jenkins 偵測到，然後自動 build 呢 :question:

當然可以 :flushed:

由於需要 https ， 所以我們透過 ngrok 幫助我們完成，可參考 [如何使用 ngrok](https://github.com/twtrubiks/facebook-messenger-bot-tutorial#%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8-ngrok)

> ./ngrok http 8080

port 設定 8080 是因為 jenkins 的 port 為 8080

![](https://i.imgur.com/MYv5Z59.png)

點選組態

![](https://i.imgur.com/vbQrNkF.png)

找到建置觸發程序，並選擇 GitHub hook trigger for GITScm polling

![](https://i.imgur.com/NXtvmVZ.png)

接著到 [Docker 基本教學 - 從無到有 Docker-Beginners-Guide 教你用 Docker 建立 Django + PostgreSQL](https://github.com/twtrubiks/docker-tutorial)

專案中設定 Webhooks ( 你可以直接 fork 這個專案去改 )

![](https://i.imgur.com/xwtkmOO.png)

Payload URL 就是填入 ngrok 幫你產生的 https 的網址，記得要在網址後面補上 `github-webhook`，

例如範例的 [https://65daeaf5.ngrok.io/github-webhook/](https://65daeaf5.ngrok.io/github-webhook/ )

![](https://i.imgur.com/zGLgLlh.png)

如果設定正確，會有一個綠色的小勾勾

![](https://i.imgur.com/sbZwq9n.png)

這時候，你可以對  [Docker 基本教學 - 從無到有 Docker-Beginners-Guide 教你用 Docker 建立 Django + PostgreSQL](https://github.com/twtrubiks/docker-tutorial)

專案 commit 後再 push，你會發現 jenkins 自動開始 build 了 :satisfied:

![](https://i.imgur.com/kkLJ6nr.png)

也可以點選左側的 GitHub Hook Log 觀看紀錄

![](https://i.imgur.com/wJilA17.png)

### Jenkins + BitBucket private repo 實戰

如果你是使用 **private** 的 REPO， 需要多設定 SSH KEY ，這裡使用 BitBucket 當作範例。

首先，進入你的 jenkins container 產生 ssh key ，可參考 [generating-a-new-ssh-key](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#generating-a-new-ssh-key)。

如果還是不懂，也可參考 [Git-Tutorials GIT 基本使用教學](https://github.com/twtrubiks/Git-Tutorials)，或是直接看之前的影片教學

[github 基本教學 - 從無到有](https://www.youtube.com/watch?v=py3n6gF5Y00)，影片教學包含如何產生 SSH key。

![](https://i.imgur.com/THw32js.png)

> cat /var/jenkins_home/.ssh/id_rsa.pub

![](https://i.imgur.com/vbgfzhJ.png)

到你的 BitBucket private repo 的地方設定

![](https://i.imgur.com/eWC0GWz.png)

貼上剛剛的 id_rsa.pub

![](https://i.imgur.com/khM9X0n.png)

如果你貼上一個 private repository，你會發現會有紅色警告

![](https://i.imgur.com/RpfbFJu.png)

這時候有兩種方法 Add Credentials

第一種

![](https://i.imgur.com/h3DE2b7.png)

第二種 ( 安全一點 :stuck_out_tongue: )

![](https://i.imgur.com/5J5ZpGW.png)

如果設定正確，你會發現紅字消失了

![](https://i.imgur.com/bOF7ZsK.png)

### Jenkins + Notifications - Send Email

管理 Jenkins -> 設定系統

![](https://i.imgur.com/lZwWdXS.png)

找到電子郵件通知，並且設定，這邊使用 gmail 當作範例，

測設定完成後你也可以寄測試信看看設定有沒有正確 :relaxed:

![](https://i.imgur.com/4vOsthJ.png)

如果你使用 gmail，然後一直寄不出去，你可以參考 [使用-gmail-寄信---前置作業](https://github.com/twtrubiks/Flask-Mail-example#使用-gmail-寄信---前置作業) 的設定。

接著再回到專案的組態

![](https://i.imgur.com/vbQrNkF.png)

選擇電子郵件通知

![](https://i.imgur.com/gmYMohn.png)

輸入建立失敗時，誰要收到 email 通知

![](https://i.imgur.com/EDGB577.png)

### Jenkins + Slack

這裡使用 [Slack](https://slack.com/) 當作範例 :blush:

之前我也有簡單整合過 [Slack](https://slack.com/)，

可參考[使用Hubot建立屬於自己的機器人 (Build Your Own Robot With Hubot)](https://github.com/twtrubiks/mybot)。

安裝 Slack Notification Plugin

![](https://i.imgur.com/T3apVow.png)

接著到你自己的 [Slack](https://slack.com/) ( 申請步驟這邊不介紹了 )，點選左邊 Channels 的 +

![](https://i.imgur.com/PibIvkJ.png)

建立一個 channel

![](https://i.imgur.com/8vEnSx8.png)

點選右上角齒輪 Add an app

![](https://i.imgur.com/ceEYhCg.png)

搜尋 jenkins

![](https://i.imgur.com/VrsiDJi.png)

Install

![](https://i.imgur.com/zE5MXpQ.png)

接著會有圖文教學教你如何將 Slack 整合進去 jenkins

![](https://i.imgur.com/zayZzKU.png)

要設定的有三個地方，分別為 **Base URL**、**Integration Token**、**Chnnel**

![](https://i.imgur.com/uQmedh6.png)

你的 Post to Channel

![](https://i.imgur.com/aHkZw0C.png)

回到 Jenkins，左側管理 Jenkins，設定系統

![](https://i.imgur.com/LGp8yn0.png)

找到 Slack, 設定 *Workspace* *Credential* *Default channel / member id*

![](https://i.imgur.com/jUmj4c3.png)

點選 Add Credential ( 使用這個安全一點:relaxed: )

![](https://i.imgur.com/f3q4kzC.png)

最後可以點 Test Connection 測試看看是否設定正確，如果正確會顯示 Success :grin:

接著再回到 jenkins 專案裡的組態

![](https://i.imgur.com/8MMxUEa.png)

點選 建置後動作 並選擇 Slack Notifications

![](https://i.imgur.com/djodDCN.png)

依照自己需求通知的做設定

![](https://i.imgur.com/vPy8eyR.png)

接著設定完成，讓我們來試試看 :flushed:

可以手動點選馬上建置或是如果你已經 GitHub Integration Plugin 直接 commit push 一次即可，

回到你的 slack 你會發現成功了 :satisfied:

![](https://i.imgur.com/qLLEDsa.png)

## 軟體版本週期

既然都對 CI / CD 有基本的認識了，那麼你一定要再了解軟體版本週期。這個是什麼呢？我相信

大家一定常常聽導什麼 Alpha Beta 之類的，其實這就是指軟體版本的週期，以下我將簡單介紹他

的週期。

### Alpha

Alpha（ α ）版本基本上就是測試版本，很多功能都未完善，因為是屬於軟體版本週期中的最初階

段。這階段通常會由開發者下去做測試。

### Beta

Beta 版本是最早對外公開的軟體版本，由一般大眾協助測試，通常 Beta 版本包含所有的功能，但

可能會有一些已知的 bug，Beta 版本有時候也會作為測試市場對產品的反應。

### Release Candidate

Release Candidate（ 簡稱 RC ），通常這版本會成為最終產品候選的版本，這階段通常不會有嚴重

的 bug，有些開源軟體會推出 RC2，而 RC2 則會成為正式版本。

### Release to Manufacting

Release to Manufacting（ 簡稱 RTM ），這版本基本上是已經要上線了。

### General availability

General availability（ 簡稱 GA ），這階段軟體基本上已機上線了。

## Job chaining in Jenkins

依照文章最開始的 CI / CD 介紹的圖，使用簡單 Build -> Deploy -> Test 的 workflow，

依照這個 workflow，我將介紹 Jenkins 的 Job chaining

* [Youtube Tutorial PART 9 - Jenkins Job chaining tutorial](https://youtu.be/FOhxViut4cI)

## Build / Delivery Pipeline Plugin

剛剛介紹了 Job chaining 的概念，接下來要推薦大家可以將 Job chaining 視覺化的套件。

### Delivery Pipeline Plugin

* [Youtube Tutorial PART 10 - Jenkins + Delivery Pipeline tutorial](https://youtu.be/kBAAtMOclv8)

[Delivery Pipeline Plugin](https://wiki.jenkins.io/display/JENKINS/Delivery+Pipeline+Plugin)

Delivery Pipeline Plugin 要求 job 要有 downstream/upstream relationships （上下游的關係）。

### Build Pipeline Plugin

* [Youtube Tutorial PART 11 - Jenkins + Build Pipeline tutorial](https://youtu.be/Dk4busLipS0)

[Build Pipeline Plugin](https://wiki.jenkins.io/display/JENKINS/Build+Pipeline+Plugin)

## Remote access API

建議看影片，實戰給大家看會比較有感覺 😁

* [Youtube Tutorial PART 12 - Jenkins +  Remote access API tutorial](https://youtu.be/p7uxurX4MnI)

可參考 [Remote access API](https://wiki.jenkins.io/display/JENKINS/Remote+access+API)。

主要是我們可以透過 terminal 呼叫 API，達到幫我們 build 的功能。

如何取得自己的 User ID 以及 API Token ，請先點選右上角

![](https://i.imgur.com/vEHGxH6.png)

設定 -> API Token

![](https://i.imgur.com/ZgfPxpK.png)

點選顯示 API Token 後，

![](https://i.imgur.com/35Z4rao.png)

溫馨小提醒  :heart:

以下使用 curl 來當做範例，如果你是 windows 或 Linux 用戶，

請自己安裝 curl，這邊就不再做介紹了☺️

官方範例

```cmd
curl -X POST JENKINS_URL/job/JOB_NAME/build \
  --user USER:TOKEN \
  --data-urlencode json='{"parameter": [{"name":"id", "value":"123"}, {"name":"verbosity", "value":"high"}]}'
```

範例，假設 job 為 demo

```cmd
curl -X POST http://localhost:8080/job/demo \
   --user twtrubiks:8d3215553ca9623300f4967827c61291
```

`-d` 參數說明

> -d/--data , Send specified data in POST request.

`--data-urlencode` 參數說明

> --data-urlencode , (HTTP) This posts data, similar to the other -d, --data options with the exception that this performs URL-encoding.

`-u` 參數說明

> -u/--user <user[:password]> , Set user and password

範例

```cmd
curl --user name:password http://www.example.com
curl -u user:password http://www.example.com
```

`-X` 參數說明

> -X/--request The request method to use.

範例

```cmd
curl -X POST http://www.example.com
```

如果在 terminal 中輸入後，什麼都沒發生，就代表成功了（ 但通常應該都會有錯誤😅 ），

如果，你看到以下錯誤

```cmd
....
<title>Error 403 No valid crumb was included in the request</title>
....
```

![](https://i.imgur.com/QYhNler.png)

就代表你有啟動比較安全的機制 CSRF Protection（ 預設是啟動的 ）。

要如何解決呢？ 請繼續往下看😊

### CSRF Protection

主要是防止 CSRF 攻擊，

如果不知道什麼是 CSRF ，可參考我之前寫的 [CSRF-tutorial 📝](https://github.com/twtrubiks/CSRF-tutorial)

管理 Jenkins -> 設定全域安全性

![](https://i.imgur.com/78mW8qh.png)

防範 CSRF 入侵預設有被打勾

![](https://i.imgur.com/auFI5Uy.png)

把打勾取消，就可以用剛剛的方法了。

但是，如果我還是希望打勾防範 CSRF 入侵，那我該怎麼辦呢😬

這時候，必須先得到 Jenkins-Crumb

```cmd
curl -s -u twtrubiks:8d3215553ca9623300f4967827c61291  http://localhost:8080/crumbIssuer/api/json
```

![](https://i.imgur.com/WgrbIKi.png)

`-s` 參數說明

> -s/--silent Silent mode. Don't output anything

然後再將 Jenkins-Crumb 的值帶進去，如下（ 假設 job 為 demo ）

```cmd
curl -X POST http://localhost:8080/job/demo/build --user twtrubiks:8d3215553ca9623300f4967827c61291  -H "Jenkins-Crumb:6fbe69cd42a261330cb37e74af1ed1d1"
```

![](https://i.imgur.com/xwmGGO2.png)

`-H` 參數說明

> -H/--header  Custom header

如果沒跳出任何資訊 ( 有跳訊息通常是有錯誤 )，就代表成功了👍

![](https://i.imgur.com/lqb8HQL.png)

你可以回到你的 Jenkins ，你會發現他自動開始 build 了 :satisfied:

### SSH Plugin - SSH remote hosts

這個 Jenkins Plugin 可以幫助你遠端 ssh 到主機上執行 script,

請到 Manage Jenkins -> Manage Plugins 底下安裝 Plugin

![alt tag](https://i.imgur.com/EDE9SB8.png)

之後到 Manage Jenkins -> System Configuration -> Configure System 底下

設定你的 SSH remote hosts

![alt tag](https://i.imgur.com/sBWXYfB.png)

最後你到一般的 job 裡就會看到 Execute shell script on remote host using ssh

![alt tag](https://i.imgur.com/dI9PD44.png)

順便補充一下, aws ssh key 設定的部份,

Kind 的部份選擇 SSH Username with private key

![alt tag](https://i.imgur.com/79D9SbG.png)

這邊再把你的 key 貼上去

![alt tag](https://i.imgur.com/8r8adqF.png)

### 設定時區

Manage Jenkins -> Tools and Actions -> Script Console

直接執行以下的指令

```cmd
System.setProperty('user.timezone', 'Asia/Taipei')
```

Manage Jenkins -> Status Information -> System Information

![alt tag](https://i.imgur.com/mh5Touq.png)

Manage Jenkins -> Manage Users -> 選擇 user -> User Defined Time Zone

![alt tag](https://i.imgur.com/byNu4JT.png)

### 設定權限 Security

之後到 Manage Jenkins -> Security -> Configure Global Security,

找到 Authorization 之後選擇 Project-based Matrix Authorization Strategy

這個設定是依據專案分權限, 你可以依照自己的需求設定

![alt tag](https://i.imgur.com/aC0lr98.png)

然後因為你是選擇依照專案分權限,

你在 job 中的設定會多出 Enable project-based security,

可選擇是否繼承上面的 Security 設定,

也可以依照 user 設定 (像是這個專案只有誰可以看到這樣)

![alt tag](https://i.imgur.com/y35InG0.png)

## 後記：

這次和大家介紹 Jenkins，相信大家一定覺得 Jenkins 超棒 :heart_eyes: ，我也只介紹比較基本的功能，如果要全部介紹完，

要花好多時間 ( 默默研究中:sweat_smile: )。如果你是沒有導入CI 的團隊，我會建議先導入幾個重要的部份就好，整個流程不一

定要完全的複製到你們的團隊上，可以一小部分一小部分慢慢導入，這樣整個團隊的反彈也不會那麼大。

最後大家可以朝下面這張圖的目標前進

![](https://i.imgur.com/dCASBwT.png)

圖片來源 [https://chrisshayan.atlassian.net/wiki/spaces/my/blog/2013/07/23/4227074/Continuous+Delivery+Matrix](https://chrisshayan.atlassian.net/wiki/spaces/my/blog/2013/07/23/4227074/Continuous+Delivery+Matrix)

## 執行環境

* Linux
* Mac
* Python 3.6.2
* windows 10

## Reference

* [https://docs.docker.com/](https://docs.docker.com/)
* [docker jenkins](https://hub.docker.com/r/jenkins/jenkins)

## Donation

文章都是我自己研究內化後原創，如果有幫助到您，也想鼓勵我的話，歡迎請我喝一杯咖啡:laughing:

![alt tag](https://i.imgur.com/LRct9xa.png)

[贊助者付款](https://payment.opay.tw/Broadcaster/Donate/9E47FDEF85ABE383A0F5FC6A218606F8)

## License

MIT license
