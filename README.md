# wxsp-components
微信小程序常用组件
1. [选项卡](#tab)
2. [登录](#login)
## tab

选项卡
### wxml
```
<view class='flex align-items-center tab'>
    <view class="{{active == 1?'active':''}}" bindtap='switchTab' data-type='1'>选项一</view>
    <view class="{{active == 2?'active':''}}" bindtap='switchTab' data-type='2'>选项二</view>
    <view class="{{active == 3?'active':''}}" bindtap='switchTab' data-type='3'>选项三</view>
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
        active: 1,
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
### js
```
// pages/login/login.js
Page({
    data: {
        canIUse: wx.canIUse('button.open-type.getUserInfo')
    },
    onLoad: function (options) {
        let url = options.url
        this.setData({
            url
        })
        // 查看是否授权
        wx.getSetting({
            success: function (res) {
                if (res.authSetting['scope.userInfo']) {
                    // 已经授权，可以直接调用 getUserInfo 获取头像昵称
                    wx.getUserInfo({
                        success: function (res) {
                            console.log(res.userInfo)
                            wx.navigateTo({
                                url: url,
                            })
                        }
                    })
                }
            }
        })
    },
    bindGetUserInfo: function (e) {
        console.log(e.detail.userInfo)
        let url = this.data.url
        wx.redirectTo({
            url: url,
        })
    }
})
```
