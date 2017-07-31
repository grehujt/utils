- get nkd environment (docker)
- [download iperf source code](https://iperf.fr/iperf-download.php) 
- extract source code to a folder (jni)
- modify tmp dir into /sdcard in iperf_api.c
- change sys/fcntl.h into fcntl.h in net.c
- write Andoird.mk in jni folder
```
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)

LOCAL_LDLIBS := -llog 

LOCAL_CFLAGS += -fPIE -pie
#LOCAL_LDFLAGS += -fPIE 

LOCAL_MODULE := iperf3
LOCAL_SRC_FILES :=   cjson.c \
	                   cjson.h \
						         dscp.c \
										 flowlabel.h \
										 iperf.h \
										 iperf_api.c \
										 iperf_api.h \
										 iperf_auth.c \
										 iperf_auth.h \
										 iperf_client_api.c \
										 iperf_error.c \
										 iperf_locale.c \
										 iperf_locale.h \
										 iperf_sctp.c \
										 iperf_sctp.h \
										 iperf_server_api.c \
										 iperf_tcp.c \
										 iperf_tcp.h \
										 iperf_udp.c \
										 iperf_udp.h \
										 iperf_util.c \
										 iperf_util.h \
										 main.c \
										 net.c \
										 net.h \
										 portable_endian.h \
										 queue.h \
										 tcp_info.c \
										 tcp_window_size.c \
										 tcp_window_size.h \
										 timer.c \
										 timer.h \
										 units.c \
										 units.h
                     
include $(BUILD_EXECUTABLE)
```
- write Application.mk in jni folder
```
APP_ABI := all
APP_PLATFORM := android-22
```
- ndk-build
- adb push bianries to /data/local/tmp to test if works
- copy binaries to assets folder in Android studio project
- In android, copy the binaries to getApplicationInfo().dataDir
- file.setReadable(true, false); file.setExecutable(true, false); file.setWritable(false, false);
