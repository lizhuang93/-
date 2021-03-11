### 1. BFC
> 个人理解为css作用域

【应用】
  1. 浮动定位和清除浮动时只会应用于同一个BFC内的元素。
  2. 外边距折叠（Margin collapsing）也只会发生在属于同一BFC的块级元素之间。

【创建BFC】（典型例子3为2不为）
  1. position 为 absolute、fixed。
  2. display 为 inline-block。
  3. flex、inline-flex的直接子元素。
  4. float不为none。
  5. overflow 不为 visible 的块元素。
### 2. 优化SEO
1. 每个页面的 title, keywords, description 的描述尽可能的与该页面相符。
2. 冗余代码优化，css，js封装进.css, .js文件。
3. 使用sitemap。
4. js 自动推送(这里针对百度)。
    ```
    <script>
      (function() {
        var bp = document.createElement('script')
        var curProtocol = window.location.protocol.split(':')[0]
        if (curProtocol === 'https') {
          bp.src = 'https://zz.bdstatic.com/linksubmit/push.js'
        } else {
          bp.src = 'http://push.zhanzhang.baidu.com/push.js'
        }
        var s = document.getElementsByTagName('script')[0]
        s.parentNode.insertBefore(bp, s)
      })()
    </script>
    ```
5. 外链使用 nofollow。
    ```
    <a rel="nofollow" href="www.XXX.com">
    ```
6. 为所有图片添加ALT标签。
7. 使用语义化的 html 标签(一个页面应该只有一个 h1 标签)。
### 3. 物理像素、逻辑像素、设备像素比
- 物理像素（设备像素）：设备一出厂就确定了，固定的。
- 逻辑像素（设备独立像素）：device-width = 逻辑像素；css像素。
- 设备像素比：window.devicePixelRatio是物理像素和设备独立像素(device-independent pixels (dips))的比例。 

### 4. 