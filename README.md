## 实现思路
1. 实际页面的宽度 => html font-size = 实际页面的宽度
2. 1rem = 实际页面的宽度
3. 然后将页面中的尺寸全部改为rem，例如width = 32px，转换为rem = 32 / 320 = 0.1rem



## 注意事项
1. 不能将html font-size设置的小于12px，因为在chrome中，字体最小为12px，如果小于12px，则会使用12px覆盖


## 1像素问题
对于普通屏幕： css 1px === 设备的1px
对于Retina屏幕： css 1px === 设备的2px
		 border-width === 设备的1px
                 border-width：0.5px === 设备1px（兼容性问题）

页面整体缩放50% `<meta name="viewport" content="initial-scale=0.5, maximum-sclae=0.5, minimum-scale=0.5">`
border-width: 1px === 设备的1px

副作用：
所有div都变成原来的50%, border不是用rem，用的是px

所有长度都以rem为基准

rem变为原来的2倍

**1px解决方法**
1. 获取像素比 (1/2/3)
2. initial-scale = 1 / 像素比
3. 计算html font size
4. border-top: 1px soli red;

## 实现流程
1. 先设置meta
  `<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalabler=no">`
2. 获取设备像素比
  `var dpr = window.devicePixelRatio`
3. 设置缩放比
  `var scale= 1 / dpr`
4. 设置rem, 因为页面缩放了，所以在rem这里补回来
  `var rem = document.documentElement.clientWidth * dpr / 10`
5. 设置meta，主要是设置`width = dpr * document.documentElement.clientWidth; `和 scale
    `metaEl.setAttribute('content', 'width=' + dpr * docEle.clientWidth + ', initial-scale = ' + scale + ', maximum-scale =' +
      scale + ', minimum-scale = ' + scale + ', user-scalable = no');`
6. 设置html font-size = rem
7. 设置px转rem，这个页面使用的是320的基准页面宽度，因此rem = 32(假如使用的设计稿是以iphone为基准，尺寸为750*1334，那么rem = 75)
  下面使用的是sass定义的函数，将px转换为rem
   ```css
   @function rem($px) {
     $remSize: $px / 32;
     @return #{$remSize}rem;
   }
   ```
      

## 兼容性问题
`vw`可以完全代替`rem方案`，但是`vw`在安卓4.3以下不兼容，而安卓4.3占有的市场份额很大


## 参考
- [移动端高清、多端适配方案](https://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html)
