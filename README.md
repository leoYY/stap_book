# Stap Book

## 目录

－ 安装  
－ 基本使用

### 安装
### 源码安装  
环境：centos7  Linux version 3.10.0-229-el17.x86_64  
源码版本：[systemtap-2.8.tar.gz](https://sourceware.org/systemtap/ftp/releases/systemtap-2.8.tar.gz)  
依赖程序：[elfutils-0.158](https://fedorahosted.org/releases/e/l/elfutils/0.158/elfutils-0.158.tar.bz2), m4, yacc, g++  
（其中原本试验在ubuntu中，但由于debian系列存在问题，而且平时用的centos比较多，还是在centos7上搞定了）  
### test 验证  
  
	sudo stap -ve 'probe begin{log("hello world!");exit();}'  

暂时未安装kernel debuginfo，无法probe系统接口，可probe用户接口  
test_stp.c:  
	
	#include <stdlib.h>
	#include <stdio.h>
	
	void func() {
		// just for test
		return;
	}
	
	int main(int argc, char* argv[]) {
		while (1) {
			sleep(1);
			func();
		}
		return 0;
	}
编译：
	
	gcc -g test_stp.c -o test_stp
test_stp.stp:
	
	#!/usr/bin/env stap
	probe process("./test_stp").function("func") {
		log("func is called");
	}

	
kernel相关CentOS系列需要安装[kernel-debuginfo-\`uname -r\`, kernel-debuginfo-common-\`uname -r\`](http://debuginfo.centos.org/7/x86_64/)  
systemtap 无论对于用户态程序还是内核态接口均有很强大的trace功能  
