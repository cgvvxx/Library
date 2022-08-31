# sudo & su

리눅스는 대표적인 다중 사용자 시스템(Multi-User System OS)이다. 일반적으로 서버용 OS로 많이 사용되고, 여러 명의 사용자가 동시 접근을 하는 경우가 존재한다. 따라서 리눅스에서는 각 유저들에 대한 권한이 구분되어 있다. 예를 들어, 리눅스 상의 모든 명령어를 실행할 수 있는 슈퍼 계정인 root 계정 있다. sudo와 su 명령어는 이러한 권한 설정에 관한 명령어이다.

## 1. su (Switch User)

su 명령어는 현재 사용자를 로그아웃하지 않은 상태에서 다른 사용자의 계정으로 전환하는 명령어이다. 이 때, 전환하고자 하는 사용자의 계정의 비밀번호가 필요하다. su 명령어는 현재 사용자가 가진 환경변수 설정(working directory 등)을 유지한 채 대상 사용자의 계정으로 전환한다. 계정명을 입력하지 않으면 기본적으로 root 계정으로 전환된다. (즉, su와 su root 명령어는 동일하게 root 계정으로 전환하는 명령어이다.)

```bash
$ su <otheruser>
```

su - 명령어는 현재 사용자를 다른 계정으로 전환하고, 전환한 계정의 환경변수를 적용하는 명령어이다. '-'는 '-l' 혹은 '--login'과 동일한 기능을 한다. 마찬가지로 계정명을 입력하지 않으면 기본적으로는 root 계정으로 전환된다.

```bash
$ su - <otheruser>
$ su -l <otheruser>
$ su --login <otheruser>
```

예를 들어, 현재 사용자의 계정에서 a라는 변수에 sutest라는 값을 넣어두고, su 명령어로 계정을 변경하면 a라는 환경변수의 값이 sutest로 유지되지만, su - 명령어로 계정을 변경하면 a라는 환경변수의 값에 아무것도 저장되어 있지 않는다.

```bash
$ export a=sutest
$ su
# echo $a
sutest
# exit
$ su -
# echo $a
(null)

# <참고>
# '$' : `일반 사용자 계정일때`
# '#' : `root 게정일때`
```

## 2. sudo (Substitute User DO)

sudo 명령어는 일반 사용자가 root 권한을 일회성으로 빌려 명령을 실행할 수 있게 하는 명령어이다. 

```bash
$ sudo <command>
```

sudo는 root 권한을 빌려 명령어를 실행할 수 있게 하므로 생성, 수정 등의 이력이 남는 작업의 경우 root 유저가 아닌 명령을 내리는 현재 로그인된 사용자의 계정이 남게 된다.

**/etc/sudoers **에 등록이 되어있는 사용자 혹은 그룹만 sudo 명령어를 사용할 수 있고 sudo 명령어를 사용하는 경우, 현재 로그인된 사용자 계정의 비밀번호를 사용한다.

root 권한을 빌려 명령어를 내리려고 할 때마다 sudo 명령어를 입력하는 것은 번거로우므로 sudo -s 또는 sudo su 명령어를 통해 root 권한을 반영구적으로 빌릴 수 있다.

```bash
$ sudo -s
$ sudo su
```

- sudo -s : 환경변수까지 root 계정의 상태로 전환
- sudo su : 현재 사용자 계정의 환경변수를 root 계정으로 넘김

<br>

**Reference**

- [Su Command in Linux (Switch User) | Linuxize](https://linuxize.com/post/su-command-in-linux/)

- [The Differences between Su, Sudo Su, Sudo -s and Sudo -i - Make Tech Easier](https://www.maketecheasier.com/differences-between-su-sudo-su-sudo-s-sudo-i/)

- [root - What's the difference between sudo su vs just su? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/225451/whats-the-difference-between-sudo-su-vs-just-su)
