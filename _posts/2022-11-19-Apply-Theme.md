---
title: "[Windows] Chapter02. 깃허브 블로그 테마 적용하기"
excerpt: "깃허브 블로그 테마 적용하는 방법"

categories:
 - Blog
tags:
 - [Blog, jekyll, Github, Git, Ruby]

toc: true
toc_sticky: true

date: 2022-11-19
last_modified_at: 2022-11-19
---

<!-- outline-start -->


### 00. 블로그 테마를 정하자.

- 아래 3개의 페이지를 참조하여 본인이 하고 싶은 블로그 테마를 정한다.
https://jekyllrb-ko.github.io/docs/themes/
http://themes.jekyllrc.org/
https://jekyll-themes.com/free/

- 하고 싶은 테마를 다운로드 한다.
download와 fork 2가지의 방식이 있지만 나중에 페이지에 글을 쓰거나 수정을 하려면 download 방식이 편해서 download를 추천한다.

- download 한 것을 압축을 플고, 자신의 블로그 폴더에 붙여넣기 한다.

### 01. bundle 설치하기.

- 윈도우 cmd 창을 열어 bundle 을 설치하자.

- 일단 깃허브 블로그 폴더로 이동한다.
cd Hangyeryeong.github.io

- gem install bundle

- bundle install

### 02. url 수정하기.

- _config.yml 파일에 들어가 url에 본인의 블로그 주소로 수정한다.
url: "https://Hangyeryeong.github.io"

### 03. 적용하기.

- 자신의 블로그 폴더를 오른쪽 마우스를 눌러 'Git Bash Here'을 누른다.

- 다음 명령어를 통해 테마를 적용 해 주자.

- bundle exec jekyll serve

- 127.0.0.1:4000 에 들어가서 테마가 적용되었는지 확인한다.

- 위 주소는 본인의 컴퓨터에서만 보이기 때문에 public 주소에도 이제 테마를 적용해야 한다.


### 04. 업로드하기.

- 자신의 블로그 폴더를 오른쪽 마우스를 눌러 'Git Bash Here'을 누른다.

- 다음 명령어를 통해 업로드 해 주자.

- git add .

- git commit -m "add theme"

- git push

### 05. 확인하기.

- 자신의 블로그에 들어가 테마가 잘 적용이 되었는지 확인 해 보자.(시간이 조금 걸릴 수 있음)