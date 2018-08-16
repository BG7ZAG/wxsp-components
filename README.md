# wxsp-components
微信小程序常用组件
1. [选项卡](#tab)
2. [登录](#login)
2. [验证码](#code)
## tab

选项卡
### wxml
```
<view class='flex align-items-center tab'>
    <view wx:for="{{tabs}}" wx:key="tabs" class="{{active == index?'active':''}}" bindtap='switchTab' data-type='{{index}}'>选项一</view>
</view>
```
### css
```
.flex{
    display: flex;
}
.align-items-center{
    align-items: center;
}
.tab{
    background: #fff;
    text-align: center;
    font-size: 28rpx;
    height: 80rpx;
    line-height: 80rpx;
}
.tab view{
    flex-grow: 1;
}
.tab view.active{
    border-bottom: 1px solid #02D6C8;
    color: #02D6C8;
}
```
### js
```
Page({

    /**
     * 页面的初始数据
     */
    data: {
        active: 0,
        tabs: ['选项一','选项二'],
    },

    /**
     * 切换选项卡
     */
    switchTab: function (e) {
        let _type = e.currentTarget.dataset.type
        this.setData({
            active: _type
        })
    },
})
```
## login
授权登录页
### wxml
```
<!--pages/login/login.wxml-->
<!-- 如果只是展示用户头像昵称，可以使用 <open-data /> 组件 -->
<view class='avatar'>
    <open-data type="userAvatarUrl"></open-data>
</view>
<view class='nickname'>
    <open-data type="userNickName"></open-data>
</view>
<!-- 需要使用 button 来授权登录 -->
<button class='btn' wx:if="{{canIUse}}" type='primary' open-type="getUserInfo" bindgetuserinfo="bindGetUserInfo">授权登录</button>
<view class='tip' wx:else>请升级微信版本</view>
```
### wxss
```
/* pages/login/login.wxss */
.avatar{
    width: 200rpx;
    height: 200rpx;
    border-radius: 50%;
    overflow: hidden;
    margin: 100rpx auto 0;
}
.nickname{
    text-align: center;
    margin-top: 30rpx;
}
.tip{
    text-align: center;
    margin-top: 50rpx;
}
.btn{
    margin-top: 50rpx;
    width: 80%;
}
```
### 上一个页面跳转进来之前储存的参数
```
// 比如上一个页面为 /pages/goodDetail/goodDetail
// 需要的参数为 id = options.id type = options.type

let url = `/pages/goodDetail/goodDetail?id=${options.id}&type=${options.type}`
let params = `id=${options.id}&type=${options.type}`

wx.setStorageSync('loginUrl', '/pages/goodDetail/goodDetail')
wx.setStorageSync('loginParams', params)
wx.setStorageSync('loginToType', 'redirectTo')
// 跳到登录页
wx.navigateTo({
    url: '/pages/login/login',
})
```
### js
```
// pages/login/login.js
const app = getApp();
Page({
    data: {
        canIUse: wx.canIUse('button.open-type.getUserInfo')
    },
    onLoad: function (options) {
        let that = this
        let url = wx.getStorageSync('loginUrl')
        let params = wx.getStorageSync('loginParams')
        let loginToType = wx.getStorageSync('loginToType')
        this.setData({
            url,
            params,
            loginToType
        })
        // 查看是否授权
        wx.getSetting({
            success: function (res) {
                if (res.authSetting['scope.userInfo']) {
                    // 已经授权，可以直接调用 getUserInfo 获取头像昵称
                    wx.getUserInfo({
                        success: function (res) {
                            console.log(res.userInfo)

                            // 跳转
                            switch (loginToType) {
                                case 'switchTab':
                                    wx.switchTab({
                                        url: url,
                                    })
                                    break;
                                default:
                                    wx.redirectTo({
                                        url: url + '?' + params,
                                    })
                            }
                        }
                    })
                }
            }
        })
    },
    bindGetUserInfo: function (e) {
        console.log(e.detail.userInfo)
        if (e.detail.userInfo) {
            let that = this
            let url = this.data.url
            let loginToType = this.data.loginToType
            let params = this.data.params
            
            // 跳转
            switch (loginToType) {
                case 'switchTab':
                    wx.switchTab({
                        url: url,
                    })
                    break;
                default:
                    wx.redirectTo({
                        url: url + '?' + params,
                    })
            }
        }else{
            wx.showToast({
                title: '请授权登录',
                icon: 'none',
            })
        }
    }
})
```
## code
验证码
### WXML
```
<view class="{{isGetCode?'color':''}}" bindtap='getCode'>{{isGetCode?'获取验证码': count+ 's后重新获取'}}</view>
```
### JS
```
// pages/login/login.js
Page({

    /**
    * 页面的初始数据
    */
    data: {
        count: 60,
        isGetCode: true,
    },
    /**
        * 获取验证码
        */
    getCode:function(e){
        if(this.data.isGetCode){
            this.setData({
                isGetCode: false
            })
            countTime(this, 60)
        }
    },
})
// 倒计时
function countTime(that, e) {
    if (e == 0) {
        that.setData({
            isGetCode: true
        })
        return
    }
    e--
    that.setData({
        count: e
    })
    setTimeout(()=>{
        countTime(that, e)
    },1000)
}
```