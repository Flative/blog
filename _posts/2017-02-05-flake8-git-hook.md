---
layout: post
title: '[Python] flake8 git hook의 문제와 해결'
author: 최민석
image: /static/covers/python-logo.png
date: 2017-02-05
tags: [python]
draft: true
---

안녕하세요. Flative의 최민석 입니다.

Python으로 개발을 하며 보다 일관된 규칙로, 효율적으로 개발하기 위해 `flake8` 린터를 붙여 CI에서 체크하게 해두던 와중. 매번 새로 push하기 전

```bash
$ flake8 --config .flake8
```

하기가 귀찮아 이 과정을 자동으로 할 수는 없을까? 해서 `git`에서 커밋, 푸시, 커밋 메세지 등 다양한 상황에서 사용할 수 있는 hook이란걸 알게되어.

사용하던 `flake8`을 pre-commit 훅에 붙이려고 이런저런 시도들을 하다가.

`flake8`에서 공식적으로 지원하는

```bash
$ flake8 --install-hook git
```

명령어로 훅을 설치하고 커밋을 하려 했는데 문제가 발생했습니다.

1. flake8 린터가 파이썬이 아닌 파일들(.md, .txt, .rst, …)을 검사하고 에러 발생

```
/var/folders/bb/0_8jdgw15f72f18m_fpqwcqr0000gn/T/tmpqbcrt3vn/Users/Curzy/Workspace/flative/hbnn/hbnn/README.md:3:2:
E999 SyntaxError: invalid syntax
/var/folders/bb/0_8jdgw15f72f18m_fpqwcqr0000gn/T/tmpqbcrt3vn/Users/Curzy/Workspace/flative/hbnn/hbnn/README.md:3:20:
E226 missing whitespace around arithmetic operator
```

2. config파일에 의해 검사에서 제외된 파일들을 포함하여 검사해서 에러 발생

```.flake8
[flake8]
ignore = F401
exclude =
    .git,
    __pycache__,
    hbnn/settings.py,
    manage.py,
    */migrations
```

```
./hbnn/settings.py:102:80: E501 line too long (83 > 79 characters)
./user/migrations/0001_initial.py:20:80: E501 line too long (88 > 79 characters)
./user/migrations/0001_initial.py:21:80: E501 line too long (103 > 79 characters)
```

첫 번째 문제는

``` before
def find_modified_files(lazy):
    diff_index_cmd = [
        'git', 'diff-index', '--cached', '--name-only',
        '--diff-filter=ACMRTUXB', 'HEAD`
    ]
```

변경된 파일들을 가져오는 함수를

``` after
def find_modified_files(lazy):
    diff_index_cmd = [
        'git', 'diff-index', '--cached', '--name-only',
        '--diff-filter=ACMRTUXB', 'HEAD', '|', 'grep', '-e', '\.py$'
    ]
```

파이썬 확장자에 해당하는 경우로 필터링하여 해결 했고.

두번째 문제는
excludes(제외할 리스트)의 앞에 검사에서 임시적으로 사용할 temp path를 붙여서 실제 제외할 파일의 경로와 맞지 않아 생겼던 문제인걸로 판단하여,

``` before
def update_excludes(exclude_list, temporary_directory_path):
    return [
        (temporary_directory_path + pattern)
        if os.path.isabs(pattern) else pattern
        for pattern in exclude_list
    ]
```

``` after
def update_excludes(exclude_list, temporary_directory_path):
    return [
        (temporary_directory_path + pattern)
        for pattern in exclude_list
        if os.path.isabs(pattern)
    ]
```

원본 file 경로와 임시 경로를 모두 포함하게 해서 해결했습니다.

이러고 기분좋게 히히 드디어 오픈소스에 첫 기여를 할 수 있다는 상상에 `flake8`  [PyCQA / flake8 · GitLab](https://gitlab.com/pycqa/flake8) 에 머지 리퀘스트를 날렸는데!

날리고 보니 현재 진행중인 개발버전에서 처리가 되었고 다음 버전(3.3.0) 릴리즈때 반영될 예정이라고 합니다. ㅜ-ㅜ

그래도 다음 버전이 나오기 전까지 사용할 수 있는. 위 문제들을 해결한 pre-commit-installer 스크립트를 만들었으니. 필요하신 분들께 도움이 되었으면 합니다.

[pre-commit-installer.py](https://gist.githubusercontent.com/Curzy/2381055de0ec160ec0b69d325f7e5ef9/raw/542bb8aa5659ab724e154905e1698c309fa7f3e6/pre-commit-installer.py)
위 스크립트 파일 다운받고,

```bash
$ python pre-commit-installer.py
```

로 설치할 수 있습니다.

린터를 통과하지 못할 경우 커밋이 불가능하게 해두었는데 원하지 않는 분이라면 set_strict()를 빼고

```bash
$ git config --bool flake8.strict false
```

로 strict 설정을 변경해주면 됩니다 :)

감사합니다.
