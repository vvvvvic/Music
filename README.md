
### swiper

###### mode="widthFix"   等比缩放

```js
  <image src="{{item.url}}" mode="widthFix"></image>
```



### 设计组件

###### 高内聚

###### 低耦合
  组件相对独立，功能性相对完整。

###### 单一职责

###### 避免过多参数



### 设置文本显示两行
```css
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
  text-overflow: ellipsis;
```



### 数据监听器

###### observers




### wx:for   修改默认item

```js
<block wx:for="{{swiperImgUrls}}" wx:for-item="data" wx:for-index="idx" wx:key="*this">
  {{idx}} : {{data}}
</block>
```



### 在小程序端，如何使用 async/await 语法？

在云函数里，由于 Node 版本最低是 8.9，因此是天然支持 async/await 语法的。而在小程序端则不然。在微信开发者工具里，以及 Android 端手机（浏览器内核是 QQ浏览器的 X5），async/await是天然支持的，但 iOS 端手机在较低版本则不支持，因此需要引入额外的 文件。可把这个 regenerator/runtime.js 文件引用到有使用 async/await 的文件当中。

```
import regeneratorRuntime from '../../utils/runtime.js'
```



### Request-Promise

This module is installed via npm:
```
npm install --save request
npm install --save request-promise
```

```
const rp = require('request-promise');
```



### 云数据库
云数据库插入只能单条插入


初始化数据库
```
const db = cloud.database()
```

插入数据库
```
db.collection('musiclist').add()
```

### 突破获取数据条数的限制
```
db.collection('musiclist').get()
```
 在云函数中获取，最多每次只能获取100条数据
 在小程序端获取，最多每次只能获取20条数据




### 定时触发器
⚠️ json文件中千万不要写注释
```
{
  // triggers 字段是触发器数组，目前仅支持一个触发器，即数组只能填写一个，不可添加多个
  "triggers": [
    {
      // name: 触发器的名字，规则见下方说明
      "name": "myTrigger",
      // type: 触发器类型，目前仅支持 timer (即 定时触发器)
      "type": "timer",
      // config: 触发器配置，在定时触发器下，config 格式为 cron 表达式，规则见下方说明
      "config": "0 0 2 1 * * *"
    }
  ]
}
```
### 云函数路由优化 tcb-router
一个用户在一个云环境中只能创建50个云函数
相似的请求归类到同一个云函数中处理
tcb-router是一个koa风格的云函数路由库



### 本地存储
```
wx.setStorageSync(key, value)
```
```
wx.setStorage(key, value)
```



### 引入iconfont图标



### 播放音乐

获取全局位移的背景音频管理器
```
wx.getBackgroundAudioManager()
```



### 组件间传值
需求：歌词与播放歌曲同步
自定义事件  triggerEvent()

progress-bar组件 传值currentTime  到 lyric组件

progress-bar组件
```
this.triggerEvent('timeUpdate', {
    currentTime
  })
```

player组件（该组件调用progress-bar）
player.wxml
```
<!-- 歌词 -->
<m-lyric class="lyric"/>

<!-- 进度条 -->
<view class="progress-bar">
  <m-progress-bar bind:timeUpdate="timeUpdate"/>
</view>
```

player.js
根据类名获取组件
```
timeUpdate(event) {
    this.selectComponent('.lyric').updata(event.detail.currentTime)
  }
```
lyric组件
```
updata(currentTime) {

    }
```




### 组件的生命周期
```
lifetimes: {
  ready() {

  }
}
```



### 歌词随着播放移动
lyric.js
```
lifetimes: {
    ready() {
      //获取当前设备信息
      wx.getSystemInfo({
        success(res) {
          //res.screenWidth / 750  求出1rpx大小
          //res.screenWidth / 750 * 64  一行歌词所对应的高度（css里设置了一行歌词64rpx）
          lyricHeight = res.screenWidth / 750 * 64
        }
      })
    }
  },

  //scrollTop scroll-view组件滚动高度
  methods: {
    //接收进度条组件传过来的时间
    updata(currentTime) {
      let lrcList = this.data.lrcList
      if (lrcList.length == 0) {
        return
      }
      for(let i=0; i<lrcList.length; i++) {
        if (currentTime <= lrcList[i].time) {
          this.setData({
            nowLyricIndex: i-1,
            scrollTop: (i - 1) * lyricHeight
          })
          break
        }
      }
    },
 }
```



### 小程序设置全局属性
app.js
```
App({
  onLaunch: function () {
    //全局变量
    this.globalData = {
      //当前播放的歌曲id
      playingMusicId: -1
    }
  },

  //设置playingMusicId
  setPlayMusicId(musicId) {
    this.globalData.playingMusicId = musicId
  },

  //获取playingMusicId
  getPlayMusicId(musicId) {
    return this.globalData.playingMusicId
  }
```

