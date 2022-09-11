## Windows通过ffmpeg批量合并视频

使用ffmpge是，感觉通过命令合并视频比较麻烦，在windows中拖拽(drag and drop)是比较方便的一种操作，下面这个链接给了一些启发，但在我的本地不能工作
[FFmpeg - Drag and drop video joiner](https://quizgenerator.net/en/2018/02/ffmpeg-%E3%83%89%E3%83%A9%E3%83%83%E3%82%B0%E3%82%A2%E3%831tp1-tb3%E3%83%89%E3%83%89%E3%83%AD%E3%83%83%E3%83%97%E3%81%A71-tp1te5%8B%95%E7%94%BB%E7%B5%90%E5%90%88/)


调整后的脚本如下：

不改变原编码

```Windows Batch
@echo off 

for %%i in (%*) do (
  echo %%i
	echo file '%%i'>> mylist.txt
)

pause


ffmpeg -f concat  -safe 0 -i mylist.txt -c copy merged.mp4

pause

del mylist.txt
```




改变编码

```Windows Batch
@echo off 

set FILES=
set N=0
setlocal EnableDelayedExpansion
for %%i in (%*) do (
	echo %%i
	set /a N=N+1
	set FILES=!FILES! -i %%i

)

echo %1
echo %N%
echo %FILES%

pause

ffmpeg %FILES% -filter_complex "concat=n=%N%:v=1:a=1" merged_recode.mp4
```
#注意，目录不能带中文


# 其他参考链接
https://cocoalopez.com/blog/?p=3642  
https://anaheimofmanheim.wordpress.com/2012/12/31/drag-and-drop-multiple-files-for-batch-ffmpeg-processing/
