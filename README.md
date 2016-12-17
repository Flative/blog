flative.github.io
==============

> 주의: [GitHub Pages]와 [Jekyll]에 대해서 충분히 숙지할 것.
> 주의: [Collaborating on projects using issues and pull requests](https://help.github.com/categories/collaborating-on-projects-using-issues-and-pull-requests/)을 정독.


### 설치

<https://github.com/flative/flative.github.io> 에 push 권한이 있다면:

1. git fetch or pull or clone
2. [Jekyll] 설치

```console
$ git clone git@github.com:flative/flative.github.io.git
$ cd flative.github.io
$ bundle install
```

### 실행(로컬)

```
$ bundle exec jekyll serve
$ open http://localhost:4000
```

### 글 쓰기

1. `_posts` 디렉토리에 `yyyy-mm-dd-slug.md` 파일로 복사(or 이동).
 - slug: 해당 포스트의 고유 키로 url의 일부로 사용. 왠만하면 특수문자없이 영문자,숫자,-(하이픈),.(점)...만 사용.
 - yyyy,mm,dd: 발행 년,월,일.
 - 참고: 최종적으로 포스트의 url(permalink)는 http://flative.github.io/yyyy/mm/dd/slug/
2. 파일 상단에 [front matter] 작성
 - layout: post # 레이아웃(필수). `page` 레이아웃을 사용하면 목록에 보이지 않는 글을 쓸 수 있음.
 - title: '제목' # 제목(필수)
 - author: `username` # 필자(필수).
 - tags: `[tag1,tag2,tag3,...]` # 태그 목록(선택). 왠만하면 특수문자없이 영소문자,숫자,-(하이픈),.(점)...만 사용.
 - image: http://... # 커버이미지 url(선택)
 - draft: `draft: true` 를 적지 않으면 글 목록에 표시가 되니, 임시 글일 경우에 꼭 표시하기
 - date: `YYYY-MM-DD` # 발행일(필수)
3. 처음 글을 쓰는 필자이라면 [**필자 등록**](#필자-등록)(필수)
4. 유력한(?) 태그가 새로 등장했다면 [**태그 등록**](#태그-등록)(선택)

### 필자 등록

1. `_authors` 디렉토리에 `username.md` 이름으로 필자 정보 파일 추가
 - 참고: 최종적으로 사용자 포스트 목록 페이지의 url은 http://flative.github.io/authors/username/
2. 파일 상단에 [front matter] 작성
 - layout: author
 - name: ex) leejaedus
 - title: ex) 이재연
 - image: ex) static/authors/XXX.jpg 직접 넣으세요
 - email: ex) leejaeduss@gmail.com
 - facebook: ex) https://www.facebook.com/leejaeduss
 - github: ex) https://github.com/leejaedus
3. 내용은 필요없음

### 태그 등록

1. `_tags` 디렉토리에 `tag-name.md` 이름으로 필자 정보 파일 추가
 - 참고: 최종적으로 사용자 포스트 목록 페이지의 url은 http://flative.github.io/tags/tag-name/
2. 파일 상단에 [front matter] 작성
 - layout: tag # 레이아웃(필수)
 - name: `tag-name` # post의 tags 배열의 항목과 매칭(필수). 왠만하면 특수문자없이 영소문자,숫자,-(하이픈),.(점)...만 사용.
 - title: ... # 좀 더 길고 구체적인 설명(필수)
 - image: http://... # 태그 이미지(선택)

---

문의: <leejaeduss@gmail.com>

May the **SOURCE** be with you...

[GitHub Pages]: https://pages.github.com
[Jekyll]: https://jekyllrb.com
[front matter]: https://jekyllrb.com/docs/frontmatter/
[gfm]: https://guides.github.com/features/mastering-markdown/
[kramdown]: http://kramdown.gettalong.org
[rouge]: http://rouge.jneen.net
