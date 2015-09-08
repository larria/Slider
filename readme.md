# Slider.js - 滑动轮播控件

### 特性

- 可配置横竖两种方向，是否循环，以及起始元素和位置，可调节动画快慢及自动播放

- 支持触屏交互, 采用 “跟手” 形式

- 公开部分状态读取和控件公有方法，支持onReady等自定义回调

### 资源引用

``` html
<script src="src/Slider-min.js"></script>
```

### 初始化参数

- **wrap** : 初始化外层容器，也可直接传入dom

- itemClass : 子元素共用类名，默认值'slider_item'

- slideBy : 每次滑动跨越的元素个数，默认值1

- speed : 滑动动动画速度，默认值6

- isVertical : 滑动方向（是否为纵向），默认值false

- isLoop : 是否循环滑动，默认值true

- autoPlay : 是否自动播放，默认值false

- autoInterval : 自动播放时间间隔，需先置autoPlay为true；默认值3，即每隔3s自动滑动一次

##### 回调参数

- onReady : 控件渲染完毕，this指向控件本身，下同。

- onAniStart : 每次滚动动画开始。

- onAniEnd : 每次滚动动画结束。

- onIndexChanged : 当前滚动或者（因循环滚动引起的）瞬移导致页码发生变化。

- onEdge : 当滚动到边缘时。

### 公有方法

- slideTo(index) : 滚动到指定序号的元素

- prev() : 滚动到前一个元素；若当前为第一个元素，且不允许循环滚动，该方法不生效

- next() : 滚动到后一个元素；若当前为最后一个元素，且不允许循环滚动，该方法不生效

- autoSlide(isAuto) : 手动开启或关闭自动滚动

- refresh() : 用以支持响应式页面，当容器尺寸发生变化后手动调用

------------

##### 示例1：最简调用

###### [demo1](../demo1.html)

``` javascript
new Slider({
    wrap : 'slider'
});
```

##### 示例2：完整调用并实现常见的轮播图

###### [demo2](../demo2.html)

``` javascript
var slide_01 = new Slider({
    wrap : 'slider',
    itemClass : 'slider_item',
    startOn : 0,
    slideBy : 1,
    speed : 3,
    isVertical : false,
    isLoop : true,
    autoPlay : true,
    autoInterval : 5,
    onReady : function () {
        var that = this,
            total = this.states.total / 2,
            dots = this._id('hd_slider_dot'),
            btnPrev = this._id('hd_slider_prev'),
            btnNext = this._id('hd_slider_next'),
            dotCls = '',
            dotsStr = '';
        // 点阵
        for(var i = 0; i < total; i ++){
            dotCls = i == 0 ? 'dot_list dot_list_cur' : 'dot_list';
            dotsStr += '<span class="' + dotCls + '" onmouseover="slide_01.slideTo(' + i + ')"></span>';
        }
        dots.innerHTML = dotsStr;
        this.doms.dots = dots.getElementsByTagName('span');
        // 前进/后退按钮
        this._addEvent(btnPrev, 'click', function () {
            that.prev();
        });
        this._addEvent(btnNext, 'click', function () {
            that.next();
        });
    },
    onAniStart : function () {},
    onAniEnd : function () {},
    onIndexChanged : function (index) {
        var that = this,
            dots = this.doms.dots,
            total = this.states.total / 2;
        for(var i = 0; i < total; i ++){
            dots[i].className = 'dot_list';
        }
        dots[index % total].className = 'dot_list dot_list_cur';
    },
    onEdge : function (isLeft) {}
});
```