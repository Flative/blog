---
layout: post
title: 'Jekyll + GitHub Pages 의존성 쉽게 관리하기'
author: 이현수
date: 2016-09-23
tags: [jekyll, github-pages]
---

안녕하세요. Flative의 이현수입니다. 혹여나 모르는 분들을 위한 짤막한 정보글입니다.

최근 많은 곳에서 [Jekyll](https://jekyllrb.com/)과 [GitHub Pages](https://pages.github.com/)를 이용해 블로그를 운영하고 있습니다. Flative도 마찬가지로 두 서비스를 이용해서 블로그를 운영하고 있으며, 제가 현재 근무하고 있는 [애드투페이퍼](http://www.add2paper.com/about/)와 [개인 블로그](https://incleaf.github.io)에서도 사용하고 있습니다.

무료로 쉽게 블로그를 운영할 수 있다는 장점도 있지만 Github Pages라는 플랫폼 위에서 돌아가는 블로그이기 때문에 **GitHub Pages의 디펜던시 버전은 점점 올라가는 반면에 블로그의 디펜던시 버전은 일정하게 유지되기 때문에** 로컬에선 잘 보이던 블로그가 GitHub에 업로드 후에는 제대로 보이지 않는 문제가 발생할 수도 있습니다.

이와 같은 문제를 해결하기 위해 GitHub에서 [pages-gem](https://github.com/github/pages-gem)이라는 Gem을 제공하고 있습니다.

1. Gemfile에 `gem 'github-pages', group: :jekyll_plugins` 추가
2. `bundle install` 커맨드를 실행
3. PROFIT!

도움이 됐으면 좋겠습니다. 다음엔 유익한 글로 찾아오겠습니다.