player.js
```
//调用全局属性和方法
const app = getApp()

//设置当前播放歌曲id为全局
app.setPlayMusicId(songId)
```



### 组件样式隔离
在search组件中，无法调用全局的icon图标
解决，将图标文件放在组件内部


### slot插槽  具名插槽



### 用户授权
小程序为button提供的开放功能
```
<button open-type="getUserInfo" bindgetuserinfo="ongetUserInfo">获取微信授权信息</button>
```
```
ongetUserInfo(event) {
  const userInfo = event.detail.userInfo
  if (userInfo) {
    //允许授权

  }else {

  }
}
```



### 原生组件
textarea  video  map  ~~~
在小程序中
  - 原生组件层级最高，无论把其他元素的z-index设置多高，都覆盖不了原生组件
  - 原生组件，不能使用在<swiper>等容器中

原生组件绑定事件   不能使用 ：      bindinput="onInput"
```
<textarea bindinput="onInput" placeholder="分享新鲜事..."></textarea>
```



### 小程序中绑定事件的方式
bind:tap   存在事件冒泡
catch:tap  不存在事件冒泡



### setData 回调 第二个参数在第一个完成后执行

```
      this.setData({
        loginShow: false
      }, ()=>{
        this.setData({
          loginShow: true
        })
      })
```




### 云调用实现模版消息推送
- 必须使用form表单    report-submit="true"

```
<form slot="modal-content" report-submit="true" bind:submit="onSend">
  <textarea name="content" bindinput="onInput" class="comment-content" placeholder="写评论" value="{{content}}" fixed="true"></textarea>
  <button class="send" form-type="submit">发送</button>
</form>
```

- 新建云函数

- 配置 config.json
```
{
  "permissions": {
    "openapi": [
      "templateMessage.send"
    ]
  }
}
```


### 分享
必须用 button   open-type="share"
```
<button open-type="share" data-blogid="{{blogId}}" data-blog="{{blog}}" class="share-btn" hover-class="share-hover">
    <i class="iconfont icon-icon-test1 icon"></i>
    <text>分享</text>
</button>
```


### 设置背景图片
1. 不能用项目中的图片，使用相对路径设置
   可以使用网络图片
2. 可以使用base64格式图片



### 小程序的本地存储
- 最近播放 功能  使用本地存储
- ../../pages/player/player?musicid={dataset.musicid}&index={dataset.index}



### 小程序码
配置权限
config.json
```
{
  "permissions": {
    "openapi": [
      "wxacode.getUnlimited"
    ]
  }
}
```

小程序上线后，如何得到用户是扫描谁分享得二维码进入页面
- 在对应页面加载函数中  options.scene



### 小程序渲染与逻辑层交互原理
- 小程序无DOM操作
- 不能频繁setData
- 避免使用 :active 伪类来实现点击态
- 滚动区域可开启惯性滚动以增强体验
  - 安卓自带惯性滚动
  - ios
    ```
    -webkit-overflow-scrolling: touch
    ```

 ![image](./img/1.png)



### 小程序更新机制
app.js中检查更新
```
checkUpdate() {
    const updateManager = wx.getUpdateManager()
    //检测版本更新
    updateManager.onCheckForUpdate((res)=>{
      if(res.hasUpdate) {
        updateManager.onUpdateReady(()=>{
          console.log(123456)
          wx.showModal({
            title: '更新提示',
            content: '新版本已经准备好，是否重启应用',
            success(res) {
              if (res.confirm) {
                updateManager.applyUpdate()
              }
            }
          })
        })
      }
    })
  }
```



### Audits
- 手动：小程序体验评分
- 自动： 详情-本地设置-自动运行体验评分



### setData
- setData是一个同步过程，其回调是一个异步过程
  - testData 初始值设置为0
  ```
  console.log('testData start' + this.data.testData)
  this.setData({
    testData: 1
    },()=>{
      console.log('回调执行')
      })
  console.log('testData end' + this.data.testData)
  ```
  打印结果：
  ```
  testData start 0
  testData end 1
  回调执行

  ```
- 避免setData数据过大（不要超过1M）

- 避免setdata的调用过于频繁

- 避免未绑定的wxml的变量传入setData（与页面显示没有关系的变量，不要存在setData）

- setData 一个对象中的单独值
```
data:{
  testObj: {
    name: 'jack',
    age: 33
  }
}
```
```
this.setData({
  ['testObj.age']: 34,
  ['testObj.city']: Beijing
  })
```


### 场景值scene的作用与应用场景



### 页面收录sitemap.json的作用与使用方法


### 小程序上线审核



### access_token的缓存与更新

npm install request

npm install request-promise

### node获取文件的绝对路径
```
const path = require('path')
const fileName = path.resolve(__dirname, '文件名')
```


### koa-router
