# SandHook
- Android ART Hook
- Native Inline Hook

Repackage 'de.robv.android.xposed' to 'de.robv.android.xposedcompat' to avoid Xposed detection.

## Version

Based on SandHook 4.2.1, commit 6fc401bd

## Chinese

[中文文档以及实现](https://github.com/ganyao114/SandHook/blob/master/doc/doc.md)

[中文 Blog](https://blog.csdn.net/ganyao939543405/article/details/86661040)  

QQ Group：756071167  

与 VirtualApp 相关的商业合作请联系 QQ939543405

# arch support 

- ARM64
- ARM32(no tested)
- Thumb-2

# OS

4.4(ART Runtime) - 11.0 dev-preview-1

# Project Struct

- annotation<br/>
annotation api
- hooklib<br/>
core lib of art hook
- nativehook<br/>
lib of native hook
- xposedcompat<br/>
stable implement of xposed api compat for sandhook
- xposedcompat_new<br/>
annother implement of xposed api compat for sandhook(hook more fast first time)
- hookers<br/>
hook plugin demo for annotation api

# how to use

Add jitpack repository first:

```gradle
allprojects {
    repositories { maven { url 'https://jitpack.io' } }
}
```

Then add these dependencies:

```gradle
    implementation 'com.github.DevDengChao.SandHook:hooklib:4.2.1'
    implementation 'com.github.DevDengChao.SandHook:xposedcompat:4.2.1'
```

## Xposed API

--------------------------------------------------------------------

Now you can use Xposed api:

```java

//setup for xposed
//for xposed compat only(no need xposed comapt new)
XposedCompat.cacheDir = getCacheDir();

//for load xp module(sandvxp)
XposedCompat.context = this;
XposedCompat.classLoader = getClassLoader();
XposedCompat.isFirstApplication= true;

//do hook
XposedHelpers.findAndHookMethod(Activity.class, "onResume", new XC_MethodHook() {
      @Override
      protected void beforeHookedMethod(MethodHookParam param) throws Throwable {
          super.beforeHookedMethod(param);
          Log.e("XposedCompat", "beforeHookedMethod: " + param.method.getName());
      }

      @Override
      protected void afterHookedMethod(MethodHookParam param) throws Throwable {
          super.afterHookedMethod(param);
          Log.e("XposedCompat", "afterHookedMethod: " + param.method.getName());
      }
});

```

# Notice

## Disable Inline

### JIT inline

We can do nothing to prevent some methods been inlined before app start, but we can try to disable VM Jit Inline after launch.

if you will hook some method that could be inlined, please call SandHook.disableVMInline()(OS >= 7.0) in Application.OnCreate()

### Inline by dex2oat

#### Background dex2oat

SandHook.tryDisableProfile(getPackageName());

#### dex2oat by DexClassLoader

SandHook.disableDex2oatInline(fullyDisableDex2oat);

or

ArtDexOptimizer.dexoatAndDisableInline to dex2oat manuly 

### Deoptimize(Boot Image)

You can also deoptimize a caller that inlined your hook method by SandHook.deCompile(caller), just implement >= 7.0

## Hidden API

SandHook.passApiCheck();

To bypass hidden api on P & Q

## Debuggable

You must set debuggble of the target hook process before init when OS >= 8.0.  

SandHookConfig.DEBUG = <Debuggable of target process>

## more

- disassembler (only implement important instructions)
- assembler (only implement important instructions)

# Demo

## SandVXPosed

non-Root Xposed Environment Demo (VirtualApp with SandHook):

https://github.com/ganyao114/SandVXposed

## EdXposed(SandHook Brunch)

Unofficial xposed framework >= 8.0

See release above

https://github.com/ElderDrivers/EdXposed

# Android Q(10.0)

in MyApp.java

//if you want test Android Q, please set true, because SDK_INT of Android Q is still 28
public final static boolean testAndroidQ = false;
 

# References

- Epic:https://github.com/tiann/epic
- Yahfa:https://github.com/rk700/YAHFA


