# Frequently Used Git Commands

**목록**

- [git init](#git-init)
- [git clone](#git-clone)
- [git help](#git-help)
- [git pull](#git-pull)
- [git add](#git-add)
- [git commit](#git-commit)
- [git push](#git-push)

------

- git init

git으로 관리하고자 하는 디렉토리를 git 저장소로 초기화하는 명령어이다. 만약 git으로 관리가 되고 있지 않은 디렉토리에서 git status 명령어를 치면 다음과 같이 git 저장소(repository)가 아니라는 결과를 확인할 수 있다.

```bash
$ git status
fatal: not a git repository (or any of the parent directories): .git
```

해당 디렉토리를 git 저장소로 초기화를 하면 다음과 같다.

```bash
$ git init
Initialized empty Git repository in C:/git_test/.git/
```

또한 실제 해당 디렉토리 내부(C:/git_test)에 .git 폴더가 생기면서 해당 디렉토리가 git으로 관리되기 시작한다.

```bash
$ ls -al
total 16
drwxr-xr-x 1 0___0 197121 0 Jun  7 21:42 ./
drwxr-xr-x 1 0___0 197121 0 Jun  7 21:39 ../
drwxr-xr-x 1 0___0 197121 0 Jun  7 21:42 .git/
```

<br>

- git clone

이미 github 등에 공개되어 있는 프로젝트를 로컬에 클론하여 git 저장소를 관리할 수도 있다. 일반적으로 다음과 같은 명령어를 통해 git clone을 한다.

```bash
git clone [URL]
```

실제 다음과 같이 해당 프로젝트의 clone이 이루어진다.

```bash
$ git clone https://github.com/cgvvxx/cgvvxx.git
Cloning into 'cgvvxx'...
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (4/4), done.
Receiving objects: 100% (6/6), done.
Resolving deltas: 100% (1/1), done.
remote: Total 6 (delta 1), reused 0 (delta 0), pack-reused 0
```

그러면 다음과 같이 해당 하위 폴더에 clone한 프로젝트가 복제가 됨을 확인할 수 있다.

```bash
$ cd cgvvxx/
$ ls -al
total 8
drwxr-xr-x 1 0___0 197121    0 Jun  7 21:55 ./
drwxr-xr-x 1 0___0 197121    0 Jun  7 21:55 ../
drwxr-xr-x 1 0___0 197121    0 Jun  7 21:55 .git/
-rw-r--r-- 1 0___0 197121 2914 Jun  7 21:55 README.md
```

<br>

- git help

git 명령어에 대한 정보를 알고 싶을 때 help 명령어를 사용할 수 있다. 해당 명령어를 통해 자주 사용하는 23가지의 git 명령어에 대한 정보를 알 수 있다.

```bash
$ git help
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           [--super-prefix=<path>] [--config-env=<name>=<envvar>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone             Clone a repository into a new directory
   init              Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add               Add file contents to the index
   mv                Move or rename a file, a directory, or a symlink
   restore           Restore working tree files
   rm                Remove files from the working tree and from the index
   sparse-checkout   Initialize and modify the sparse-checkout

examine the history and state (see also: git help revisions)
   bisect            Use binary search to find the commit that introduced a bug
   diff              Show changes between commits, commit and working tree, etc
   grep              Print lines matching a pattern
   log               Show commit logs
   show              Show various types of objects
   status            Show the working tree status

grow, mark and tweak your common history
   branch            List, create, or delete branches
   commit            Record changes to the repository
   merge             Join two or more development histories together
   rebase            Reapply commits on top of another base tip
   reset             Reset current HEAD to the specified state
   switch            Switch branches
   tag               Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch             Download objects and refs from another repository
   pull              Fetch from and integrate with another repository or a local branch
   push              Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
See 'git help git' for an overview of the system.
```

또는 다음과 같은 help 명령어를 통해 git 명령어에 대한 documentation을 확인할 수 있다. 다음 두 가지 명령어는 동일한 결과를 나타낸다.

```bash
$ git help [command] 
$ git [command] --help
```

혹은 git 명령어에 대한 자세한 설명이 아닌 해당 명령어에 대한 간단한 옵션들을 확인하고 싶으면 다음과 같이 활용할 수 있다.

```bash
$ git [command] -h
```

실제로 다음과 같이 push 명령어에 대한 여러 가지 옵션들을 확인할 수 있다.

```bash
$ git push -h
usage: git push [<options>] [<repository> [<refspec>...]]

    -v, --verbose         be more verbose
    -q, --quiet           be more quiet
    --repo <repository>   repository
    --all                 push all refs
    --mirror              mirror all refs
    -d, --delete          delete refs
    --tags                push tags (can't be used with --all or --mirror)
    -n, --dry-run         dry run
    --porcelain           machine-readable output
    -f, --force           force updates
    --force-with-lease[=<refname>:<expect>]
                          require old value of ref to be at this value
    --force-if-includes   require remote updates to be integrated locally
    --recurse-submodules (check|on-demand|no)
                          control recursive pushing of submodules
    --thin                use thin pack
    --receive-pack <receive-pack>
                          receive pack program
    --exec <receive-pack>
                          receive pack program
    -u, --set-upstream    set upstream for git pull/status
    --progress            force progress reporting
    --prune               prune locally removed refs
    --no-verify           bypass pre-push hook
    --follow-tags         push missing but relevant tags
    --signed[=(yes|no|if-asked)]
                          GPG sign the push
    --atomic              request atomic transaction on remote side
    -o, --push-option <server-specific>
                          option to transmit
    -4, --ipv4            use IPv4 addresses only
    -6, --ipv6            use IPv6 addresses only

```

<br>

- git status

git status 명령어를 통해 git 저장소에 있는 파일들에 대한 상태를 알 수 있다. git에서 관리하는 파일들은 Untracked, Tracked, Modified, Staged 등의 상태로 관리되는데 자세한 설명은 다음을 참고한다. 현재 파일들에 변경 사항이 존재하지 않는 경우 다음과 같은 상태를 확인할 수 있다.

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

만약 해당 디렉토리에 새로운 파일을 생성하고 상태를 확인하면 다음과 같다.

```bash
$ touch git_test.txt
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        git_test.txt

nothing added to commit but untracked files present (use "git add" to track)
```

- git pull

원격 저장소(remote repository)에서 변경된 데이터를 가져와서 로컬 저장소(local repository)에 반영하고자 할 때 git pull 명령어를 활용한다. [remote]에 원격 저장소명(또는 URL), [branch]에 브랜치명을 입력한다. 만약 remote가 origin이고 branch가 현재 브랜치와 같다면 생략할 수 있다.

```bash
$ git pull [remote] [branch]
```

따라서 원격 저장소의 main 브랜치를 가져오고자 하는 경우 다음과 같은 명령어를 활용할 수 있다. 이 때, 로컬 저장소가 최신화 되어 있다면 다음과 같은 결과를 나타낸다.

```bash
$ git pull origin main
From github.com:cgvvxx/cgvvxx
 * branch            main       -> FETCH_HEAD
Already up to date.
```

실제 이는 원격 저장소에서 변경된 내용을 로컬로 가져와 병합하는 것과 같은 과정이므로 다음의 명령어와 동일한 결과를 나타낸다.

```bash
$ git fetch origin
$ git merge origin/main
```

만약 원격 저장소에 변경된 내용이 있다면 다음과 같이 merge되는 결과 또한 같이 보여준다.

```
$ git pull origin main
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
Unpacking objects: 100% (3/3), 652 bytes | 81.00 KiB/s, done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
From github.com:cgvvxx/cgvvxx
 * branch            main       -> FETCH_HEAD
   614012c..548646c  main       -> origin/main
Updating 614012c..548646c
Fast-forward
 README.md | 2 --
 1 file changed, 2 deletions(-)
```

<br>

- git add

로컬 저장소 내에서 변경된 내용이 있는 파일을 스테이징 영역(Staging Area)으로 올리는 명령어이다. [pathspec]에는 스테이징 영역으로 올리고자 하는 파일 또는 폴더의 경로를 입력한다.

```bash
$ git add [pathspec]
```

만약 현재 디렉토리의 모든 변경 내용을 스테이징 영역으로 올리고자 할 때는 다음과 같은 명령어를 사용한다.

```bash
$ git add .
```

혹은 작업 디렉토리 내의 모든 변경 내용을 스테이징 영역으로 올리고자 한다면 다음과 같은 명령어를 사용한다.

```bash
$ git add -A
$ git add --all
```

또는 현재 디렉토리 하위의 docs의 모든 txt 파일을 스테이징 영역으로 올린다면 다음과 같이 명령어를 사용할 수도 있다.

```bash
$ git add docs/*.txt
```

해당 명령어를 통해 변경 내용을 스테이징 영역으로 올렸다면 git status 명령어를 통해 다음과 같이 변경 내용을 포함하는 파일들의 상태를 확인할 수 있다.

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   docs/git_test1.txt
        new file:   docs/git_test2.txt
```

<br>

- git commit

git add를 통해 스테이지 영역으로 올린 파일들을 커밋하는 경우 git commit 명령어를 사용한다. [message]에는 간단한 커밋 메세지를 작성할 수 있다. 

```bash
$ git commit -m [message]
```

-m 옵션을 사용하지 않는다면 커밋 메시지를 작성하는 vim 편집기가 열리고 해당 부분에 커밋 메시지를 작성할 수 있다.

- [i]로 입력 모드 전환 > 커밋 메세지 작성 > [esc]로 명령 모드 전환 > :wq로 vim편집기 나가기

실제 다음 예제와 같이 현재 스테이징 영역에 있는 파일을 커밋할 수 있다.

```bash
$ git commit -m "commit message test"
[main 83d7765] commit message test
 1 file changed, 1 insertion(+)
 create mode 100644 git_test.txt
```

스테이징 영역에 있는 모든 파일을 커밋한 경우 git status 명령어를 사용하면 다음과 같은 내용을 확인할 수 있다.

```bash
$ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

<br>

- git push

로컬 저장소에서 커밋한 내용을 원격 저장소에 반영하고자 경우 git push 명령어를 사용한다. git pull 명령어와 마찬가지로 remote가 origin이고 branch가 현재 브랜치와 같다면 생략할 수 있다.

```bash
git push [remote] [branch]
```

실제 git push를 통해 다음과 같은 결과를 확인할 수 있다.

```bash
$ git push origin main
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 282 bytes | 282.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:cgvvxx/cgvvxx.git
   548646c..c4f6f15  main -> main
```

<br>
