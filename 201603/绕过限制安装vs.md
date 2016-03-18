安装vs有个限制，必须ie10以上才行，下面这个方式可以绕过这个限制
将下面内容保存到xx.bat中，然后执行bat脚本，重启后即可安装
```
@ECHO OFF

:IE10HACK 
REG ADD "HKLM\SOFTWARE\Wow6432Node\Microsoft\Internet Explorer" /v Version /t REG_SZ /d "9.10.9200.16384" /f 
REG ADD "HKLM\SOFTWARE\Wow6432Node\Microsoft\Internet Explorer" /v svcVersion /t REG_SZ /d "10.0.9200.16384" /f 
REG ADD "HKLM\SOFTWARE\Microsoft\Internet Explorer" /v Version /t REG_SZ /d "9.10.9200.16384" /f 
REG ADD "HKLM\SOFTWARE\Microsoft\Internet Explorer" /v svcVersion /t REG_SZ /d "10.0.9200.16384" /f 
GOTO EXIT

:REVERTIE 
REG DELETE "HKLM\SOFTWARE\Wow6432Node\Microsoft\Internet Explorer" /v svcVersion 
REG DELETE "HKLM\SOFTWARE\Microsoft\Internet Explorer" /v svcVersion 
REG ADD "HKLM\SOFTWARE\Wow6432Node\Microsoft\Internet Explorer" /v Version /t REG_SZ /d "8.0.7601.17514" /f 
REG ADD "HKLM\SOFTWARE\Microsoft\Internet Explorer" /v Version /t REG_SZ /d "8.0.7601.17514" /f 
GOTO EXIT

:EXIT
```

> 副作用是你看ie的信息时会有点别扭，提示版本是10
