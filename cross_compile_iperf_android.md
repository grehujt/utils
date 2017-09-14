# ndk_build_iperf3.md

- get src code from https://github.com/esnet/iperf/releases
- untar & cd to the repo root
- chmod +x configure && ./configure
- cp -r src jni
- cd jni
- vi Android.mk

```
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)

LOCAL_LDLIBS := -llog

LOCAL_CFLAGS += -fPIE -pie
#LOCAL_LDFLAGS += -fPIE

LOCAL_MODULE := iperf3
LOCAL_SRC_FILES :=   cjson.c \
                     dscp.c \
                     iperf_api.c \
                     iperf_auth.c \
                     iperf_client_api.c \
                     iperf_error.c \
                     iperf_locale.c \
                     iperf_sctp.c \
                     iperf_server_api.c \
                     iperf_tcp.c \
                     iperf_udp.c \
                     iperf_util.c \
                     main.c \
                     net.c \
                     tcp_info.c \
                     tcp_window_size.c \
                     timer.c \
                     units.c

include $(BUILD_EXECUTABLE)

```

- vi Application.mk

```
APP_ABI := all
APP_PLATFORM := android-22

```

- vi iperf_api.c to change the tmp dir to /data/data/com.example.appdemo for iperf3.XXXXXX
- vi net.c: change sys/fcntl.h into fcntl.h
- docker pull snowdream/android-ndk
- docker run --rm -ti -v /path/to/jni:/opt/jni snowdream/android-ndk bash
- cd /opt && ndk-build
