# git alias

- git config를 이용하여 각 명령의 alias를 만들어서 사용가능
- EX. 자주 사용하는 명령어를 간단한 이름으로 사용	

```
$ git config --global alias.ci commit
$ git config --global alias.st 'status -s'
>> git ci "커밋메시지"
>> git st
```

- EX. add, commit, push를 acp로 한번에

```
$ git config --global alias.acp '!f() { git add -A && git commit -m "$@" && git push; }; f'
>> git acp "커밋메세지"
```

- git config로 삭제가능

```
$ git config --global --unser alias.acp
```

