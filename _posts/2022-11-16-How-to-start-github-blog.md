---
title: "[Windows] Chapter01. 깃허브 블로그 만들기"
excerpt: "깃허브 블로그 만드는 방법"

categories:
 - Blog
tags:
 - [Blog, jekyll, Github, Git, Ruby]

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

 ![jekyll 1](https://user-images.githubusercontent.com/117553252/202201371-dfd3e491-1150-48ee-ad98-ad21941841b3.png){: width=”100%“ height=”100%“}{: .center}



- jekyll 가 잘 설치되었는지 cmd에서 확인하자.


   ```yaml
    jekyll -version
   ```

 ![jekyll 2](https://user-images.githubusercontent.com/117553252/202201887-2e1e3e5b-eb54-446c-bb8a-6412f780efa6.png){: width=”100%“ height=”100%“}{: .center}

