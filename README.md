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
                console.log(1)
            }
            console.log(2)
        }).blur();
    };
    function placeholderSupport() {
        return 'placeholder' in document.createElement('input');
    }
```
### CSS相关
* 不定高盒子内不定高内容的垂直居中方法：
  1.flex布局
  2.padding-bottom+margin-bottom组合使用

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

### TOOL相关
#### gulp相关
* 

#### webpack相关
* 

### FRAMWORK相关
#### vue相关
* 
