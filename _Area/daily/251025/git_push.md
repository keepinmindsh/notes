
# git / 원격 Repo 연결시 

```shell 
git remote add origin https://github.com/keepinmindsh/lines-gpu-finetuning-model-serving.git
git config --global push.autoSetupRemote true
```

# git/ pull, push with force

```shell
git pull --ff
git push -f
git push origin master --force
```

# git / remote origin repo update 

```shell
git remote set-url origin https://github.com/lines-code/lines-gpu-finetuning-model-serving.git
```

```shell
git remote set-url origin  https://github.com/keepinmindsh/lines-gpu-finetuning-model-serving.git 
```