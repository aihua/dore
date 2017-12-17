# Dore

> Dore is a WebView container implemented using React Native. Help you migrate Cordova application to React Native WebView.

**Dore 是一个使用 React Native 实现的 WebView 容器，可以让你在 WebView 调用 React Native 组件。**

设计初衷：用于迁移 Cordova 的 WebView 应用到 React Native 的 WebView。

Ionic v1 + Angular Demo: [examples](https://github.com/phodal/dore/tree/master/examples)

Screenshots:

![Screenshots](./doc/demo.jpg)

Features
---

Support Bridges:

 - Badge (by react-native-icon-badge)
 - Clipboard 
 - DatePicker (iOS by react-native-nogtag-datepicker)
 - DeviceInfo (by react-native-device-info)
 - Geolocation
 - Keyboard
 - NetInfo
 - Orientation
 - State
 - StatusBar
 - Toast (by react-native-toast)
 
Usage
---

```bash
yarn add dore
```

Example
---

1.Import to your React-Native WebView

```javascript
...
import Toast from 'dore-toast';
import Orientation from 'react-native-orientation';
import RNIconBadge from 'dore-icon-badge'
import RNDeviceInfo from "react-native-device-info";

import Dore from 'dore';

export default class ExampleWebView extends Component {
  ...
  
  constructor() {
    super()
    ...
    Dore.inject([{
      name: 'Toast',
      class: Toast
    }, {
      name: 'Orientation',
      class: Orientation
    }, {
      name: 'Badge',
      class: RNIconBadge
    }, {
      name: 'DeviceInfo',
      class: RNDeviceInfo
    }])
  }

  onMessage = evt => {
    Dore.handleMessage(evt, this.webView)
  };

  onWebViewLoadStart = () => {
    if (this.state.isLoading) {
      this.webView.injectJavaScript('window.isPhone = true;');
    }
  };

  render() {
    ...

    return (
      <View>
        <WebView
          ...
          onMessage={this.onMessage}
        />
      </View>
    )
  }
}
```

2.import [DoreClient](./client/DoreClient.js), e.x:

```html
<script src="js/promise.js"></script>
<script src="js/DoreClient.js"></script>
```

3.use in JavaScript

```javascript
$scope.showToast = function () {
  DoreClient.showToast('this is a toast')
};
```

All Examples
---

### App State Example

```
DoreClient.addStateListener();
$ionicPlatform.on('STATE', function (event) {
  $scope.state = event.detail.data;
  $scope.$apply();
});
```

```javascript
angular.module('starter.controllers', [])

  .controller('DashCtrl', function ($scope, DoreClient, $ionicPlatform) {
    $scope.version = null;
    $scope.text = 'Text Copy';
    $scope.copyText = '';

    DoreClient.addStateListener();
    $ionicPlatform.on('STATE', function (event) {
      $scope.state = event.detail.data;
      $scope.$apply();
    });

    DoreClient.getAppVersion().then(function (data) {
      $scope.version = data.version;
    });
    $scope.showToast = function () {
      DoreClient.showToast('this is a toast')
    };
    $scope.showToastCenter = function () {
      DoreClient.showToast('this is a toast', 'long', 'center')
    };

    $scope.open = function () {
      DoreClient.open('https://www.phodal.com/')
    };

    $scope.copy = function () {
      DoreClient.copy($scope.text);
    };
    $scope.paste = function () {
      $ionicPlatform.on('PASTE', function (event) {
        $scope.copyText = event.detail.data;
        $scope.$apply();
      });
      DoreClient.paste();
    };

    $scope.getBadge = function () {
      DoreClient.getBadge()
    };
    $scope.setBadge = function () {
      DoreClient.setBadge(19)
    };
    $scope.addBadge = function () {
      DoreClient.addBadge()
    };
    $scope.minusBadge = function () {
      DoreClient.minusBadge()
    };
    $scope.showDatePicker = function () {
      var options = {
        date: '2017-10-22 12:12:12',
        maxDate: '2022-10-22 12:12:12'
      };
      DoreClient.showDatePicker(options).then(function (data) {
        $scope.date = data.date;
      })
    };

    $scope.getCurrentPosition = function () {
      DoreClient.getCurrentPosition().then(function (data) {
        $scope.location = data;
      })
    };
    $scope.watchPosition = function () {
      $ionicPlatform.on('WATCH_POSITION', function (event) {
        $scope.wPosition = event.detail.data;
        $scope.$apply();
      });
      DoreClient.watchPosition()
    };
    $scope.clearWatch = function () {
      DoreClient.clearWatch()
    };
    $scope.stopObserving = function () {
      DoreClient.stopObserving()
    };
    $scope.getOrientation = function () {
      DoreClient.getOrientation().then(function (data) {
        console.log(data);
        $scope.orientation = data;
      })
    };
    $scope.lockToLandscape = function () {
      DoreClient.lockToLandscape();
    };
    $scope.lockToPortrait = function () {
      DoreClient.lockToPortrait();
    };
    $scope.getConnectionInfo = function () {
      DoreClient.getConnectionInfo().then(function (data) {
        $scope.connectionInfo = data;
      })
    };
    $scope.addEventListener = function () {
      $ionicPlatform.on('CONNECTION_CHANGE', function (event) {
        $scope.wConnectionInfo = event.detail.data;
        $scope.$apply();
      });
      DoreClient.addNetInfoEventListener();
    };
    $scope.removeEventListener = function () {
      DoreClient.removeNetInfoEventListener();
    };
    $scope.hideStatusBar = function () {
      DoreClient.hideStatusBar();
    };
    $scope.showStatusBar = function () {
      DoreClient.showStatusBar();
    };
  })
```

License
---

[![Phodal's Idea](http://brand.phodal.com/shields/idea-small.svg)](http://ideas.phodal.com/)

© 2017 A [Phodal Huang](https://www.phodal.com)'s [Idea](http://github.com/phodal/ideas).  This code is distributed under the MIT license. See `LICENSE` in this directory.
