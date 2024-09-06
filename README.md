[TOC]

# simplelog.py
## 一个简单的python日志框架模块

simplelog是一个简单的python日志框架模块。


### 快速使用simplelog：

#### 导入模块
将simplelog.py放入你的项目，然后：
```
import simplelog
```
导入simplelog模块即可使用

#### 创建日志对象
```
log = simplelog.LogHandle("test.log")
```
即可创建日志对象，其中logfile是日志文件输出的文件名


#### 输出信息到日志
**普通输出**
```
log.tolog("test info.")
```
输出到文件的信息为：
``
test info.
``

**行输出**
```
log.tologOnline("test info.")
```
会输出单独的一行信息到文件，即：
```
test info.

```

**带有时间戳的输出**
```
log.toLogWithTime("test info.")
```
输出到文件的信息为：
``
2024-08-04 19:01:05 test info.
``

**带有时间戳的行输出**
```
log.toLogOnLineWithTime("test info.")
```
输出到文件的信息为：
```
2024-08-04 19:01:05 test info.

```
<br>

### simplelog的实际使用

#### simplelog的参数
日志句柄的初始化方法定义如下：
```
class LogHandle :
    def __init__( self, out_logfile, log_level = ERROR, date_rotate_flag = False, time_stamp_flag = False) :
```
**out_logfile：**
传入日志输出的路径及文件名。

**log_level：**
设置日志句柄使用的日志等级，默认值设置为ERROR。

**date_rotate_flag：**
设置日志文件是否进行日期轮转，默认值设置为False。设置为True，会导致日志文件后自动加上日期并根据日期变化创建新的日志文件。

**time_stamp_flag:**
设置日志句柄输出是否使用时间戳，默认值设置为False。



#### simplelog的日志级别
在模块中，定义有以下几个常量：
```
CLOSE = 0
INFO = 1
WARNING = 2
ERROR = 3
CRITICAL = 4
DEBUG = 5
```
它分别对应了不同的日志级别。

#### 不同日志等级的输出方法
在LogHandle类中也对应了不同等级的输出方法:
```
def info(self, out_info) :

def waring(self, out_info) :

def error(self, out_info) :

def critical(self, out_info) :

def debug(self, out_info) :
```
除critical()外，其它方法均会向日志文件写入一条带有等级标签的信息。<font color="red">使用critical()不仅会向日志文件写入一条带有等级标签的信息，还会终止程序的运行。</font>
<br>

#### 示例1：
当使用以下以下方式新建日志句柄对象并输出信息时：
```
import simplelog
logfile = "test.log"
info = "test info."
#日志等级INFO，时间戳false， 文件日期轮转false
log = simplelog.LogHandle(logfile, log_level = simplelog.DEBUG, date_rotate_flag = False, time_stamp_flag = False)

log.info(info)
log.waring(info)
log.error(info)
log.debug(info)
log.critical(info)
```
test.log文件中将会输出以下信息，并终止程序的运行：
```
[INFO]test info.
[WARING]test info.
[ERROR]test info.
[DEBUG]test info.
[CRITICAL]test info.
```
<br>

#### 示例2
```
import simplelog
logfile = "test.log"
info = "test info."
#日志等级INFO，时间戳true， 文件日期轮转true
log = simplelog.LogHandle(logfile, log_level = simplelog.DEBUG, date_rotate_flag = True, time_stamp_flag = True)

log.info(info)
log.waring(info)
log.error(info)
log.debug(info)
log.critical(info)
```
<font color="red">因date_rotate_flag = True，若当前日期为2024年8月4日，则会写入信息至test_20240804.log。</font>并终止程序的运行：
```
2024-08-04 19:17:01 [INFO]test info.
2024-08-04 19:17:01 [WARING]test info.
2024-08-04 19:17:01 [ERROR]test info.
2024-08-04 19:17:01 [DEBUG]test info.
2024-08-04 19:17:01 [CRITICAL]test info.
```
<font color="red">因date_rotate_flag = True，当日期至2024年8月5日时，则会将信息写入test_20240805.log。</font>
<br>

#### 日志级别的设置及效果

**CLOSE**：

```
log = simplelog.LogHandle(logfile, log_level = simplelog.CLOSE)
```
关闭日志输出，<font color="red">调用critical()也不会输出信息和终止程序运行。</font>

**INFO**：
```
log = simplelog.LogHandle(logfile, log_level = simplelog.INFO)
```
仅输出INFO，调用critical()会输出信息和终止程序运行。

**WARING**:
```
log = simplelog.LogHandle(logfile, log_level = simplelog.WARING)
```
输出INFO、WARING，调用critical()会输出信息和终止程序运行。

**ERROR**:
```
log = simplelog.LogHandle(logfile, log_level = simplelog.ERROR)
```
输出INFO、WARING、ERROR，调用critical()会输出信息和终止程序运行。

**CRITICAL**:
```
log = simplelog.LogHandle(logfile, log_level = simplelog.CRITICAL)
```
输出INFO、WARING、ERROR，调用critical()会输出信息和终止程序运行。

**DEBUG**:
```
log = simplelog.LogHandle(logfile, log_level = simplelog.DEBUG)
```
输出INFO、WARING、ERROR、DEBUG，调用critical()会输出信息和终止程序运行。
<br>

#### 查看句柄参数：
**查看日志输出文件：**
```
log.getOutFile()
```
返回输出文件路径的字符串。
**查看设置的日志等级:**
```
log.getLogLevel()
```
返回设置日志等级的字符串。
**查看时间戳设置：**
```
log.getTimeStampFlag()
```
返回True或False。
**查看文件轮转设置：**
```
log.getDateRotateFlag()
```
返回True或False。
<br>

#### 修改句柄的参数：
**修改日志等级：**
若需要修改已经创建的日志句柄的日志级别，可以使用以下方法进行修改：
```
log.changeLogLevel(simplelog.INFO)
```

**修改输出到新的日志文件：**
```
log.changeOutFile(filename)
```
filename为新的文件路径。


**修改时间戳设置：**
```
log.changeTimeStampFlag(True)
```
即设置为启用，传入False则为禁用。

**修改文件轮转设置：**
```
log.changeDate_rotate_flag(True)
```
即设置为启用，传入False则为禁用。