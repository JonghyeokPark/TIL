# Gitbook Setup
> Markdown 문법을 활용하여 문서를 작성할 수 있는 Gitbook 설정방법에 대해 알아본다.

## Prerequsite
* [NodeJS](https://nodejs.org/ko/download/)
* Mac OS X, Windows, Linux, Unix 운영체제

## Installation
1. NPM으로 설치하기
~~~
npm install gitbook-cli -g
~~~
* gibook-cli는 Gitbook command line interface이며, Gitbook을 build 및 생성 그리고 배포할 수 있는 도구이다.

2. Gitbook 빌드하기
~~~
gitbook build 
~~~
* Gitbook build를 하게되면 다음과 같은 기본 디렉토리 구조가 생성된다.
>  - book.json /* 기본 gitbook 구성 데이터 */
>  - REAMDE.md /* Gitbook 소개 */
>  - SUMMARY.md /* 목차 참조 */

3. Gitbook 생성하기
~~~
gitbook init (./new-dir)
~~~
*  만약 새로운 디렉토리에 생성하려면 `gitbook init [디렉토리명]` 을 입력한다.

4. Gitbook 배포하기
~~~
gitbook serve --port 4000
~~~
* port를 지정하지 않는 경우, 기본 포트인 4000으로 접속이 가능하다. (https://localhost:4000)

5. Gitbook Plugin
~~~
/* location: book.json */
{
    "plugins":
    [
        "plantuml",
        "include-highlight",
        "toc",
        "fancybox",
        "collapsible-menu",
        "image-captions",
        "anchors",
        "gtoc",
        "theme-comscore",
        "youtubex",
        "etoc",
        "splitter",
        "codeblock-filename",
        "hide-published-with",
        "expandable-chapters-small"
    ]
~~~
* book.josn 파일에 Gitbook 생성에 효과적인 Plugin을 추가할 수 있다. 
* gitbook build시, 자동으로 npm을 통해 설치가 가능하다.
* plugin을 추가할 경우, 다시 빌드를 해야한다.


6. Gitbook 디버깅하기
~~~
gitbook build ./ --log=debug --debug
~~~
* 위의 명령어를 활용하여 디버깅이 가능하다.