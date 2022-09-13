---
layout: post
title: SpringBoot에서 JPA 사용하기
# subtitle: A awesome static site generator.
author: woong99
categories: spring
#

tags: springboot
sidebar: []
---

## JPA란?

- Java Persistence API (자바 ORM 기술에 대한 API 표준 명세)로 ORM을 사용하기 위한 인터페이스들을 모아둔 것이다.
- 자바 애플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스이다.
- ORM에 대한 자바 API 규격이며 대표적으로 Hibernate가 JPA를 구현한 구현체이다.

## ORM이란?

- Object-Relational Mapping (객체와 관계형데이터베이스 매핑, 객체와 DB의 테이블이 매핑을 이루는 것)
- 한마디로, 객체가 테이블이 되도록 매핑시켜주는 프레임워크이다.
- 프로그램의 복잡도를 줄이고 자바 객체와 쿼리를 분리할 수 있으며, 트랜잭션 처리나 기타 데이터베이스 관련 작업을 좀 더 편리하게 처리할 수 있다.
- SQL Query가 아닌 직관적인 코드(메소드)로 데이터를 조작할 수 있다.

## Hibernate란?

- JPA를 사용하기 위해서 JPA를 구현한 ORM 프레임워크 중 하나이다.
- 자바를 위한 오픈소스 ORM 프레임워크를 제공한다.
- Hibernate는 JPA 명세의 구현체이다. `javax.persistence.EntityManager`와 같은 JPA의 인터페이스를 직접 구현한 라이브러리이다.

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

## section 1

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

## section 2

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

$ a \* b = c ^ b $

$ 2^{\frac{n-1}{3}} $

$ \int_a^b f(x)\,dx. $

```cpp
#include <iostream>
using namespace std;

int main() {
  cout << "Hello World!";
  return 0;
}
// prints 'Hi, Tom' to STDOUT.
```

```python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

p1 = Person("John", 36)

print(p1.name)
print(p1.age)
```
