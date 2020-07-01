---
layout: post
title: "Jekyll 블로그 시작하기 1 - 개발 환경 구축 및 Github 배포"
tags: ["jekyll"]
---

`Jekyll`은 개인 또는 조직을 위한 정적 웹 사이트를 구축할 수 있는 오픈 소스입니다. 이번 장에서 `Jekyll`로 정적 웹 사이트를 개발하기 위한 환경 구축과 `Github`에 직접 배포해보는 과정을 함께 해보겠습니다.

## Jekyll 개발 환경 구축

저는 `Mac`을 사용하기 때문에 `MacOS` 환경에서 개발 환경을 구축했습니다.

### Command-Line Tools

터미널을 열고 아래와 같이 입력해서 `Command-Line Tools`를 설치합니다.

```shell
$ xcode-select --install
```

`Command-Line Tools`를 설치해야 되는 이유는 [Xcode 없이 맥에 '명령어 라인 도구(Command Line Tools)'를 설치하는 방법](https://macnews.tistory.com/4243)에 잘 설명된 문구가 있어 차용하였습니다.

> 맥 운영체제에서 'gcc' 'make' 같은 컴파일 툴이나 'svn' 'git' 같은 분산 버전 관리툴, 또는 기본적인 Unix/Linux 툴킷을 사용하려면 기본적으로 홈브류(Homebrew)나 명령어 라인 도구(Command Line Tools)을 먼저 설치해야 합니다.

### Homebrew 설치

여기서는 `Homebrew`로 `Ruby`를 설치합니다. 터미널에서 아래와 같이 입력하여 `Homebrew`를 설치합니다. 만약 `Homebrew`가 이미 설치되어 있다면 건너뛰어도 괜찮습니다.

```shell
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Ruby 설치

터미널에서 아래와 같이 입력하여 `Ruby`를 설치합니다.

```shell
$ brew install ruby
```

터미널에서 `Ruby` 명령어를 사용할 수 있도록 아래과 같이 입력해서 `Shell` 설정에 `Ruby` 경로를 추가합니다.

```shell
$ echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile
```

터미널에서 아래와 같이 입력하여 `Ruby`와 `Gem`이 잘 설치되어 있는지 확인합니다.

```shell
$ ruby -v
ruby 2.6.3p62 (2019-04-16 revision 67580) [universal.x86_64-darwin19]

$ gem -v
3.0.3
```

### Jekyll 설치

`Gem`으로 `Jekyll`을 설치합니다. 터미널에서 아래와 같이 입력하여 `Jekyll`을 설치합니다.

```shell
$ sudo gem install -n /usr/local/bin/ jekyll
```

터미널에서 아래와 같이 입력하여 `Jekyll`이 잘 설치되어 있는지 확인합니다.

```shell
$ jekyll -v
jekyll 4.1.0
```

## Jekyll 프로젝트 생성

터미널에서 아래와 같이 입력하여 `Jekyll` 프로젝트를 생성합니다. `--blank` 옵션을 주게되면 테마가 없는 빈 프로젝트를 위한 디렉토리(`Scaffold`)가 생성됩니다.

```shell
$ jekyll new my-blog --blank
New jekyll site installed in /path/to/my-blog.
```

프로젝트가 생성된 디렉토리로 이동하여 아래와 같이 입력하여 `Jekyll` 프로젝트를 실행합니다. 포트 옵션을 따로 주지 않으면 기본적으로 `4000` 포트에서 실행됩니다.

```shell
$ cd my-blog
$ jekyll serve
```

[http://localhost:4000/](http://localhost:4000/)로 접속하여 `Jekyll` 프로젝트가 정상적으로 동작하는지 확인합니다.

### Github 배포

`Github`에서 `Github Page`라는 서비스를 무료로 이용할 수 있습니다. `Github Page`는 `Repository`에 저장한 파일을 웹 페이지로 보여줄 수 있는 서비스입니다. 즉, 정적 웹 사이트를 구축할 수 있습니다.

여기서는 `Github` 계정이 있다고 가정하겠습니다. `Repository` 생성 버튼을 누른 다음 `[사용자이름].github.io`이라는 이름으로 `Repository`를 생성합니다. 반드시 `[사용자이름].github.io`라는 이름으로 만드셔야 합니다.

터미널을 열고 `Jekyll` 프로젝트 디렉토리로 가서 아래와 같이 입력하여 `Repository`에 `Jekyll` 프로젝트를 업로드합니다.

```shell
$ git init
$ git add .
$ git commit -m "publish jekyll blog"
$ git remote add origin https://github.com/[사용자계정]/[사용자이름].github.io.git
$ git push -u origin master
```

정상적으로 업로드가 되었다면 [https://[사용자이름].github.io](https://[사용자이름].github.io)에 접속하여 `Jekyll` 블로그가 정상적으로 호스팅되는지 확인하면 됩니다!

## References

- [https://jekyllrb.com/](https://jekyllrb.com/)
- [Xcode 없이 맥에 '명령어 라인 도구(Command Line Tools)'를 설치하는 방법](https://macnews.tistory.com/4243)
- [GitHub Pages를 이용한 무료 홈페이지 만들기](https://wepplication.github.io/programming/github-pages/)
