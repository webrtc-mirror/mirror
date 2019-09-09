# webrtc-mirror



`学而思网校` 提供的WebRTC国内加速镜像


## 项目背景

在构建学而思的低延迟互动直播网络的过程中需要经常的编译WebRTC，但由于WebRTC依赖较多（十几个G）， 而且大多数内容需要翻墙，这对我们编译工作造成很多困扰，我们尝试了多种加速编译的方法，最终找到一个对WebRTC代码没有任何侵入的镜像方案。
相信行业内的很多人都体验过WebRTC编译的痛苦，尤其是新接触WebRTC的人，我们决定提供这个WebRTC镜像的方案。快乐自己造福大家。


## 原理

WebRTC的依赖主要分为三类:

1，放在git中的代码，这部分大概有三十多个仓库， `gcient sync` 会把这些git仓库的历史记录都拉下来，所以有的厂库会非常大，这些代码大都在google code上， 在镜像的时候我们把这些git代码同步到了gitlab， 之所以放到gitlab上是因为github上对仓库大小会做限制，导致有些代码同步不成功。这些依赖会10分钟同步一次，保持跟google code上代码仓库保持一致。

2，cipd的模块代码， 这部分代码放在appspot.com上面，这部分代码不好做镜像，可以通过采用http代理的方式来进行下载。

3，google storage上的依赖， google storage上的内容非常庞大， 不好做镜像， 可以通过采用http代理的方式进行下载。


在同步WebRTC的依赖过程中，git中的代码会从gitlab相对应的仓库中拉取， 不好镜像的部分我们提供了http代理进行下载。


## 镜像说明

- 对WebRTC源码不做任何修改
- 每十分钟和官方代码同步一次


## 感谢

- 我们的镜像方案参考了声网的方案，具体可以看 https://rtcdeveloper.com/t/topic/14914
- 感谢gitlab承载了部分webrtc代码和第三方依赖的代码，https://gitlab.com/webrtc-mirror
- 感谢学而思网校提供的服务器带宽资源  




## 编译步骤



#### 替换git仓库地址

