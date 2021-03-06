---
title: "[Linux] 포그라운드(fg)와 백그라운드(bg) 프로세스(process)"
layout: post
tags:
- Linux
- foreground
- background
- process
- nohup
---

쉘 스크립트로 bulk파일 생성하던 도중 생성시간이 오래걸려 백그라운드로 밤에 작업이 되도록 진행한 적이 있다. 
이 작업을 통해 알게 된 내용을 정리하고자 한다.

모든 프로세스는 백그라운드 또는 포그라운드 형태로 작동한다.

백그라운드(background process, bg) : 프로세스가 실행되는 동안에도 터미널에서 작동을 받아드릴 수 있고, 로그 확인하고 싶을 때 별도 파일로 로그를 남겨야함
포그라운드(foreground process, fg) : 터미널에 직접 연결된 것으로 터미널과 입출력을 주고받는 프로세스로 터미널 창에서 지속적인 모니터링 가능





**[프로세스 실행/종료]**
**백그라운드 프로세스 실행**은 명령어 뒤에 &를 붙인다.
	ex) test라는 쉘 스크립트가 있다 가정하면,     $ ./test.sh &  명령어 치면 백그라운드로 프로세스 실행됨
	$ ./test.sh & 명령어 실행 시, 백그라운드 프로세스 job 번호와 pid(프로세스 id)가 뜬다.
	
```
$ ./test.sh & 
[1]     29486
```

	
백그라운드 프로세스 확인은 jobs 명령어로 실행중인 백그라운드 프로세스 확인 가능하다.
 
```
$ jobs
 
```
job번호, 상태, 실행중인 명령 확인가능하다.

**백그라운드 프로세스 강제종료**는 kill -9 %job번호

```
kill -9 %1
 
```

**포그라운드 프로세스 실행**은 터미널에서 일반적으로 실행시키는 방식으로 하면된다.
```
$ ./test.sh
 
```

**포그라운드 프로세스 강제종료 시**, Ctrl + C  를 입력한다.





**[bg -> fg, fg -> bg]**

**백그라운드에서 포그라운드 전환 시**, jobs 명령어를 통해 job 번호를 확인 한 후 $fg %job번호 명령어를 입력한다.
ex) job번호가 2번일 경우,

```
$ fg %2
 
```

**포그라운드에서 백그라운드 전환 시**, 실행 중인 프로세스를 먼저 종료해야 한다.
Ctrl + Z 프로세스 인터럽트한 후 jobs로 해당 프로세스 번호 확인 후 $ bg %job번호 명령어를 입력한다.
ex) job번호가 1번일 경우, 


```
$ bg %1
 
```




**[백그라운드 로그 남기기]**

$ 명령어 >> 로그남길_파일명 & 

```
$ test.sh >> test_log.log & 
 
```

	
test라는 쉘 스크립트 실행하면서 로그는 test_log.log파일에 기록한다.






**[nohup]**

터미널 사용자들이 시스템에 로그인하여 프로그램을 실행시키고 해당 프로그램이 종료되지 않은 상태에서 로그아웃 시, 프로그램은 HUP(행업) 프로세스에 의해 강제종료된다. 즉, 백그라운드로 프로세스 실행 후 완료되지 않은 상태로 터미널을 종료하거나 세션이 끊기면 백그라운드 프로세스가 중단된다. 이때 사용할 수 있는 명령어가 nohup이다.

nohup 명령어를 동시에 주면 백그라운드로 작업 실행시키고 터미널을 종료해도 백그라운드 프로세스는 계속 실행된다.

```
 $ nohup ./test.sh >> test_log.log &
  
```

test.sh 쉘 스크립트를 백그라운드로 실행하며 로그는 test_log.log 파일에 기록하고 터미널이 종료되도 영향받지 않는다. 

log파일을 별도로 주지 않아도 nohup.out 파일에 로그가 자동으로 쌓이게 된다.






**정리) 장시간 실행되는 프로세스에 주로 nohup 명령어와 함께 백그라운드 프로세스를 실행할 수 있다.**