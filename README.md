# wxsp-components
微信小程序常用组件

## tab选项卡
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