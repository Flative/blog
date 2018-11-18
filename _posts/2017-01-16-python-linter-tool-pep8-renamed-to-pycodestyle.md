---
layout: post
title: '[Python] 왜 pep8은 pycodestyle이 되었을까?'
author: 최민석
image: /static/covers/python-logo.png
date: 2017-01-16
tags: [python]
---
안녕하세요. Flative의 최민석 입니다.

사실 제목의 `pep8`은 [PEP-0008](https://www.python.org/dev/peps/pep-0008/)문서가 아닌, Python linter tool `pep8`입니다.

Flative에서 새로 준비하고 있는 프로젝트(가제: 혼밥남녀)에 CI를 붙이면서 이왕이면 코드도 보다 효율적으로, 일관된 규칙으로 짜기 위해 linter를 붙이려고 `pep8`, `flake8`, `pylint`등등 여러가지 린터들을 찾아보고 비교하고 있던중 본 이야기를 공유하려 합니다.

그중 `pep8`이 뭔가 공식적인 느낌이 들어서 검색의 대부분은 `pep8`을 중심으로 다른 린터들과 비교해 보는것 이었는데,

[Please rename this tool · Issue #466 · PyCQA/pycodestyle · GitHub](https://github.com/PyCQA/pycodestyle/issues/466)
(자세한 내용이 궁금하시면 쭉 읽어보시길 추천드립니다.)

검색도중 이 이슈를 찾았습니다. 처음에는 `pep8`이라는 네이밍에 단순히 불평이 있는 사람인줄 알았더니 이슈 제안자가 [gvanrossum (Guido van Rossum) · GitHub](https://github.com/gvanrossum) 파이썬의 아버지 귀도셨습니다.

대략적인 대화의 뉘앙스가 이렇게 흘러갑니다.

> gvanrossum: PEP라는 이름을 사용하면 안된다. Python style guide는 사람을 위해 쓰여진거고 여러 섬세한? 미묘한 부분들이 있다.  `pep8`(linter)에 문제가 있으면 사람들은 이게 tool의 문제가 아닌 PEP의 문제로 인식할 수 있다.
>
> sigmavirus24: 많은 프로젝트들이 `flake8`을 활용하고 있고. 대부분 `pep8`이 [PEP-0008](https://www.python.org/dev/peps/pep-0008/)에서 영감을 받은 linter라는것을 알고 있을것이다.

또

> sigmavirus24: [PSF Trademark Usage Policy](https://www.python.org/psf/trademarks/)를 인용하면서 PEP는 상표로 등록되어있지 않다~ 라고 이야기하면서 논쟁이 쭈욱 계속되다가
>
> 1월 12일  [Nurdok (Amir Rachum) · GitHub](https://github.com/Nurdok)이 pep가 붙은 다른 프로젝트 pep257도 곧 이름이 바뀔거다, pep8도 이름이 바뀔거라면 `pycodestyle` `pydocstyle` `pycodelint` `pydoclint`등의 이름으로 바꾸는게 좋을거다

라고 제안하고.

결국 `pycodestyle`로 이름이 바뀌었습니다
![Finally renamed](/blog/static/images/2017-01-16-python-linter-tool-pep8-renamed-to-pycodestyle/finally.png)
이 과정을 쭉 지켜보니 느낀점들이

* 네이밍은 신중하게, 그리고 명확하게
* 이름 하나 변경할 뿐인데, `pep8`이라고 쓰여있던 모든 문서들, 코드 의존성, 사용자에게 `pep8`이 `pycodestyle`로 변경되었다고 알리기 까지. 많은 번거로움이 뒤따릅니다.
* 네이밍은 정말로 신중하게 하자.
