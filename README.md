# 懒加载效果
## 预览地址
https://evenyao.github.io/lazyload-demo/

## 图解

通过下图所示（绿色为页面范围，红色为窗口范围，蓝色为待显示元素）。 `$(window).scrollTop()` 为 页面顶部 到 窗口顶部的高度。 ` $(window).height()` 为 窗口的高度。`$node.offset().top` 为页面顶部到待显示元素的高度。即我们可以知道如何判断一个元素，是否进入用户视野范围内，即

```JavaScript
$node.offset().top <= $(window).height() + $(window).scrollTop()
```

条件满足的时候，这个元素就进入了我们的视野。

![](https://upload-images.jianshu.io/upload_images/12904618-3499ce9caae44911.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 懒加载的实现
```JavaScript
  //进页面直接调用
  start()
  // 滚动的时候执行加载函数
  $(window).on('scroll',function(){
    start()
  })

  //加载函数
  function start(){
    //not('[data-isLoaded]') 用来过滤已经加载过的，实现节流
    $('.container img').not('[data-isLoaded]').each(function(){
      if( isShow($(this)) ){
        loadImg($(this))
      }
    })
  }

  // 判断是否进入视野的函数
  function isShow($node){
    return $node.offset().top <= $(window).height() + $(window).scrollTop()
  }

  // 格式化图片加载地址函数
  function loadImg($img){
    //setTimeout模拟延迟 测试效果用，实际环境不要加
    // setTimeout(function(){
      $img.attr('src', $img.attr('data-src'))
    // },500)
     //加载过后就添加 data-isLoaded属性
      $img.attr('data-isLoaded',1)
  }
```

## 参考
[滚动加载图片（懒加载）实现原理](https://www.cnblogs.com/flyromance/p/5042187.html)
