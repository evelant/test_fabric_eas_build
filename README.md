# EAS build fabric and turbomodules failure example app

This repo illustrates how an android app on sdk 47 fails when built with 
fabric and turbomodules enabled. 

The app builds successfully but upon launch will crash with `java.lang.AssertionError: TurboModules are enabled, but mTurboModuleRegistry hasn't been set.`



### Build command
There is no documentation that I can find about enabling fabric with expo so I did my best to figure it out.
It's unclear what the `experiments -> turboModules` key in `app.json` actually does. 
I built with the following command to enable fabric:
```
USE_FABRIC=1 RCT_NEW_ARCH_ENABLED=1 ORG_GRADLE_PROJECT_newArchEnabled=true eas build -p android --profile development --local
```

### APK
The apk from the above command is commited to this repo. `build-1667589985318.apk`.
It will fail on launch with the stack below.


### Questions
1. Is there a documented way to build an app with fabric and turboModules? I couldn't find anything despite some references to an `experiments.enableFabric` and `enableNewArchitecture` app.json keys in some github issues.
2. What's needed to get fabric/turbomodules building successfully with EAS?

### Stack

```
FATAL EXCEPTION: mqt_native_modules
Process: com.evelant.test_fabric_eas_build, PID: 5278
java.lang.AssertionError: TurboModules are enabled, but mTurboModuleRegistry hasn't been set.
    at com.facebook.infer.annotation.Assertions.assertNotNull(Assertions.java:19)
    at com.facebook.react.bridge.CatalystInstanceImpl.getTurboModuleRegistry(CatalystInstanceImpl.java:483)
    at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:494)
    at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:478)
    at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:89)
    at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:47)
    at com.facebook.react.ReactInstanceManager.attachRootViewToInstance(ReactInstanceManager.java:1240)
    at com.facebook.react.ReactInstanceManager.setupReactContext(ReactInstanceManager.java:1182)
    at com.facebook.react.ReactInstanceManager.access$1600(ReactInstanceManager.java:136)
    at com.facebook.react.ReactInstanceManager$5$2.run(ReactInstanceManager.java:1136)
    at android.os.Handler.handleCallback(Handler.java:883)
    at android.os.Handler.dispatchMessage(Handler.java:100)
    at com.facebook.react.bridge.queue.MessageQueueThreadHandler.dispatchMessage(MessageQueueThreadHandler.java:27)
    at android.os.Looper.loop(Looper.java:237)
    at com.facebook.react.bridge.queue.MessageQueueThreadImpl$4.run(MessageQueueThreadImpl.java:228)
    at java.lang.Thread.run(Thread.java:919)
```


