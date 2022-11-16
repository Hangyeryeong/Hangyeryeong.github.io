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

- `git` 설치
- `ruby` 설치
 1. [Downloads](https://rubyinstaller.org/)
 1. `WITH DEVKIT` 에 있는 `(x86)` 중 마음에 드는 것을 다운 받는다.
  ![ruby 1](https://user-images.githubusercontent.com/117553252/202195331-b0fea8c8-4e8c-4c17-92f0-3169cd6fc377.png){: width=”50%“ height=”50%“}{: .center}
 1. 둘 다 체크
  ![ruby 2](https://user-images.githubusercontent.com/117553252/202196131-d7cacdfb-99a4-47af-9608-6ef56eadb233.png){: width=”50%“ height=”50%“}{: .center}
 1. 마지막에 체크하여 Ruby를 실행시켜 준다.
  ![ruby 3](https://user-images.githubusercontent.com/117553252/202196210-97b269d2-d837-4715-9e7b-3ad6797708c5.png){: width=”50%“ height=”50%“}{: .center}
 1. 이 화면이 나오면 `1,2,3` 을 입력하고 Enter 한다.
  ![ruby 4](https://user-images.githubusercontent.com/117553252/202196266-94f34ea2-ada5-44f2-a994-ee31390ab9b4.png){: width=”70%“ height=”70%“}{: .center}
 1. 윈도우의 cmd 창을 열어 ruby가 잘 설치 됐는지 확인한다.
 
   ```yaml
   ruby --version
   ```
 ![ruby 5](https://user-images.githubusercontent.com/117553252/202198332-abb2ac3c-967b-420f-b819-c4545c53dcfb.png)


- `jekyll` 설치

가 필요하다.




You’ll find this post in your _posts` directory. Go ahead and edit it and re-build the site to see your changes.<!-- outline-end --> You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
