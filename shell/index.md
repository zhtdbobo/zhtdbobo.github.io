# shell


# 文件重命名
### 使用date
> cp：复制一个文件或文件夹
	-r ：递归持续复制，用于目录的复制行为；
	-f ：为强制 (force) 的意思，若有重复或其它疑问时，不会询问使用者，而强制复制；
	old：是复制源的文件夹路径，因为我已经cd到该路径下了，所以不要前缀；
	copy_test/new_date '+%Y%m%d_%H.%M.%S'：是复制之后的文件夹路径，复制到copy_test文件夹下面的名为new_date '+%Y%m%d_%H.%M.%S'；
```
	cp -rf old copy_test/new_`date '+%Y%m%d_%H.%M.%S'`
```
