---
title: "[Windows] Chapter01. 깃허브 블로그 만들기"
excerpt: "깃허브 블로그 만드는 방법"

categories:
 - Blog
tags:
 - [Blog, jekyll, Github, Git, Ruby]
permalink: categories/Blog

toc: true
toc_sticky: true

date: 2022-11-16
last_modified_at: 2022-11-16
---

<!-- outline-start -->

### 00. 시작하기 전에

   ```yaml
   git, ruby, jekyll 설치가 필요하다. 특히 Ruby는 윈도우 필수로 설치해야 됨!
   ```

자, 그럼 이제 설치 해 보자.


#### `git` 설치
 git은 다 설치가 되어 있을거라 생각하고 pass.



#### `ruby` 설치

 - [Downloads](https://rubyinstaller.org/)


 - `WITH DEVKIT` 에 있는 `(x86)` 중 마음에 드는 것을 다운 받는다.

  ![ruby 1](https://user-images.githubusercontent.com/117553252/202195331-b0fea8c8-4e8c-4c17-92f0-3169cd6fc377.png){: width=”50%“ height=”50%“}{: .center}



 - 둘 다 체크

  ![ruby 2](https://user-images.githubusercontent.com/117553252/202196131-d7cacdfb-99a4-47af-9608-6ef56eadb233.png){: width=”50%“ height=”50%“}{: .center}



 - 마지막에 체크하여 Ruby를 실행시켜 준다.

  ![ruby 3](https://user-images.githubusercontent.com/117553252/202196210-97b269d2-d837-4715-9e7b-3ad6797708c5.png){: width=”50%“ height=”50%“}{: .center}



 - 이 화면이 나오면 `1,2,3` 을 입력하고 Enter 한다.

  ![ruby 4](https://user-images.githubusercontent.com/117553252/202196266-94f34ea2-ada5-44f2-a994-ee31390ab9b4.png){: width=”70%“ height=”70%“}{: .center}



 - 윈도우의 cmd 창을 열어 ruby가 잘 설치 됐는지 확인한다.
 

   ```yaml
   ruby --version
   ```

 ![ruby 5](https://user-images.githubusercontent.com/117553252/202198332-abb2ac3c-967b-420f-b819-c4545c53dcfb.png){: width=”100%“ height=”100%“}{: .center}




#### `jekyll` 설치

- 윈도우 cmd 창에서 jekyll를 설치한다.


   ```yaml
    gem install jekyll bundle
   ```

 ![jekyll 1](https://user-images.githubusercontent.com/117553252/202201371-dfd3e491-1150-48ee-ad98-ad21941841b3.png){: width=”70%“ height=”100%“}{: .center}



- jekyll 가 잘 설치되었는지 cmd에서 확인하자.


   ```yaml
    jekyll -version
   ```

 ![jekyll 2](https://user-images.githubusercontent.com/117553252/202201887-2e1e3e5b-eb54-446c-bb8a-6412f780efa6.png){: width=”100%“ height=”100%“}{: .center}





### 01. Repository 만들기.

- New respository를 눌러 새 Respository를 만든다.

-Respository name 은 본인의아이디.github.io 형식으로 적는다.

   ```yaml
   Hangyeryeong.github.io
   ```

- Public, Add a README file 을 체크하고 Create 를 누르면 끝!

### 02. Clone 해 주자.

- 윈도우 cmd 창을 열어 git clone HTTPS주소 를 적는다.

   ```yaml
   git clone https://github.com/Hangyeryeong/Hangyeryeong.github.io.git
   ```

### 03. index.html 파일을 생성하자.

- 윈도우 cmd 창을 열어 우선 자신의 깃허브 블로그 파일로 이동한다.

   ```yaml
   cd Hangyeryeong.github.io
   ```

- echo 명령어를 사용하여 파일을 생성하자.

   ```yaml
   echo "Hello World" > index.html
   ```

- 해당 폴더에 들어가서 파일이 생성 되었는 지 확인한다.

### 04. 업로드 하기.

- 윈도우 cmd 창에서(자신의 깃허브 블로그 파일에서) 업로드를 해 준다.
다음과 같이 명령어를 치면 된다.

   ```yaml
   git add *
   git commit -m "Start Page"
   git push -u origin main
   ```


- 이때, commit 명령어를 쳤는데 git config 하라고 나오면 그대로 해주면 된다.
- 나의 경우,
  
   ```yaml
   git config --global user.email "https.sooe13@gmail.com"
   git config --global user.name "Hangyeryeong"
   ```


- config 하고 그 다음에 다시 commit부터 하면 순서대로 하면 된다.

### 05. 확인하기.

- 본인의 깃허브 블로그 주소에 들어가보자.

   ```yaml
   Hangyeryeong.github.io
   ```


- 그럼 페이지가 생성이 되었을 것이다.