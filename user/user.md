要实现在 H5 页面中通过自定义按钮调起微信的分享功能，你需要使用微信 JS-SDK。微信 JS-SDK 允许网页调用微信的原生分享功能，包括分享给朋友、分享到朋友圈、分享到QQ等。

以下是使用微信 JS-SDK 实现自定义分享按钮的步骤：

1. 引入微信 JS-SDK

在你的 HTML 页面中引入微信 JS-SDK：

Copy
<script src="https://res.wx.qq.com/open/js/jweixin-1.6.0.js"></script>

2. 配置微信 JS-SDK

在使用 JS-SDK 之前，你需要通过后端服务获取必要的配置参数，包括 appId、timestamp、nonceStr 和 signature。这些参数用于调用微信的 wx.config 方法进行配置：

Copy
wx.config({
  debug: false, // 开启调试模式
  appId: '你的公众号的appid', // 必填，公众号的唯一标识
  timestamp: '生成签名的时间戳', // 必填，生成签名的时间戳
  nonceStr: '生成签名的随机串', // 必填，生成签名的随机串
  signature: '生成的签名', // 必填，签名
  jsApiList: ['onMenuShareTimeline', 'onMenuShareAppMessage'] // 必填，需要使用的JS接口列表
});

3. 自定义分享内容

在 wx.ready 方法中，你可以设置分享给朋友和分享到朋友圈的内容：

Copy
wx.ready(function () {
  // 分享给朋友
  wx.onMenuShareAppMessage({
    title: '自定义分享标题', // 分享标题
    desc: '自定义分享描述', // 分享描述
    link: '分享链接', // 分享链接
    imgUrl: '分享图标的URL', // 分享图标
    type: 'link', // 分享类型，music、video 或 link，不填默认为 link
    dataUrl: '', // 如果 type 是 music 或 video，则要提供数据链接，默认为空
    success: function () {
      // 用户点击了分享后执行的回调函数
    }
  });

  // 分享到朋友圈
  wx.onMenuShareTimeline({
    title: '自定义分享标题', // 分享标题
    link: '分享链接', // 分享链接
    imgUrl: '分享图标的URL', // 分享图标
    success: function () {
      // 用户点击了分享后执行的回调函数
    }
  });
});

4. 绑定自定义按钮事件

在你的页面上，添加一个自定义按钮，并为其绑定点击事件。在点击事件的处理函数中，你可以调用 wx.updateAppMessageShareData 和 wx.updateTimelineShareData 方法来触发分享：

Copy
document.getElementById('my-share-button').onclick = function () {
  // 调用分享给朋友的接口
  wx.updateAppMessageShareData({
    title: '自定义分享标题',
    desc: '自定义分享描述',
    link: '分享链接',
    imgUrl: '分享图标的URL',
    success: function () {
      // 设置成功
    }
  });
};

5. 处理错误

你还可以在 wx.error 方法中处理配置失败的情况：

Copy
wx.error(function (error) {
  // 配置失败的处理
  console.log(error);
});

注意事项
微信 JS-SDK 的配置数据（appId、timestamp、nonceStr、signature）需要通过后端服务安全地生成，以避免暴露你的 appsecret。
确保你的网页是通过 HTTPS 访问的，因为微信 JS-SDK 要求页面必须是安全的 HTTPS 页面。
微信分享的链接通常需要在微信公众平台的“JS接口安全域名”中进行登记。
分享的图片链接需要是完整的 URL，不能是本地路径，并且图片需要已经在线上可访问。

通过以上步骤，你可以在 H5 页面中实现自定义分享按钮，调起微信的分享功能。记得在实际部署应用时，确保所有的配置项都是正确的，并且后端服务能够安全可靠地提供所需的签名信息。

----

免费矢量图网站提供了大量的矢量图形资源，这些资源可以用于个人项目或商业用途（根据各自的许可协议）。以下是一些流行的免费矢量图网站：

Freepik - 提供大量的免费矢量图形、图标和PSD资源。商业用途通常需要属性。

网址: https://www.freepik.com/

Vecteezy - 提供高质量的免费矢量图形和矢量艺术。商业用途可能需要属性。

网址: https://www.vecteezy.com/

Flaticon - 是一个庞大的免费图标库，提供各种格式的图标，包括矢量格式。

网址: https://www.flaticon.com/

SVG Repo - 提供免费的SVG图标和矢量图形，可以直接在网站上自定义颜色。

网址: https://www.svgrepo.com/

unDraw - 提供开源的插图集合，可以根据你的设计自定义颜色。

网址: https://undraw.co/

Openclipart - 提供各种免费的矢量剪贴画资源，可以用于商业和非商业项目。

网址: https://openclipart.org/

Vector4Free - 一个免费矢量图形资源网站，提供各种风格的矢量图。

网址: https://www.vector4free.com/

SVG Silh - 提供免费的SVG图像资源，所有图像都在公共领域或CC0许可下。

网址: https://svgsilh.com/

Vector.me - 提供数以万计的免费矢量图形、徽标和图标。

网址: https://vector.me/

Vexels - 提供各种矢量图形和设计资源，部分资源是免费的。

网址: https://www.vexels.com/

在使用这些资源时，请务必检查每个图形的许可协议，以确保你遵守了正确的使用条款，特别是如果你打算将这些图形用于商业项目。有些网站可能要求你在使用资源时提供属性或者遵循特定的许可协议。