```
git config --global url.https://gitlab.com/webrtc-mirror/webrtc.git.insteadOf https://chromium.googlesource.com/external/webrtc.git
git config --global url.https://gitlab.com/webrtc-mirror/base.git.insteadOf https://chromium.googlesource.com/chromium/src/base
git config --global url.https://gitlab.com/webrtc-mirror/build.git.insteadOf https://chromium.googlesource.com/chromium/src/build
git config --global url.https://gitlab.com/webrtc-mirror/buildtools.git.insteadOf https://chromium.googlesource.com/chromium/src/buildtools
git config --global url.https://gitlab.com/webrtc-mirror/gradle.git.insteadOf https://chromium.googlesource.com/external/github.com/gradle/gradle.git
git config --global url.https://gitlab.com/webrtc-mirror/ios.git.insteadOf https://chromium.googlesource.com/chromium/src/ios.git
git config --global url.https://gitlab.com/webrtc-mirror/testing.git.insteadOf https://chromium.googlesource.com/chromium/src/testing
git config --global url.https://gitlab.com/webrtc-mirror/third_party.git.insteadOf https://chromium.googlesource.com/chromium/src/third_party
git config --global url.https://gitlab.com/webrtc-mirror/clang-format.git.insteadOf https://chromium.googlesource.com/chromium/llvm-project/cfe/tools/clang-format.git
git config --global url.https://gitlab.com/webrtc-mirror/libcxx.git.insteadOf https://chromium.googlesource.com/chromium/llvm-project/libcxx.git
git config --global url.https://gitlab.com/webrtc-mirror/libcxxabi.git.insteadOf https://chromium.googlesource.com/chromium/llvm-project/libcxxabi.git
git config --global url.https://gitlab.com/webrtc-mirror/libunwind.git.insteadOf https://chromium.googlesource.com/external/llvm.org/libunwind.git
git config --global url.https://gitlab.com/webrtc-mirror/android_ndk.git.insteadOf https://chromium.googlesource.com/android_ndk.git
git config --global url.https://gitlab.com/webrtc-mirror/android_tools.git.insteadOf https://chromium.googlesource.com/android_tools.git
git config --global url.https://gitlab.com/webrtc-mirror/auto.git.insteadOf https://chromium.googlesource.com/external/github.com/google/auto.git
git config --global url.https://gitlab.com/webrtc-mirror/catapult.git.insteadOf https://chromium.googlesource.com/catapult.git
git config --global url.https://gitlab.com/webrtc-mirror/compact_enc_det.git.insteadOf https://chromium.googlesource.com/external/github.com/google/compact_enc_det.git
git config --global url.https://gitlab.com/webrtc-mirror/colorama.git.insteadOf https://chromium.googlesource.com/external/colorama.git
git config --global url.https://gitlab.com/webrtc-mirror/depot_tools.git.insteadOf https://chromium.googlesource.com/chromium/tools/depot_tools.git
git config --global url.https://gitlab.com/webrtc-mirror/errorprone.git.insteadOf https://chromium.googlesource.com/chromium/third_party/errorprone.git
git config --global url.https://gitlab.com/webrtc-mirror/ffmpeg.git.insteadOf https://chromium.googlesource.com/chromium/third_party/ffmpeg.git
git config --global url.https://gitlab.com/webrtc-mirror/findbugs.git.insteadOf https://chromium.googlesource.com/chromium/deps/findbugs.git
git config --global url.https://gitlab.com/webrtc-mirror/freetype2.git.insteadOf https://chromium.googlesource.com/chromium/src/third_party/freetype2.git
git config --global url.https://gitlab.com/webrtc-mirror/harfbuzz.git.insteadOf https://chromium.googlesource.com/external/github.com/harfbuzz/harfbuzz.git
git config --global url.https://gitlab.com/webrtc-mirror/gtest-parallel.git.insteadOf https://chromium.googlesource.com/external/github.com/google/gtest-parallel
git config --global url.https://gitlab.com/webrtc-mirror/googletest.git.insteadOf https://chromium.googlesource.com/external/github.com/google/googletest.git
git config --global url.https://gitlab.com/webrtc-mirror/icu.git.insteadOf https://chromium.googlesource.com/chromium/deps/icu.git
git config --global url.https://gitlab.com/webrtc-mirror/jsr-305.git.insteadOf https://chromium.googlesource.com/external/jsr-305.git
git config --global url.https://gitlab.com/webrtc-mirror/jsoncpp.git.insteadOf https://chromium.googlesource.com/external/github.com/open-source-parsers/jsoncpp.git
git config --global url.https://gitlab.com/webrtc-mirror/junit.git.insteadOf https://chromium.googlesource.com/external/junit.git
git config --global url.https://gitlab.com/webrtc-mirror/fuzzer.git.insteadOf https://chromium.googlesource.com/chromium/llvm-project/compiler-rt/lib/fuzzer.git
git config --global url.https://gitlab.com/webrtc-mirror/libjpeg_turbo.git.insteadOf https://chromium.googlesource.com/chromium/deps/libjpeg_turbo.git
git config --global url.https://gitlab.com/webrtc-mirror/libsrtp.git.insteadOf https://chromium.googlesource.com/chromium/deps/libsrtp.git
git config --global url.https://gitlab.com/webrtc-mirror/libvpx.git.insteadOf https://chromium.googlesource.com/webm/libvpx.git
git config --global url.https://gitlab.com/webrtc-mirror/libyuv.git.insteadOf https://chromium.googlesource.com/libyuv/libyuv.git
git config --global url.https://gitlab.com/webrtc-mirror/linux-syscall-support.git.insteadOf https://chromium.googlesource.com/linux-syscall-support.git
git config --global url.https://gitlab.com/webrtc-mirror/mockito.git.insteadOf https://chromium.googlesource.com/external/mockito/mockito.git
git config --global url.https://gitlab.com/webrtc-mirror/nasm.git.insteadOf https://chromium.googlesource.com/chromium/deps/nasm.git
git config --global url.https://gitlab.com/webrtc-mirror/openh264.git.insteadOf https://chromium.googlesource.com/external/github.com/cisco/openh264
git config --global url.https://gitlab.com/webrtc-mirror/requests.git.insteadOf https://chromium.googlesource.com/external/github.com/kennethreitz/requests.git
git config --global url.https://gitlab.com/webrtc-mirror/robolectric.git.insteadOf https://chromium.googlesource.com/external/robolectric.git
git config --global url.https://gitlab.com/webrtc-mirror/ub-uiautomator.git.insteadOf https://chromium.googlesource.com/chromium/third_party/ub-uiautomator.git
git config --global url.https://gitlab.com/webrtc-mirror/usrsctp.git.insteadOf https://chromium.googlesource.com/external/github.com/sctplab/usrsctp
git config --global url.https://gitlab.com/webrtc-mirror/binaries.git.insteadOf https://chromium.googlesource.com/chromium/deps/yasm/binaries.git
git config --global url.https://gitlab.com/webrtc-mirror/patched-yasm.git.insteadOf https://chromium.googlesource.com/chromium/deps/yasm/patched-yasm.git
git config --global url.https://gitlab.com/webrtc-mirror/tools.git.insteadOf https://chromium.googlesource.com/chromium/src/tools
git config --global url.https://gitlab.com/webrtc-mirror/client-py.git.insteadOf https://chromium.googlesource.com/infra/luci/client-py.git
git config --global url.https://gitlab.com/webrtc-mirror/boringssl.git.insteadOf https://boringssl.googlesource.com/boringssl.git
```


