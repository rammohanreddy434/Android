### Android hello program compilation steps.

$ cd $HOME/JLR/external
$ mkdir  JLR_tool
$ vim external/JLR_tool/Android.bp
```
cc_binary {
    name: "hello",
    srcs: ["hello.c"],
    shared_libs: [
        "libcrypto",
        "libssl",
        "libz",
    ],
    cflags: ["-Wno-error"],
    include_dirs: [
        "external/boringssl/src/include/openssl",
	"external/zlib/src/contrib/minizip"
    ],
}
```

$ vim external/JLR_tool/hello.c

```
#include<stdio.h>
int main()
{
printf("*********************hello Visteon****************\n");
return 0;
}
```

$ vim device/visteon/jlr-ricm/device.mk
`PRODUCT_PACKAGES += hello`


Note : Output binary hello is in /system/bin

------------------------------------------------------

Soong is a build system for Android, intended as a replacement for the old make-based build system. Soong reads Android.bp files, which define modules in a Bazel-like syntax. Soong itself is written in Go on top of the Blueprint framework, which in turn uses Ninja as a back-end. Ninja is designed for high efficiency, especially for incremental builds.

Before the Android 7.0 release, Android used GNU Make exclusively to describe and execute its build rules. The Make build system is widely supported and used, but at Android's scale became slow, error prone, unscalable, and difficult to test. The Soong build system provides the flexibility required for Android builds.
