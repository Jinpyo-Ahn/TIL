# 1.6 Git 최초 설정

`git config`라는 도구로 설정 내용을 확인하고 변경

- Git은 이 설정에 따라 동작
- 이때 사용하는 설정 파일은 3가지가 된다.
    1. `/etc/gitconfig`
        
        시스템의 모든 사용자와 모든 저장소에 적용되는 설정
        
        `git config --system` 옵션으로 이 파일을 읽고 쓴다.
        
    2. `~/.gitconfig`, `~/.config/git/config`
        
        특정 사용자(현재 사용자)에게만 적용되는 설정
        
        `git config --global` 옵션으로 이 파일을 읽고 쓴다.
        
    3. `.git/config`
        
        이 파일은 Git 디렉토리에 있고 특정 저장소(현재 작업 중인 프로젝트)에만 적용
        
        `--local` 옵션을 사용해 이 파일을 사용
        
        기본적으로 이 옵션이 적용되어 있다.
        

# 사용자 정보

사용자 이름과 이메일 주소를 설정

Git은 커밋할 때마다 이 정보를 사용

한번 커밋한 후 정보 변경 불가능

```tsx
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

만약 프로젝트마다 다른 이름과 이메일 주소를 사용하려면 --global 옵션을 빼고 실행

# 설정 확인

git config --list 명령을 실행하면 설정한 모든 것을 확인

```tsx
$ git config --list
user.name=John Doe // 사용자 이름
user.email=johndoe@example.com // 사용자 이메일
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
```

Git은 같은 키를 여러 파일(`/etc/gitconfig` 와 `~/.gitconfig` 같은)에서 읽기 때문에 같은 키가 여러 개 있을 수 있다. Git은 나중 값을 사용한다.

git config <key> 명령으로 Git이 특정 Key에 대해 어떤 값을 사용하는지 확인

```tsx
$ git config user.name
John Doe
```