#### 安装depot_tools


```
export WORKSPACE=$(pwd)
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
export PATH=$WORKSPACE/depot_tools:$PATH
```

#### 配置http和https代理

```
// http和https代理服务，因为是代理外网服务，有可能出现间歇性的不稳定，如果不工作可以提issuse
// 请不要滥用该http和https代理服务，后面会加上黑名单服务

export http_proxy=http://39.105.13.136:8080
export https_proxy=http://39.105.13.136:8080
```

#### 同步WebRTC

```
gclient config --name src https://chromium.googlesource.com/external/webrtc.git

gclient sync 
```


#### 编译WebRTC


mac平台

```
cd src

// 可以加入其它的编译参数
gn gen  out/mac --args='is_debug=false target_cpu="x64" rtc_include_tests=false rtc_build_tools=false rtc_build_examples=false'

// mac_framework_objc 为framework， 可以为其它的target
ninja -C out/mac  mac_framework_objc
```

iOS平台

```
cd src

python tools_webrtc/ios/build_ios_libs.py  --output-dir  out/ios  --arch arm64  --extra-gn-args rtc_include_tests=false rtc_build_tools=false rtc_build_examples=false
```


Linux平台
```
cd src

// 安装依赖
bash build/install-build-deps.sh 

// 可以加入其它的编译参数
gn gen  out/linux --args='is_debug=false target_cpu="x64" rtc_include_tests=false rtc_build_tools=false rtc_build_examples=false'


ninja -C out/linux 
```

Android 平台（须在linux平台上编译）

```

# 添加安卓平台
echo "target_os = [ 'android' ]" >>  .gclient

gclient sync 

cd src

// 安装android依赖
./build/install-build-deps-android.sh

python tools_webrtc/android/build_aar.py  --build-dir out/android  --arch armeabi-v7a   --extra-gn-args rtc_include_tests=false rtc_build_tools=false rtc_build_examples=false
```

Windows 平台

```

gn gen out/Win  --args='proprietary_codecs=true  is_debug=false target_cpu="x86"  ffmpeg_branding="Chrome" rtc_include_tests=false'

ninja -C out/Win

```


#### 清空http和https代理

由于对http和https代理做了相应的白名单处理， 使用该http代理后访问其它的网站会被禁止， 在编译完WebRTC需要把http和https代理设置为空

```

export http_proxy=''
export https_proxy=''

```



## 注意 

- 为了加快下载和编译我们目前禁止掉了测试文件的下载， 所以在编译的时候请加上 `rtc_include_tests=false`
- 为了节省代理流量，我们对通过代理的域名进行了过滤，只允许WebRTC相关域名通过


## 最后

我们也在招聘WebRTC方向的人才，共同打造引领教育行业的音视频实时互动产品，有意向可以把简历发送至 `liulianxiang@100tal.com`





