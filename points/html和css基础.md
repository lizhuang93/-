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