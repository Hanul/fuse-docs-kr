#Uno에서 네이티브 API 사용하기
이 장에서는 Uno에서 Android와 iOS의 네이티브 API 사용에 대한 내용을 다룹니다.

## Android API 사용하기
`.unoproj`파일을 생성하고 `Android`패키지를 다음과 같이 포함시켜줍니다:
```
{
"RootNamespace":"",
    "Packages": [
        ... other packages ...
        ,"Android"
    ],
    "Includes": [ "*" ]
}
```
이제 Uno 코드에서 Android API를 사용할 수 있습니다. 다음은 `RingtoneManager`를 이용하여 시스템 사운드에 접근하는 예제입니다.

```
using global::Android.android.media;
using global::Android.android.app;

extern(Android) class AndroidSystemSoundsProvider : ISystemSoundsProvider
{

    public void PlayNotification()
    {
        var uri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
        var ringtone = RingtoneManager.getRingtone(Activity.GetAppActivity(), uri);
        ringtone.play();
    }
}
```

> ++[Android 개발자 사이트](http://developer.android.com/reference/android/media/RingtoneManager.html)++에서 이 API에 대한 전체 개요를 찾을 수 있습니다.

##iOS API 사용하기
`.unoproj`파일을 생성하고 `Experimental.iOS`와 `ObjC`패키지를 다음과 같이 포함시켜줍니다:
```
{
"RootNamespace":"",
    "Packages": [
        ... other packages ...
        "Experimental.iOS",
        "ObjC"
    ],
    "Includes": [ "*" ]
}
```

위의 Android API 예제에서 보았던 사운드 예제를 마찬가지로 iOS에서 해보겠습니다.

```
using global::iOS.AudioToolbox;

extern(iOS) class iOSSystemSoundsProvider : ISystemSoundsProvider
{
    public void PlayNotification()
    {
        Functions.AudioServicesPlaySystemSound(1310);
    }
}
```

> ++[iOS 개발자 라이브러리](https://developer.apple.com/library/prerelease/ios/documentation/AudioToolbox/Reference/SystemSoundServicesReference/index.html#//apple_ref/c/func/AudioServicesPlaySystemSound)++에서 이 API에 대한 전체 개요를 찾아볼 수 있습니다.

## 새로운 FuseJS 모듈 생성하기
Fuse는 모든 플랫폼에 통합된 JavaScript API를 사용할 수 있도록 대상의 특정 코드를 모듈화할 수 있는 방법을 제공합니다.

- ++[UXL 핸드북](https://www.fusetools.com/developers/guides/uxl-handbook)++에서 특정 대상에 대한 컴파일의 자세한 설명을 볼 수 있습니다.
- Uno를 통해 JavaScript 모듈을 생성하기위한 예제는 ++[여기](https://www.fusetools.com/community/guides/fusejs/nativemodules)++서 제공합니다.

다음 예제와 같이, `extern`과 `if defined` 문법, `NativeModule` 생성을 사용하여 각각 빌드한 플랫폼에 따라 적합한 방법을 선택할 수 있습니다.
```
using Uno;
using Fuse.Scripting;
using Fuse.Reactive;

using Android.android.media;
using Android.android.app;
using iOS.AudioToolbox;

public class SystemSounds : NativeModule
{
    public SystemSounds()
    {
        AddMember(new NativeFunction("playNotification", (NativeCallback)PlayNotification));
    }

    object PlayNotification(Context c, object[] args)
    {
        if defined(Android)
        {
            var uri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
            var ringtone = RingtoneManager.getRingtone(Activity.GetAppActivity(), uri);
            ringtone.play();
        }
        else if defined (iOS)
        {
            Functions.AudioServicesPlaySystemSound(1310);
        }
        return null;
    }
}
```
이제 다음과 같이 JavaScript를 이용한 네이티브 기능을 호출할 수 있습니다.
```
<App Theme="Basic">
    <Panel>
        <SystemSounds ux:Global="SystemSounds"/>
        <JavaScript>
            var SystemSounds = require('SystemSounds');

            function playSound(){
                SystemSounds.playNotification();
            }

            module.exports = { playSound: playSound };
        </JavaScript>
        <Button Clicked="{playSound}" Text="Click me!" Alignment="Center" />
    </Panel>
</App>
```