### ADB logs 
This is the complete ADB logcat output when running the app filtered by `package:com.evelant.test_fabric_eas_build`
```
2022-11-04 15:35:41.843  5278-5278  fb-UnpackingSoSource    com.evelant.test_fabric_eas_build    V  locked dso store /data/user/0/com.evelant.test_fabric_eas_build/lib-main
2022-11-04 15:35:41.844  5278-5278  fb-UnpackingSoSource    com.evelant.test_fabric_eas_build    V  deps mismatch on deps store: regenerating
2022-11-04 15:35:41.844  5278-5278  fb-UnpackingSoSource    com.evelant.test_fabric_eas_build    V  so store dirty: regenerating
2022-11-04 15:35:41.921  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libc++_shared.so: deferring to libdir
2022-11-04 15:35:41.921  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libdevmenureanimated.so: deferring to libdir
2022-11-04 15:35:41.922  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libevent-2.1.so: deferring to libdir
2022-11-04 15:35:41.922  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libevent_core-2.1.so: deferring to libdir
2022-11-04 15:35:41.922  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libevent_extra-2.1.so: deferring to libdir
2022-11-04 15:35:41.922  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libexpo-modules-core.so: deferring to libdir
2022-11-04 15:35:41.922  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libfabricjni.so: deferring to libdir
2022-11-04 15:35:41.922  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libfb.so: deferring to libdir
2022-11-04 15:35:41.922  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libfbjni.so: deferring to libdir
2022-11-04 15:35:41.922  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libflipper.so: deferring to libdir
2022-11-04 15:35:41.922  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libfolly_runtime.so: deferring to libdir
2022-11-04 15:35:41.923  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libgifimage.so: deferring to libdir
2022-11-04 15:35:41.923  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libglog.so: deferring to libdir
2022-11-04 15:35:41.923  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libglog_init.so: deferring to libdir
2022-11-04 15:35:41.923  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libimagepipeline.so: deferring to libdir
2022-11-04 15:35:41.923  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libjsc.so: deferring to libdir
2022-11-04 15:35:41.923  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libjscexecutor.so: deferring to libdir
2022-11-04 15:35:41.923  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libjsi.so: deferring to libdir
2022-11-04 15:35:41.923  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libjsijniprofiler.so: deferring to libdir
2022-11-04 15:35:41.923  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libjsinspector.so: deferring to libdir
2022-11-04 15:35:41.924  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/liblogger.so: deferring to libdir
2022-11-04 15:35:41.924  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libmapbufferjni.so: deferring to libdir
2022-11-04 15:35:41.924  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libnative-filters.so: deferring to libdir
2022-11-04 15:35:41.924  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libnative-imagetranscoder.so: deferring to libdir
2022-11-04 15:35:41.924  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_codegen_rncore.so: deferring to libdir
2022-11-04 15:35:41.924  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_config.so: deferring to libdir
2022-11-04 15:35:41.924  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_debug.so: deferring to libdir
2022-11-04 15:35:41.924  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_nativemodule_core.so: deferring to libdir
2022-11-04 15:35:41.924  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_animations.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_attributedstring.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_componentregistry.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_core.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_debug.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_graphics.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_imagemanager.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_leakchecker.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_mapbuffer.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_mounting.so: deferring to libdir
2022-11-04 15:35:41.925  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_runtimescheduler.so: deferring to libdir
2022-11-04 15:35:41.926  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_scheduler.so: deferring to libdir
2022-11-04 15:35:41.926  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_telemetry.so: deferring to libdir
2022-11-04 15:35:41.926  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_templateprocessor.so: deferring to libdir
2022-11-04 15:35:41.926  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_textlayoutmanager.so: deferring to libdir
2022-11-04 15:35:41.926  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_render_uimanager.so: deferring to libdir
2022-11-04 15:35:41.926  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreact_utils.so: deferring to libdir
2022-11-04 15:35:41.926  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreactnativeblob.so: deferring to libdir
2022-11-04 15:35:41.926  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreactnativejni.so: deferring to libdir
2022-11-04 15:35:41.926  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libreactperfloggerjni.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/librrc_image.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/librrc_root.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/librrc_scrollview.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/librrc_text.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/librrc_textinput.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/librrc_unimplementedview.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/librrc_view.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libruntimeexecutor.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libstatic-webp.so: deferring to libdir
2022-11-04 15:35:41.927  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libtestfabriceasbuild_appmodules.so: deferring to libdir
2022-11-04 15:35:41.928  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libturbomodulejsijni.so: deferring to libdir
2022-11-04 15:35:41.928  5278-5278  ApkSoSource             com.evelant.test_fabric_eas_build    D  not allowing consideration of lib/arm64-v8a/libyoga.so: deferring to libdir
2022-11-04 15:35:41.928  5278-5278  fb-UnpackingSoSource    com.evelant.test_fabric_eas_build    V  regenerating DSO store com.facebook.soloader.ApkSoSource
2022-11-04 15:35:41.929  5278-5278  fb-UnpackingSoSource    com.evelant.test_fabric_eas_build    V  Finished regenerating DSO store com.facebook.soloader.ApkSoSource
2022-11-04 15:35:41.929  5278-5278  fb-UnpackingSoSource    com.evelant.test_fabric_eas_build    V  starting syncer worker
2022-11-04 15:35:41.940  5278-5278  fb-UnpackingSoSource    com.evelant.test_fabric_eas_build    V  releasing dso store lock for /data/user/0/com.evelant.test_fabric_eas_build/lib-main (from syncer thread)
2022-11-04 15:35:41.940  5278-5278  fb-UnpackingSoSource    com.evelant.test_fabric_eas_build    V  not releasing dso store lock for /data/user/0/com.evelant.test_fabric_eas_build/lib-main (syncer thread started)
2022-11-04 15:35:42.015  5278-5278  NetworkSecurityConfig   com.evelant.test_fabric_eas_build    D  No Network Security Config specified, using platform default
2022-11-04 15:35:42.045  5278-5278  SensorManager           com.evelant.test_fabric_eas_build    D  registerListener :: 28, lsm6dsm Accelerometer Non-wakeup, 66667, 0,  
2022-11-04 15:35:42.055  5278-5491  SoLoader                com.evelant.test_fabric_eas_build    V  libfbjni.so not found on /data/data/com.evelant.test_fabric_eas_build/lib-main
2022-11-04 15:35:42.055  5278-5491  SoLoader                com.evelant.test_fabric_eas_build    D  libfbjni.so found on /data/app/com.evelant.test_fabric_eas_build-w2Fd5SSAgSBmM8ZAe9cMZA==/lib/arm64
2022-11-04 15:35:42.055  5278-5491  SoLoader                com.evelant.test_fabric_eas_build    D  Not resolving dependencies for libfbjni.so
2022-11-04 15:35:42.056  5278-5491  linker                  com.evelant.test_fabric_eas_build    W  Warning: "/data/app/com.evelant.test_fabric_eas_build-w2Fd5SSAgSBmM8ZAe9cMZA==/lib/arm64/libc++_shared.so" unused DT entry: unknown processor-specific (type 0x70000001 arg 0x0) (ignoring)
2022-11-04 15:35:42.059  5278-5491  SoLoader                com.evelant.test_fabric_eas_build    V  libflipper.so not found on /data/data/com.evelant.test_fabric_eas_build/lib-main
2022-11-04 15:35:42.059  5278-5491  SoLoader                com.evelant.test_fabric_eas_build    D  libflipper.so found on /data/app/com.evelant.test_fabric_eas_build-w2Fd5SSAgSBmM8ZAe9cMZA==/lib/arm64
2022-11-04 15:35:42.060  5278-5491  SoLoader                com.evelant.test_fabric_eas_build    D  Not resolving dependencies for libflipper.so
2022-11-04 15:35:42.083  5278-5495  TcpOptimizer            com.evelant.test_fabric_eas_build    D  TcpOptimizer-ON
2022-11-04 15:35:42.096  5278-5278  abric_eas_buil          com.evelant.test_fabric_eas_build    W  Accessing hidden field Landroid/view/View;->mKeyedTags:Landroid/util/SparseArray; (greylist, reflection, allowed)
2022-11-04 15:35:42.097  5278-5278  abric_eas_buil          com.evelant.test_fabric_eas_build    W  Accessing hidden field Landroid/view/View;->mListenerInfo:Landroid/view/View$ListenerInfo; (greylist, reflection, allowed)
2022-11-04 15:35:42.097  5278-5278  abric_eas_buil          com.evelant.test_fabric_eas_build    W  Accessing hidden field Landroid/view/View$ListenerInfo;->mOnClickListener:Landroid/view/View$OnClickListener; (greylist, reflection, allowed)
2022-11-04 15:35:42.100  5278-5278  flipper                 com.evelant.test_fabric_eas_build    I  flipper: FlipperClient::addPlugin Inspector
2022-11-04 15:35:42.100  5278-5278  flipper                 com.evelant.test_fabric_eas_build    I  flipper: FlipperClient::addPlugin React
2022-11-04 15:35:42.102  5278-5278  flipper                 com.evelant.test_fabric_eas_build    I  flipper: FlipperClient::addPlugin Databases
2022-11-04 15:35:42.103  5278-5278  flipper                 com.evelant.test_fabric_eas_build    I  flipper: FlipperClient::addPlugin Preferences
2022-11-04 15:35:42.103  5278-5278  flipper                 com.evelant.test_fabric_eas_build    I  flipper: FlipperClient::addPlugin CrashReporter
2022-11-04 15:35:42.106  5278-5278  flipper                 com.evelant.test_fabric_eas_build    I  flipper: FlipperClient::addPlugin Network
2022-11-04 15:35:42.119  5278-5491  System.err              com.evelant.test_fabric_eas_build    W  SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
2022-11-04 15:35:42.119  5278-5491  System.err              com.evelant.test_fabric_eas_build    W  SLF4J: Defaulting to no-operation (NOP) logger implementation
2022-11-04 15:35:42.119  5278-5491  System.err              com.evelant.test_fabric_eas_build    W  SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
2022-11-04 15:35:42.290  5278-5278  abric_eas_buil          com.evelant.test_fabric_eas_build    W  Accessing hidden field Ljava/lang/reflect/Field;->accessFlags:I (greylist, reflection, allowed)
2022-11-04 15:35:42.298  5278-5278  PhoneWindow             com.evelant.test_fabric_eas_build    D  forceLight changed to true [] from com.android.internal.policy.PhoneWindow.updateForceLightNavigationBar:4274 com.android.internal.policy.DecorView.updateColorViews:1547 com.android.internal.policy.PhoneWindow.dispatchWindowAttributesChanged:3252 android.view.Window.setFlags:1153 com.android.internal.policy.PhoneWindow.generateLayout:2474 
2022-11-04 15:35:42.299  5278-5278  MultiWindowDecorSupport com.evelant.test_fabric_eas_build    I  [INFO] isPopOver = false
2022-11-04 15:35:42.299  5278-5278  MultiWindowDecorSupport com.evelant.test_fabric_eas_build    I  updateCaptionType >> DecorView@39c8343[], isFloating: false, isApplication: true, hasWindowDecorCaption: false, hasWindowControllerCallback: true
2022-11-04 15:35:42.299  5278-5278  MultiWindowDecorSupport com.evelant.test_fabric_eas_build    D  setCaptionType = 0, DecorView = DecorView@39c8343[]
2022-11-04 15:35:42.317  5278-5278  abric_eas_buil          com.evelant.test_fabric_eas_build    W  Accessing hidden method Landroid/view/View;->computeFitSystemWindows(Landroid/graphics/Rect;Landroid/graphics/Rect;)Z (greylist, reflection, allowed)
2022-11-04 15:35:42.317  5278-5278  abric_eas_buil          com.evelant.test_fabric_eas_build    W  Accessing hidden method Landroid/view/ViewGroup;->makeOptionalFitsSystemWindows()V (greylist, reflection, allowed)
2022-11-04 15:35:42.325  5278-5278  SensorManager           com.evelant.test_fabric_eas_build    D  unregisterListener ::   
2022-11-04 15:35:42.351  5278-5278  ExpoDevMenu             com.evelant.test_fabric_eas_build    I  Set new dev-menu delegate: class expo.modules.devmenu.DevMenuDefaultDelegate
2022-11-04 15:35:42.358  5278-5278  SensorManager           com.evelant.test_fabric_eas_build    D  registerListener :: 28, lsm6dsm Accelerometer Non-wakeup, 66667, 0,  
2022-11-04 15:35:42.396  5278-5278  ViewRootIm...nActivity] com.evelant.test_fabric_eas_build    I  setView = com.android.internal.policy.DecorView@39c8343 TM=true MM=false
2022-11-04 15:35:42.470  5278-5278  ViewRootIm...nActivity] com.evelant.test_fabric_eas_build    I  Relayout returned: old=(0,0,1440,2960) new=(0,0,1440,2960) req=(1440,2960)0 dur=3 res=0x1 s={false 0} ch=false
2022-11-04 15:35:42.471  5278-5278  ViewRootIm...nActivity] com.evelant.test_fabric_eas_build    E  Surface is not valid.
2022-11-04 15:35:42.471  5278-5278  ActivityThread          com.evelant.test_fabric_eas_build    W  handleWindowVisibility: no activity for token android.os.BinderProxy@328b187
2022-11-04 15:35:42.489  5278-5522  SoLoader                com.evelant.test_fabric_eas_build    V  libreactnativejni.so not found on /data/data/com.evelant.test_fabric_eas_build/lib-main
2022-11-04 15:35:42.489  5278-5522  SoLoader                com.evelant.test_fabric_eas_build    D  libreactnativejni.so found on /data/app/com.evelant.test_fabric_eas_build-w2Fd5SSAgSBmM8ZAe9cMZA==/lib/arm64
2022-11-04 15:35:42.489  5278-5522  SoLoader                com.evelant.test_fabric_eas_build    D  Not resolving dependencies for libreactnativejni.so
2022-11-04 15:35:42.490  5278-5278  PhoneWindow             com.evelant.test_fabric_eas_build    D  forceLight changed to true [] from com.android.internal.policy.PhoneWindow.updateForceLightNavigationBar:4274 com.android.internal.policy.DecorView.updateColorViews:1547 com.android.internal.policy.PhoneWindow.dispatchWindowAttributesChanged:3252 android.view.Window.setFlags:1153 com.android.internal.policy.PhoneWindow.generateLayout:2474 
2022-11-04 15:35:42.491  5278-5278  MultiWindowDecorSupport com.evelant.test_fabric_eas_build    I  [INFO] isPopOver = false
2022-11-04 15:35:42.491  5278-5278  MultiWindowDecorSupport com.evelant.test_fabric_eas_build    I  updateCaptionType >> DecorView@ceb0f38[], isFloating: false, isApplication: true, hasWindowDecorCaption: false, hasWindowControllerCallback: true
2022-11-04 15:35:42.491  5278-5278  MultiWindowDecorSupport com.evelant.test_fabric_eas_build    D  setCaptionType = 0, DecorView = DecorView@ceb0f38[]
2022-11-04 15:35:42.494  5278-5522  linker                  com.evelant.test_fabric_eas_build    W  Warning: "/data/app/com.evelant.test_fabric_eas_build-w2Fd5SSAgSBmM8ZAe9cMZA==/lib/arm64/libruntimeexecutor.so" unused DT entry: unknown processor-specific (type 0x70000001 arg 0x0) (ignoring)
2022-11-04 15:35:42.494  5278-5522  linker                  com.evelant.test_fabric_eas_build    W  Warning: "/data/app/com.evelant.test_fabric_eas_build-w2Fd5SSAgSBmM8ZAe9cMZA==/lib/arm64/libruntimeexecutor.so" unused DT entry: unknown processor-specific (type 0x70000001 arg 0x0) (ignoring)
2022-11-04 15:35:42.510  5278-5522  SoLoader                com.evelant.test_fabric_eas_build    V  libjscexecutor.so not found on /data/data/com.evelant.test_fabric_eas_build/lib-main
2022-11-04 15:35:42.510  5278-5522  SoLoader                com.evelant.test_fabric_eas_build    D  libjscexecutor.so found on /data/app/com.evelant.test_fabric_eas_build-w2Fd5SSAgSBmM8ZAe9cMZA==/lib/arm64
2022-11-04 15:35:42.510  5278-5522  SoLoader                com.evelant.test_fabric_eas_build    D  Not resolving dependencies for libjscexecutor.so
2022-11-04 15:35:42.513  5278-5522  JavaScriptCore.Version  com.evelant.test_fabric_eas_build    D  250230.2.1
2022-11-04 15:35:42.515  5278-5278  ViewRootIm...rActivity] com.evelant.test_fabric_eas_build    I  setView = com.android.internal.policy.DecorView@ceb0f38 TM=true MM=false
2022-11-04 15:35:42.528  5278-5278  ViewRootIm...rActivity] com.evelant.test_fabric_eas_build    I  Relayout returned: old=(0,0,1440,2960) new=(0,0,1440,2960) req=(1440,2960)0 dur=6 res=0x7 s={true 484730216448} ch=true
2022-11-04 15:35:42.529  5278-5513  AdrenoGLES              com.evelant.test_fabric_eas_build    I  QUALCOMM build                   : fdd61e0, I20154638fb
                                                                                                    Build Date                       : 10/07/20
                                                                                                    OpenGL ES Shader Compiler Version: EV031.27.05.01
                                                                                                    Local Branch                     : 
                                                                                                    Remote Branch                    : refs/tags/AU_LINUX_ANDROID_LA.UM.8.3.R1.10.00.00.520.058
                                                                                                    Remote Branch                    : NONE
                                                                                                    Reconstruct Branch               : NOTHING
2022-11-04 15:35:42.529  5278-5513  AdrenoGLES              com.evelant.test_fabric_eas_build    I  Build Config                     : S P 8.0.11 AArch64
2022-11-04 15:35:42.532  5278-5513  AdrenoGLES              com.evelant.test_fabric_eas_build    I  PFP: 0x016ee187, ME: 0x00000000
2022-11-04 15:35:42.541  5278-5522  unknown:ReactContext    com.evelant.test_fabric_eas_build    W  initializeMessageQueueThreads() is called.
2022-11-04 15:35:42.550  5278-5513  Gralloc3                com.evelant.test_fabric_eas_build    W  mapper 3.x is not supported
2022-11-04 15:35:42.557  5278-5526  AndroidRuntime          com.evelant.test_fabric_eas_build    E  FATAL EXCEPTION: mqt_native_modules
                                                                                                    Process: com.evelant.test_fabric_eas_build, PID: 5278
                                                                                                    java.lang.AssertionError: TurboModules are enabled, but mTurboModuleRegistry hasn't been set.
                                                                                                    	at com.facebook.infer.annotation.Assertions.assertNotNull(Assertions.java:19)
                                                                                                    	at com.facebook.react.bridge.CatalystInstanceImpl.getTurboModuleRegistry(CatalystInstanceImpl.java:483)
                                                                                                    	at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:494)
                                                                                                    	at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:478)
                                                                                                    	at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:89)
                                                                                                    	at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:47)
                                                                                                    	at com.facebook.react.ReactInstanceManager.attachRootViewToInstance(ReactInstanceManager.java:1240)
                                                                                                    	at com.facebook.react.ReactInstanceManager.setupReactContext(ReactInstanceManager.java:1182)
                                                                                                    	at com.facebook.react.ReactInstanceManager.access$1600(ReactInstanceManager.java:136)
                                                                                                    	at com.facebook.react.ReactInstanceManager$5$2.run(ReactInstanceManager.java:1136)
                                                                                                    	at android.os.Handler.handleCallback(Handler.java:883)
                                                                                                    	at android.os.Handler.dispatchMessage(Handler.java:100)
                                                                                                    	at com.facebook.react.bridge.queue.MessageQueueThreadHandler.dispatchMessage(MessageQueueThreadHandler.java:27)
                                                                                                    	at android.os.Looper.loop(Looper.java:237)
                                                                                                    	at com.facebook.react.bridge.queue.MessageQueueThreadImpl$4.run(MessageQueueThreadImpl.java:228)
                                                                                                    	at java.lang.Thread.run(Thread.java:919)
2022-11-04 15:35:42.559  5278-5526  DevLauncher             com.evelant.test_fabric_eas_build    E  DevLauncher tries to handle uncaught exception.
                                                                                                    java.lang.AssertionError: TurboModules are enabled, but mTurboModuleRegistry hasn't been set.
                                                                                                    	at com.facebook.infer.annotation.Assertions.assertNotNull(Assertions.java:19)
                                                                                                    	at com.facebook.react.bridge.CatalystInstanceImpl.getTurboModuleRegistry(CatalystInstanceImpl.java:483)
                                                                                                    	at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:494)
                                                                                                    	at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:478)
                                                                                                    	at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:89)
                                                                                                    	at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:47)
                                                                                                    	at com.facebook.react.ReactInstanceManager.attachRootViewToInstance(ReactInstanceManager.java:1240)
                                                                                                    	at com.facebook.react.ReactInstanceManager.setupReactContext(ReactInstanceManager.java:1182)
                                                                                                    	at com.facebook.react.ReactInstanceManager.access$1600(ReactInstanceManager.java:136)
                                                                                                    	at com.facebook.react.ReactInstanceManager$5$2.run(ReactInstanceManager.java:1136)
                                                                                                    	at android.os.Handler.handleCallback(Handler.java:883)
                                                                                                    	at android.os.Handler.dispatchMessage(Handler.java:100)
                                                                                                    	at com.facebook.react.bridge.queue.MessageQueueThreadHandler.dispatchMessage(MessageQueueThreadHandler.java:27)
                                                                                                    	at android.os.Looper.loop(Looper.java:237)
                                                                                                    	at com.facebook.react.bridge.queue.MessageQueueThreadImpl$4.run(MessageQueueThreadImpl.java:228)
                                                                                                    	at java.lang.Thread.run(Thread.java:919)
2022-11-04 15:35:42.573  5278-5526  abric_eas_buil          com.evelant.test_fabric_eas_build    W  Accessing hidden field Lsun/misc/Unsafe;->theUnsafe:Lsun/misc/Unsafe; (greylist, reflection, allowed)
2022-11-04 15:35:42.574  5278-5526  abric_eas_buil          com.evelant.test_fabric_eas_build    W  Accessing hidden method Lsun/misc/Unsafe;->allocateInstance(Ljava/lang/Class;)Ljava/lang/Object; (greylist, reflection, allowed)
2022-11-04 15:35:42.579  5278-5278  ViewRootIm...nActivity] com.evelant.test_fabric_eas_build    I  Relayout returned: old=(0,0,1440,2960) new=(0,0,1440,2960) req=(1440,2960)8 dur=3 res=0x1 s={false 0} ch=false
2022-11-04 15:35:42.579  5278-5278  ViewRootIm...rActivity] com.evelant.test_fabric_eas_build    I  MSG_WINDOW_FOCUS_CHANGED 1 1
2022-11-04 15:35:42.580  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    D  prepareNavigationBarInfo() DecorView@ceb0f38[DevLauncherActivity]
2022-11-04 15:35:42.580  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    D  getNavigationBarColor() -855310
2022-11-04 15:35:42.581  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    D  prepareNavigationBarInfo() DecorView@ceb0f38[DevLauncherActivity]
2022-11-04 15:35:42.581  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    D  getNavigationBarColor() -855310
2022-11-04 15:35:42.581  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    V  Starting input: tba=com.evelant.test_fabric_eas_build ic=null mNaviBarColor -855310 mIsGetNaviBarColorSuccess true , NavVisible : true , NavTrans : false
2022-11-04 15:35:42.581  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    D  startInputInner - Id : 0
2022-11-04 15:35:42.581  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    I  startInputInner - mService.startInputOrWindowGainedFocus
2022-11-04 15:35:42.609  5278-5278  ViewRootIm...rActivity] com.evelant.test_fabric_eas_build    I  MSG_RESIZED_REPORT: frame=(0,0,1440,2960) ci=(0,84,0,168) vi=(0,84,0,168) or=1
2022-11-04 15:35:42.618  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    D  prepareNavigationBarInfo() DecorView@ceb0f38[DevLauncherActivity]
2022-11-04 15:35:42.619  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    D  getNavigationBarColor() -855310
2022-11-04 15:35:42.619  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    V  Starting input: tba=com.evelant.test_fabric_eas_build ic=null mNaviBarColor -855310 mIsGetNaviBarColorSuccess true , NavVisible : true , NavTrans : false
2022-11-04 15:35:42.619  5278-5278  InputMethodManager      com.evelant.test_fabric_eas_build    D  startInputInner - Id : 0
2022-11-04 15:35:42.621  5278-5278  AndroidRuntime          com.evelant.test_fabric_eas_build    D  Shutting down VM
2022-11-04 15:35:42.621  5278-5278  AndroidRuntime          com.evelant.test_fabric_eas_build    E  FATAL EXCEPTION: main
                                                                                                    Process: com.evelant.test_fabric_eas_build, PID: 5278
                                                                                                    java.lang.AssertionError: TurboModules are enabled, but mTurboModuleRegistry hasn't been set.
                                                                                                    	at com.facebook.infer.annotation.Assertions.assertNotNull(Assertions.java:19)
                                                                                                    	at com.facebook.react.bridge.CatalystInstanceImpl.getTurboModuleRegistry(CatalystInstanceImpl.java:483)
                                                                                                    	at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:494)
                                                                                                    	at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:478)
                                                                                                    	at com.facebook.react.bridge.ReactContext.getNativeModule(ReactContext.java:183)
                                                                                                    	at com.facebook.react.ReactRootView$CustomGlobalLayoutListener.emitUpdateDimensionsEvent(ReactRootView.java:1002)
                                                                                                    	at com.facebook.react.ReactRootView$CustomGlobalLayoutListener.checkForDeviceDimensionsChanges(ReactRootView.java:962)
                                                                                                    	at com.facebook.react.ReactRootView$CustomGlobalLayoutListener.onGlobalLayout(ReactRootView.java:900)
                                                                                                    	at android.view.ViewTreeObserver.dispatchOnGlobalLayout(ViewTreeObserver.java:1082)
                                                                                                    	at android.view.ViewRootImpl.performTraversals(ViewRootImpl.java:3207)
                                                                                                    	at android.view.ViewRootImpl.doTraversal(ViewRootImpl.java:2222)
                                                                                                    	at android.view.ViewRootImpl$TraversalRunnable.run(ViewRootImpl.java:9123)
                                                                                                    	at android.view.Choreographer$CallbackRecord.run(Choreographer.java:999)
                                                                                                    	at android.view.Choreographer.doCallbacks(Choreographer.java:797)
                                                                                                    	at android.view.Choreographer.doFrame(Choreographer.java:732)
                                                                                                    	at android.view.Choreographer$FrameDisplayEventReceiver.run(Choreographer.java:984)
                                                                                                    	at android.os.Handler.handleCallback(Handler.java:883)
                                                                                                    	at android.os.Handler.dispatchMessage(Handler.java:100)
                                                                                                    	at android.os.Looper.loop(Looper.java:237)
                                                                                                    	at android.app.ActivityThread.main(ActivityThread.java:8167)
                                                                                                    	at java.lang.reflect.Method.invoke(Native Method)
                                                                                                    	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:496)
                                                                                                    	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1100)
2022-11-04 15:35:42.707  5278-5525  ReactNativeJS           com.evelant.test_fabric_eas_build    E  Invariant Violation: TurboModuleRegistry.getEnforcing(...): 'PlatformConstants' could not be found. Verify that a module by this name is registered in the native binary.
2022-11-04 15:35:42.708  5278-5525  ReactNativeJS           com.evelant.test_fabric_eas_build    I  'Failed to print error: ', 'Requiring module "54", which threw an exception: Invariant Violation: TurboModuleRegistry.getEnforcing(...): \'PlatformConstants\' could not be found. Verify that a module by this name is registered in the native binary.'
2022-11-04 15:35:44.634  5278-5535  Process                 com.evelant.test_fabric_eas_build    I  Sending signal. PID: 5278 SIG: 9


```
