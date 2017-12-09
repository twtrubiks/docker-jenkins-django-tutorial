# docker-jenkins-django-tutorial

實戰 Docker + Jenkins + Django + Postgres  📝

這篇文章主要延續之前的教學文，建議對 Docker 以及 Django 有基礎的認識，可參考

[Docker 基本教學 - 從無到有 Docker-Beginners-Guide](https://github.com/twtrubiks/docker-tutorial)

[Django-REST-framework 基本教學 - 從無到有 DRF-Beginners-Guide 📝](https://github.com/twtrubiks/django-rest-framework-tutorial)

* [Youtube Tutorial PART 1 - CI ( Continuous Integration ) / CD (Continuous Delivery / Continuous Deployment) 介紹](xx)
* [Youtube Tutorial PART 2 - Docker + Jenkins + Django + Postgres 設定](xx)
* [Youtube Tutorial PART 3 - Jenkins 基本設定](xx)
* [Youtube Tutorial PART 4 - Jenkins + GitHub Integration 實戰](xx)
* [Youtube Tutorial PART 5 - Jenkins + GitHub Webhooks 實戰](xx)
* [Youtube Tutorial PART 6 - Jenkins + BitBucket private repo 實戰](xx)
* [Youtube Tutorial PART 7 - Jenkins + Notifications - Send Email 實戰](xx)
* [Youtube Tutorial PART 8 - Jenkins + Slack 實戰](xx)

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
          build: ./jenkins
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
          build: ./jenkins
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

直接瀏覽 [http://0.0.0.0:8080/](http://0.0.0.0:8080/)，會看到下圖

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

執行完畢之後，我們就可以到 [http://0.0.0.0:8002/api/music/](http://0.0.0.0:8002/api/music/) 瀏覽。

![](https://i.imgur.com/lzsHjlL.png)

也可以設定遠端觸發建置

![](https://i.imgur.com/Krn70Vi.png)

也就是設定一組 token，例如 twtrubiks，

當我輸入網址 [xx](xx) 就會自動開始 build 。

### Jenkins + GitHub Webhooks 實戰

聰明的你現在一定會問，這樣我每次 push 完，都要自己再去 jenkins build ，有夠麻煩 :expressionless:

能不能 push 之後，透過 Webhooks 讓 jenkins 偵測到，然後自動 build 呢 :question:

當然可以 :flushed:

由於需要 https ， 所以我們透過 ngrok 幫助我們完成，可參考 [如何使用 ngrok](https://github.com/twtrubiks/facebook-messenger-bot-tutorial#%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8-ngrok)

> ./ngrok http 8080

port 設定 8080 是因為 jenkins 的 port 為 8080

![](https://i.imgur.com/MYv5Z59.png)

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

找到 Global Slack Notifier Settings，將 **Base URL**、**Integration Token**、**Chnnel** 貼上去，

Integration Token 也有兩種方式設定

第一種

![](https://i.imgur.com/dxHZk1S.png)

第二種 Integration Token Credential ID ( 使用這個安全一點:relaxed: )

Add Credential

![](https://i.imgur.com/f3q4kzC.png)

![](https://i.imgur.com/HR4BMFv.png)

可以點 Test Connection 測試看看是否設定正確，如果正確會顯示 Success :grin:

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

## 後記：

這次和大家介紹 Jenkins，相信大家一定覺得 Jenkins 超棒 :heart_eyes: ，我也只介紹比較基本的功能，如果要全部介紹完，

要花好多時間 ( 默默研究中:sweat_smile: )。如果你是沒有導入CI 的團隊，我會建議先導入幾個重要的部份就好，整個流程不一

定要完全的複製到你們的團隊上，可以一小部分一小部分慢慢導入，這樣整個團隊的反彈也不會那麼大。

最後大家可以朝下面這張圖的目標前進

![](https://i.imgur.com/dCASBwT.png)

圖片來源 [https://chrisshayan.atlassian.net/wiki/spaces/my/blog/2013/07/23/4227074/Continuous+Delivery+Matrix](https://chrisshayan.atlassian.net/wiki/spaces/my/blog/2013/07/23/4227074/Continuous+Delivery+Matrix)

## 執行環境

* Mac
* Python 3.6.2
* windows 10

## Reference

* [https://docs.docker.com/](https://docs.docker.com/)
* [docker jenkins](https://hub.docker.com/_/jenkins/)

## License

MIT license
