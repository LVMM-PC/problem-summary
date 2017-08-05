# problem-summary
驴妈妈FED常见问题总结

## 目录

* [HTML相关](#html相关)
* [CSS相关](#css相关)
* [JS相关](#js相关)
* [TOOL相关](#tool相关)
* [FRAMFORK相关](#framwork相关)

### HTML相关
* `<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>` 以上代码IE=edge告诉IE使用最新的引擎渲染网页，chrome=1则可以激活Chrome Frame；
* `<meta name="renderer" content="webkit"/>` 使双引擎浏览器优先使用webkit，如360浏览器；
* IE9及以下不支持Input的placeholder属性
```js
//IE placeholder hack
    if(!placeholderSupport()){   // 判断浏览器是否支持 placeholder
        $('[placeholder]').focus(function() {
            var input = $(this);
            if (input.val() == input.attr('placeholder')) {
                input.val('');
                input.removeClass('placeholder');
            }
        }).blur(function() {
            var input = $(this);
            console.log(0)
            if (input.val() == '' || input.val() == input.attr('placeholder')) {
                input.addClass('placeholder');
                input.val(input.attr('placeholder'));
            }
        }).blur();
    };
    function placeholderSupport() {
        return 'placeholder' in document.createElement('input');
    }
```
### CSS相关
* 不定高盒子内多个内容的100%高度自适应方法：  
```html
<div>
    <div class="div-left">里面有100行的字</div>
    <div class="div-right">里面有1行的字</div>
</div>
```
```css
.div-left,.div-right{
    padding-bottom:10000px;
    margin-bottomL:-10000px
}
```

### JS相关
* 无痕浏览模式下，localStorage、sessionStorage是禁用的，可使用cookie；
* console.log异步：chrome或safri等WebKit的console.log并没有立即拍摄对象快照，相反，它只存储了一个指向对象的引用，然后在代码返回事件队列时才去拍摄快照；**Node的console.log是另一回事，它是严格同步的，因此同样的代码输出的却为{}。**
    ```js
    var obj = {};
    console.log(obj);
    obj.aa = 'hello';
    ```
* 可以使用自定义事件来解决同一页面引入的两个不同js文件的作用域问题。
```js
$(function(){
    // 搜索条固定
        function searchBar() {
            var searchBar = $('.main_search'),
                searchSortBar = searchBar.next(),
                searchBarTop = searchBar.offset().top;
            $(window).on('searchBarChange',function(e){ //监听事件
                searchBarTop = searchBar.offset().top;
            })
            $(window).scroll(function () {
                scrollTop(searchBar,searchBarTop,searchSortBar)
            });
            scrollTop(searchBar,searchBarTop,searchSortBar);
        };
})
$(function(){
    //就这么玩切换
        $('.play-btn').click(function () {
            var $this = $(this);
            if($this.hasClass('play-btn-overview')){
                $('.play-main-overview').show();
                $('.play-main-details').hide();
            }else {
                $('.play-main-details').show();
                $('.play-main-overview').hide();
                //宝典图文滚动
                bottomBarFix();
            }
            //重新计算searchBar
            $(window).trigger('searchBarChange'); //触发事件
            $this.addClass('active').siblings().removeClass('active');
        });
})
```

* 在监听scroll或者resize事件时，如果handler开销比较大时；  

    > 1.需要将handler中不必要的操作提取出来，提前计算  
2.需要引入节流函数来优化性能   
    
```js
/*导航跟随与点击*/
!function navFollow() {
    var container = $('.m-wrap'),
        nav = container.find('.nav-bar'),
        units = container.find('.floor'),
        btns = container.find('.nav-bar .btn'),
        flag = true;

    //获取每个unit的offsettop
    var arr = []; //handler瘦身
    units.each(function () {
        arr.push($(this).offset().top)
    });
    //滚动监听
    $(window).on('scroll',function () {
        if(!flag) return; //防止点击事件干扰
        throttle(scrollHandler,50,100)() //节流函数优化
    });

    function scrollHandler() {
        var sTop = $(window).scrollTop(),
            num = 0;
        for(var i = arr.length-1;i > 0;i--){
            if(arr[i]-sTop < 0){
                num = i;
                break
            }
        }
        btns.eq(num).addClass('current').siblings().removeClass('current');
    }
    
    //点击导航栏
    btns.on('click',function () {
        flag = false;
        var index = $(this).index();
        if(index>=units.length){
            index = 0
        }
        btns.eq(index).addClass('current').siblings().removeClass('current');
        $('html,body').animate({
            scrollTop: units.eq(index).offset().top + 1
        },500,function () {
            flag = true;
        })
    });
    
    //一个节流函数的简单实现
    function throttle(func, wait, mustRun) {
        var timeout,
            startTime = new Date();

        return function() {
            var context = this,
                args = arguments,
                curTime = new Date();
            clearTimeout(timeout);
            // 如果达到了规定的触发时间间隔，触发 handler
            if(curTime - startTime >= mustRun){
                func.apply(context,args);
                startTime = curTime;
                // 没达到触发间隔，重新设定定时器
            }else{
                timeout = setTimeout(func, wait);
            }
        };
    };
}();
```

### TOOL相关
#### gulp相关
* 

#### webpack相关
* 

### FRAMWORK相关
#### vue相关
* 
