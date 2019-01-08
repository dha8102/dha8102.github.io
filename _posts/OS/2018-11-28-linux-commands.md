---
layout: post
title: Linux Commands
categories : [O/S]
tags : [O/S]
---



## 자주 쓰는 리눅스 터미널 명령어 정리

---

<br>

- Ctrl + Alt + T : 터미널 실행

- Ctrl + L : 터미널 청소

- Ctrl + Shift + C : 선택된 명령어 클립보드로 복사

- Ctrl + Shift + V : 클립보드 명령 붙여넣기

- Shift + PageUp / PageDown : 터미널 스크롤

- ls : 현재 폴더의 전체 리스트를 보여줌

- cd 계열 : change directory의 약자.

- cd ~ : 맨 위 단계 directory로 이동

- cd .. :  상위 directory로 이동

- cp index.html index.old : index.html 화일을 index.old 란 이름으로 복사

- mv index.htm index.html : index.htm 화일을 index.html 로 이름 변경

- mv file  ../main/new_file : 파일의 위치변경

- rm test.html : test.html 화일 삭제

- rm -r <디렉토리> : 디렉토리 전체를 삭제

- pwd : 현재의 디렉토리 경로를 보여줌

- cat : 파일의 내용을 화면에 출력

- mkdir [폴더이름] : 디렉토리 만들기

- rmdir [폴더이름] : 디렉토리 지우기

- rm -r [폴더이름] : 하위 경로를 포함한 디렉토리 모두 지우기

- top : 메모리 상태 보여주기(q로 종료)

- echo : 어떤 것을 화면에 인쇄

- halt : 시스템 종료

- reboot (또는 Ctrl+Alt+Del) : 시스템 다시 시작

- 하드웨어 정보 보기 : sudo apt-get install lshw-gtk 후 gksu lshw-gtk로 확인, 또는 메뉴에서 Hardware Lister 검색

- lspci \| grep -i VGA : 그래픽 카드 정보 보기

- nvidia-smi : (nvidia 그래픽 카드 장착되었을 때) gpu 정보 및 메모리 사용률 보기

- (sudo) kill -9 PID : 해당 PID의 프로세스를 강제 종료

  <br>

지속 업데이트 중



