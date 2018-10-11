# Kule urBrowser
Kule urBrowser 會把使用者的作業系統、裝置以及瀏覽器等資訊紀錄下來，並且儲存於 Localstorage、Cookie 與 html 標籤上。例如：
```
<html id="mac" class="desktop chrome chrome6 webkit" data-browser-name="chrome" data-browser-version="6" data-browser-core="webkit" data-os-name="mac" data-device="desktop" data-bp="lg" data-inapp="false" data-webview="false" data-ub-version="4.0.2b">
```

***

## 使用方式
你可以[下載檔案](http://urbrowser.kule.tw/assets/js/kule.urbrowser.min.js) (ver. 4.0.3b)
並且載入到你的網頁中，urBrowser就會自動執行，建議是第一個載入順序或放在所有 Javascript 的第一行。
```
<script id="urbrowser" data-domain="你的網域" src="path/to/kule.urbrowser.min.js"></script>
```

***

## 使用說明
無論使用以上哪一種方式，你可以將你的網域加入至 data-domain 。當網頁讀取時就會自動開始執行，並且將使用者的瀏覽器、作業系統、平台等等資訊記錄下來並置放於```<html>```上，例如：
```
<html id="mac" class="desktop chrome chrome6 webkit" data-browser-name="chrome" data-browser-version="6" data-browser-core="webkit" data-os-name="mac" data-device="desktop" data-bp="lg" data-inapp="false" data-webview="false" data-ub-version="4.0.1b">
```

IE 9+, Edge 12+, Chrome 4+, Firefox 3.5+, Safari 4+, Opera 11.5+ 等瀏覽器會另外儲存至 Cookie 與 Local Storage，可隨時供 javascript 或後端語言使用，例如：
```
{"project":"urBrowser","version":"4.0.1b","author":"Kei Cheng","getNameByAgent":"chrome","getPlatform":"mac","getBrowserWithCoreName":{"origin":"chrome","name":"chrome6","core":"webkit","addon":"","full":"chrome chrome6 webkit"},"getOSName":"mac","getBrowserName":{"name":"chrome","origin":"","classes":"chrome chrome6 webkit"},"getBrowserVersion":{"int":6,"full":"6"},"getDevice":"desktop","getOrientation":"portrait","isInApp":false,"isWebView":false,"getBreakpointRange":"lg","classes":"desktop chrome chrome6 webkit"}
```

***

### 使用者的作業系統或平台
id 會記錄使用者的作業系統或平台，偶爾會發生在不同平台但相同瀏覽器的情況下發生部分的差異，所以透過 id 紀錄作業系統或是平台資訊，來處理CSS Hack，資訊包含 windows, wp(windows phone), mac, iphone, ipad, ipod, linux, android, chromeos, unix, solaris, playstation, nintendo, blackberry, freebsd, openbsd, palm, symbian，例如：
```
<html id="mac">
```

在CSS Hack上就可以寫：
```
#mac .selector {
    opacity: .5;
}
```

如果你有使用 CSS 預處理器，那麼可以這樣寫：
```
#mac {
    .selector {
        opacity: .5;
    }
}
```

***

### 使用者的裝置類型、瀏覽器資訊
urBrowser 會將使用者的裝置類型與瀏覽器資訊紀錄於```class```上，裝置類型區分 desktop 與 mobile，瀏覽器資訊則包含瀏覽器名稱、瀏覽器名稱與版號、瀏覽器核心。記錄瀏覽器核心是為了在相同核心的瀏覽器發生問題時，可以一次處理，例如Chrome, Safari, 新版的Opera 都是使用 Webkit。另外，IE 的核心名稱雖然為 Trident，但為了方便記憶，所以改為 ie，例如：
```
<html class="desktop ie11 ie">
```
```
<html class="desktop chrome chrome69 webkit">
```
```
<html class="mobile fxios firefox firefox56 webkit">
```
```
<html class="mobile safari safari12 webkit">
```
```
<html class="mobile line line8 webkit">
```

除了以上這些較為知名的瀏覽器之外，有一些瀏覽器是基於某個瀏覽器再添加各自的功能或特色，這種情況下尤其在 Chrome/Chromium 上較為明顯，此時該瀏覽器的 class 名稱則會另外新增 chrome 這個名稱，例如：
```
<html class="mobile yandex chrome webkit">
```

針對以上範例，在CSS Hack上就可以這麼寫：
```
.chrome .selector {
    opacity: .5;
}
```

如果是所有的 Webkit：
```
.webkit .selector {
    opacity: .5;
}
```

除了以上這些較為知名的瀏覽器之外，有一些瀏覽器是基於某個瀏覽器再添加各自的功能或特色，這種情況下尤其在 Chrome/Chromium 上較為明顯，此時該瀏覽器的 class 名稱則會另外新增 chrome 這個名稱，例如：
```
<html class="yandex chrome webkit">
```

針對以上範例，在CSS Hack上就可以這麼寫：
```
.ie11 .selector {
    opacity: .5;
}
```

如果是所有的IE：
```
.ie .selector {
    opacity: .5;
}
```

針對所有使用Webkit核心的瀏覽器：
```
.webkit .selector {}
```

針對Chrome, Firefox, Safari, Opera, Edge：
```
.chrome .selector {}
```
```
.firefox .selector {}
```
```
.safari .selector {}
```
```
.opera .selector {}
```
```
.edge .selector {}
```

針對Android原生瀏覽器：
```
.adrBuiltin .selector {}
```

***

### 混合使用
有時候會發生不同作業系統某個相同瀏覽器的情況下有些微不同時，必須要做些Hack，例如：
```
#mac.chrome .selector {
    opacity: .5;
}

#mac.opera .selector {
    opacity: .5;
}
```

如果你有使用 CSS 預處理器，那麼可以這樣寫：
```
#mac {
    &.chrome {
        .selector {
                opacity: .5;
        }
    }

    &.opera {
        .selector {
                opacity: .5;
        }
    }
}
```

***
### Webview
當使用 Webview 時，在某些情況下可能只需要針對 Webview 處理某些操作或是修正，因此如果你有使用 Webview 時，可在 User Agent 加上以下字串：cordova 或 phonegap 或 react 或 nodejs 或 webview 等字樣，如需分辨 iOS 與 Andoird 可如下表示：
- iOS: webview-ios
- Android: webview-android
在 ```html``` 標籤上就會顯示以下資訊：
```
<html class="webview-ios webview webkit" data-webview="ios">
```
```
<html class="webview-adr webview webkit" data-webview="android">
```
如果不是上述情況時，data-webview 則顯示為 false。

***
### 第三方 APP 內建的瀏覽器
雖然 IE 時代已經邁向終結，也不太會再遇到支援 IE 眾多鳥事，但接踵而來的卻是許多第三方 app 內建的瀏覽器，此時開發者難以知道使用者瀏覽器的版本，因此針對了 Facebook apps, Twitter, Line, Kakaotalk, MicroMessenger(微信) 等 APP 做識別，並且會將該 APP 名稱置入於 class 名稱內，並且增加 inapp 表示為內建於 app 的瀏覽器，例如：
```
<html class="mobile line line8 webkit inapp">
```

***

### 使用者的裝置
裝置資料以 Desktop, Mobile 兩種來區分，並且記錄於 class 與 data-device，例如：
```
<html class="desktop" data-device="desktop">
```

***

### 瀏覽器與系統名稱
```
<html data-browser-name="chrome" data-browser-version="6" data-os-name="mac">
```

### Breakpoints
無論使用 RWD 或是 AWD 時，可以分辨現在是使用哪一種設計版型。判斷規則為：小於 576px 會顯示 xs，大於 576px 則顯示 sm，如果大於 768px 則顯示 md，大於 992 顯示 lg，大於 1200px 顯示 xl。
```
<html data-bp="lg">
```

***

### LocalStorage 與 Cookie
urBrowser 會將所有資訊記錄於 LocalStorage 與 Cookie(預設時效為365天)，後端可透過 Cookie 取得資料，javascript 取得使用者瀏覽器資訊時，可直接透過 LocalStorage 取得，例如：
```
var urb = localStorage.getItem('urbrowser');

urb = JSON.parse(urb);
console.log(urb.getBrowserWithCoreName);
/* console
{
  "origin": "chrome",
  "name": "chrome6",
  "core": "webkit",
  "addon": "",
  "full": "chrome chrome6 webkit"
}
*/
```
注意：不支援 IE 9 以下版本 (IE 9 以下應該也不需要處理 RWD 或 AWD 才是)

LocalStorage 資料
```
{"project":"urBrowser","version":"4.0.1b","author":"Kei Cheng","getNameByAgent":"chrome","getPlatform":"mac","getBrowserWithCoreName":{"origin":"chrome","name":"chrome6","core":"webkit","addon":"","full":"chrome chrome6 webkit"},"getOSName":"mac","getBrowserName":{"name":"chrome","origin":"","classes":"chrome chrome6 webkit"},"getBrowserVersion":{"int":6,"full":"6"},"getDevice":"desktop","getOrientation":"portrait","isInApp":false,"isWebView":false,"getBreakpointRange":"lg","classes":"desktop chrome chrome6 webkit"}
```

Cookie 資料
```
urbrowser={"project":"urBrowser","version":"4.0.1b","author":"Kei Cheng","getNameByAgent":"chrome","getPlatform":"mac","getBrowserWithCoreName":{"origin":"chrome","name":"chrome6","core":"webkit","addon":"","full":"chrome chrome6 webkit"},"getOSName":"mac","getBrowserName":{"name":"chrome","origin":"","classes":"chrome chrome6 webkit"},"getBrowserVersion":{"int":6,"full":"6"},"getDevice":"desktop","getOrientation":"portrait","isInApp":false,"isWebView":false,"getBreakpointRange":"lg","classes":"desktop chrome chrome6 webkit"}}
```

可指定 Cookie 的 domain，設定屬性 ```data-domain=".你的網域"```：
```
<script data-domain=".你的網域" src="你的路徑/kule.urbrowser.min.js"></script>
```

如果要將 Cookie 傳送給後端時，可設定屬性 ```data-reload="true"```，此後使用者第一次登入網站時，將會檢查是否已存在 urbrowser 資料而判斷是否需要 reload 網站：
```
<script data-reload="true" src="path/to/kule.urbrowser.min.js"></script>
```

當後端要使用時，以PHP為例：
```
<?php
    $urbrowser = json_decode($_COOKIE['urbrowser']);
    $layout = $urbrowser->getDevice;

    if ($layout === 'desktop') {
        //dosomething...
    }

    if ($layout === 'mobile') {
        //dosomething...
    }
?>
```

以下為 PHP var_dump 之後出來的例子：
```
object(stdClass)#2 (15) {
  ["project"]=>
  string(9) "urBrowser"
  ["version"]=>
  string(6) "4.0.1b"
  ["author"]=>
  string(9) "Kei Cheng"
  ["getNameByAgent"]=>
  string(6) "chrome"
  ["getPlatform"]=>
  string(3) "mac"
  ["getBrowserWithCoreName"]=>
  object(stdClass)#3 (5) {
    ["origin"]=>
    string(6) "chrome"
    ["name"]=>
    string(7) "chrome6"
    ["core"]=>
    string(6) "webkit"
    ["addon"]=>
    string(0) ""
    ["full"]=>
    string(21) "chrome chrome6 webkit"
  }
  ["getOSName"]=>
  string(3) "mac"
  ["getBrowserName"]=>
  object(stdClass)#4 (3) {
    ["name"]=>
    string(6) "chrome"
    ["origin"]=>
    string(0) ""
    ["classes"]=>
    string(21) "chrome chrome6 webkit"
  }
  ["getBrowserVersion"]=>
  object(stdClass)#5 (2) {
    ["int"]=>
    int(6)
    ["full"]=>
    string(1) "6"
  }
  ["getDevice"]=>
  string(7) "desktop"
  ["getOrientation"]=>
  string(8) "portrait"
  ["isInApp"]=>
  bool(false)
  ["isWebView"]=>
  bool(false)
  ["getBreakpointRange"]=>
  string(2) "lg"
  ["classes"]=>
  string(29) "desktop chrome chrome6 webkit"
}
```

***

### 瀏覽器支援
最後，urBrowser 可支援的瀏覽器包含：IE 9+, Edge 12+, Chrome 4+, Firefox 3.5+, Safari 4+, Opera 11.5+。
IE 6 至 IE 8 僅顯示瀏覽器資訊於 <html>上，但不會將資訊儲存於 Cookie 與 Web Storage。IE 8 雖然支援 Web Storage，但是正常來說因為製作 RWD 或 AWD 時應該不太會需要支援到 IE 8，所以就放棄了。

***

* Author: Kei Cheng
* Website: http://www.kule.tw
* Github: https://github.com/keicheng/urbrowser
* Facebook: https://www.facebook.com/kule.tw
* CDN: https://cdnjs.com/libraries/kule.lazy