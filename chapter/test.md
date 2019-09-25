# Admob

AdMob brings together best-in-class technology in a single platform, so you can gain insights about your users, drive more in-app purchases, and maximize your ad revenue. No longer will you have to rely on a combination of tools or use precious development resources to build your own solution.

## Integration SDKBox

1. Sử dụng Creator mở project. Mở hộp thoại Build từ Menu bar -> Project. Chọn iOS/Android Platform và tiến hành Build.
2. Sau khi Build xong chọn Extension -> SDKbox -> Lauch và chọn Admob để cài đặt.

## Configuration AdMob

1. Đường dẫn file `./build/jsb-link/res/sdkbox_config.json`
2. Điền **AppID** và các **ID** quảng cáo.
3. `"testdevice"`: config các **TestID** tránh admod ban. Nếu chưa có TestID thì dùng cách sau để lấy.

### Android

* Tải ứng dụng có tích hợp quảng cáo và đưa ra một yêu cầu quảng cáo.
* Kiểm tra  logcat tìm thông báo trông giống như sau:

```javascript
I/Ads: Use AdRequest.Builder.addTestDevice("33BE2250B43518CCDA7DE426D04EE231")
to get test ads on this device."
```

### iOS

Updating ...

```json
{
    "AdMob": {
        "testdevice": "kGADSimulatorID,D8E988B99C17799A184F549CA6017EB8,33BE2250B43518CCDA7DE426D04EE231",
        "test": true,
        "appid": "ca-app-pub-7744786175205006~4620046991",
        "ads": {
            "rewarded": {
                "type": "rewarded_video",
                "id": "ca-app-pub-7744786175205006/7386397867"
            },
            "top_banner": {
                "type": "banner",
                "id": "ca-app-pub-7744786175205006/8699479532",
                "alignment": "top"
            },
            "test_rewarded": {
                "type": "rewarded_video",
                "id": "ca-app-pub-3940256099942544/5224354917"
            },
            "test_banner": {
                "type": "banner",
                "id": "ca-app-pub-3940256099942544/6300978111"
            }
        }
    }
}
```

## Create JavaScript Component

```typescript
const { ccclass, property } = cc._decorator;

@ccclass
export default class Admob extends cc.Component {
    onLoad() {
        this.init()
    }

    init() {
        if (cc.sys.isNative) {

            // Gọi init admob
            sdkbox.PluginAdMob.init()

            // Set các callback event
            sdkbox.PluginAdMob.setListener({
                adViewDidReceiveAd: function (name) {
                    cc.log('IAP adViewDidReceiveAd', name)
                },
                adViewDidFailToReceiveAdWithError: function (name, msg) {
                    cc.log('IAP adViewDidFailToReceiveAdWithError', name, msg)
                },
                adViewWillPresentScreen: function (name) {
                    cc.log('IAP adViewWillPresentScreen', name)
                },
                adViewDidDismissScreen: function (name) {
                    cc.log('IAP adViewDidDismissScreen', name)
                },
                adViewWillDismissScreen: function (name) {
                    cc.log('IAP adViewWillDismissScreen', name)
                },
                adViewWillLeaveApplication: function (name) {
                    cc.log('IAP adViewWillLeaveApplication', name)
                },
                reward(name, currency, amount) {
                    cc.log('IAP reward', name, currency, amount)

                }
            })

            // cache các quảng cáo cần thiết
            // tên các gói quảng cáo như rewarded, top_banner, ... xem config bên dưới
            sdkbox.PluginAdMob.cache('rewarded')
            sdkbox.PluginAdMob.cache('top_banner')

            // 2 quảng cáo test của admob
            sdkbox.PluginAdMob.cache('test_banner')
            sdkbox.PluginAdMob.cache('test_rewarded')
        }
    }

    // các method dể button gọi tới
    watchVideo() {
        if (!sdkbox.PluginAdMob.isAvailable('rewarded')) {
            cc.log('Ads is not availble')
            sdkbox.PluginAdMob.cache('rewarded')
            return
        }
        sdkbox.PluginAdMob.show('rewarded')
    }

    showBanner() {
        if (!sdkbox.PluginAdMob.isAvailable('top_banner')) {
            cc.log('Ads is not availble')
            sdkbox.PluginAdMob.cache('top_banner')
            return
        }
        sdkbox.PluginAdMob.show('top_banner')
    }

    testVideo() {
        if (!sdkbox.PluginAdMob.isAvailable('test_rewarded')) {
            cc.log('Ads is not availble')
            sdkbox.PluginAdMob.cache('test_rewarded')
            return
        }
        sdkbox.PluginAdMob.show('test_rewarded')
    }

    testBanner() {
        if (!sdkbox.PluginAdMob.isAvailable('test_banner')) {
            cc.log('Ads is not availble')
            sdkbox.PluginAdMob.cache('test_banner')
            return
        }
        sdkbox.PluginAdMob.show('test_banner')
    }
}

```
