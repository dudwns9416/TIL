# Dex(Dalvik Executable)
- 안드로이드에서는 JAVA의 class를 사용하지 않고 class -> dex로 변환하여 사용하고 있다.
dex 파일은 메소드 개수가 64K를 초과할 수 없다.
이를 해결하기 위해 MUltiDex가 생겼다.
# MultiDex
Android 5.0(API 레벨 21) 이상에서는 ART라는 런타임을 사용합니다. ART는 앱 설치 시에 사전 컴파일을 수행합니다. 이 때 `classesN.dex` 파일을 스캔하고, Android 기기가 실행할 수 있도록 이 파일을 단일 `.oat` 파일로 컴파일합니다. 그러므로 `minSdkVersion`이 21 이상이라면 multidex 지원 라이브러리가 필요 없습니다.
## MultiDex 앱 구성
`minSdkVersion`이 21이상이면 `multiDexEnabled를 true로 설정하기만 하면 된다.
```
    android {
        compileSdkVersion 26
        buisVersion '26.0.2'
    
        defaultConfig {
            ..
            multiDexEnabled true
            ..
        }
        ..
    }
```
`minSdkVersion`이 20이하이면 라이브러리를 사용해야 한다.
```    
    dependencies {
        compile('com.android.support:multidex:1.0.2')
    }
```

```
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        package="com.sample.multidextest"
    >
    
    <application
            android:name="com.sample.multidextest.MyApplication"        
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:largeHeap="true"      
            android:theme="@style/AppTheme" >
            <activity
            ..
            </activity>
    
    </application>
```

```
    public class MyApplication extends MultiDexApplication{
    
        ..
        
        @Override
        protected void attachBaseContext(Context base) {
            super.attachBaseContext(base);
            MultiDex.install(this);
        }
    
        ..
    }
```

### 제한사항
Android 4.0(API 레벨 14)에서는 할당 제한이 늘어났지만, Android 5.0(API 레벨 21) 미만의 Android 버전에서는 앱이 Dalvik linearAlloc 제한에 걸릴 수도 있습니다.