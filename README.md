# Animate-analyze
# animate.css是什么？能做些什么？
> animate.css是一个css动画库，使用它可以很方便的快捷的实现，我们想要的动画效果，而省去了操作js的麻烦。同时呢，它也是一个开源的库，在GitHub的点赞高达5万以上。功能非常强大。性能也足够出色。
# 如何使用它？
1. 首先你需要到github上下载它，[地址](https://github.com/daneden/animate.css)
2. 拿到它之后，把animate.css引入到你的html文件。
3. 参考官方的文档（当然了，是英文的哈哈哈，程序员不会英语可万万不行的哦。）就可以十分方便的使用它了。
4. 注意哈，内联元素比如a标签有些动画是不支持的。目前该库正在一点一点的更新中。
5. 例子
````html
（一）静态的使用它
<!-- animated是必须添加的；bounce是动画效果；infinite从语义来看也秒懂，无限循环，不添加infinite默认播放一次 -->
使用的基本公式就是：

 <div class="animated 想要的动画名称 循环的次数 延迟的时间 持续的时间">动画</div>


 <div class="animated bounce infinite delay-2s duration-2s ">动画</div>
 ````
 ````js
 （二）动态的使用它
//  主要的思路就是：给动画对象添加类，然后监听动画结束事件，一旦监听到动画结束，立即移除前面添加的类。----如果你现在还不会使用js基本语法以及jQuery，那么确实比较难理解
// 以下是官方给出的jQuery封装
//扩展$对象，添加方法
        animateCss $.fn.extend({
        animateCss: function (animationName) { var animationEnd = 'webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend';
            $(this).addClass('animated ' + animationName).one(animationEnd, function() { $(this).removeClass('animated ' + animationName);
            });
    }

// 以下是一个小demo
    //animate("选择器","动画","次数","时延")
    function animate(seletor, animation_name, count, delay) { 
        
        var target = document.querySelectorAll(seletor) 
        var timer = null;

        timer = setInterval( function() { target.forEach( function(x) { x.className += " animated " + animation_name;
        x.addEventListener("animationend", function(){ 
        x.className = x.className.replace(" animated " + animation_name, "");});
        } )
        count --; 
        if( count <= 0 ) {
                    clearInterval( timer );
                }
            }, delay)
        } //使用示例 animate('.haha', "bounce", 2, 1000);
    // 通过这个例子你就能明白如何动态的使用它了，这个小demo只实现了对animationend的监听。
````
- 如果你想更简单的使用js，请看以下代码.注意以下操作均用到了jQuery
````js
//  1.如果说想给某个元素动态添加动画样式，可以通过jquery来实现：
 $('#yourElement').addClass('animated bounceOutLeft');
//  2.当动画效果执行完成后还可以通过以下代码添加事件
 $('#yourElement').one('webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend', doSomething);
//  3.你也可以通过 JavaScript 或 jQuery 给元素添加这些 class，比如：
 $(function(){
    $('#yourElement').addClass('animated bounce');
 });
//  4.有些动画效果最后会让元素不可见，比如淡出、向左滑动等等，可能你又需要将 class 删除，比如：
 $(function(){
    $('#yourElement').addClass('animated bounce');
    setTimeout(function(){
        $('#yourElement').removeClass('bounce');
    }, 1000);
});
 ````


- 以上的js代码不会？没关系我们还有别的办法，实现同样的效果
````css
/* 我们可以通过自己重写源代码里面的属性，从而覆盖源代码，让动画实现我们想要的效果，如果这个你还不会？ok那你还是好好把前面的html啊，css啊移动端的布局啊，好好的温习温习*/

#yourElement { 
        -vendor-animation-duration: 3s; //设置-vendor-animation-delay: 2s; //设置延迟的时间-vendor-animation-iteration-count: infinite; //
        }

/* 关于时间的说明，animated中定义了几个类。在官方的文档中也给出了，默认的是delay-1s,duration也是1s
slow	2s
slower	3s
fast	800ms
faster	500ms */
````

- 还需要说明的就是在你的项目中使用它，需要注意的地方，如果你还未掌握服务器端的技术，这点你可以忽略
```json
animate提供了npm以及yarn的下载安装方式，你可以使用npm 或者yarn来下载并且使用它。如何你可以通过设置配置文件(animate-config.json)来选取你需要的功能模块。譬如，我不需要flash和shake效果，只要在配置文件中，设置为false既可。
"attention_seekers": {
  "bounce": true,
  "flash": false,
  "pulse": false,
  "shake": true,
  "headShake": true,
  "swing": true,
  "tada": true,
  "wobble": true,
  "jello":true
}

```
# 解析源码
> 这里最主要的是就是三个关键类，
1. animated类
- 以下是animated类的部分源码
````css
+++
.animated.flip {
  -webkit-backface-visibility: visible;
  /* 指定元素背面面向用户时是否可见。 */
  backface-visibility: visible;
  -webkit-animation-name: flip;
  /* 检索或设置对象所应用的动画名称，必须与规则@keyframes配合使用，因为动画名称由@keyframes定义 */
  animation-name: flip;
}
+++
.animated {
  /* 兼容性写法，duration设置的是持续的时间 */
  -webkit-animation-duration: 1s;
  animation-duration: 1s;
  /* 检索或设置对象动画时间之外的状态 both选项就是设置对象状态为动画结束或开始的状态 */
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
}

.animated.infinite {
  -webkit-animation-iteration-count: infinite;
  /* 指定动画循环可以是无限的(infinite也可以指定具体的循环次数比如9) */
  animation-iteration-count: infinite;
}

.animated.delay-1s {
  -webkit-animation-delay: 1s;
  /* 设置延迟的时间 */
  animation-delay: 1s;
}

.animated.delay-2s {
  -webkit-animation-delay: 2s;
  animation-delay: 2s;
}

.animated.delay-3s {
  -webkit-animation-delay: 3s;
  animation-delay: 3s;
}

.animated.delay-4s {
  -webkit-animation-delay: 4s;
  animation-delay: 4s;
}

.animated.delay-5s {
  -webkit-animation-delay: 5s;
  animation-delay: 5s;
}
/* 以下的四个是我们之前说的animate中自定义的时间词汇 */
.animated.fast {
  -webkit-animation-duration: 800ms;
  animation-duration: 800ms;
}

.animated.faster {
  -webkit-animation-duration: 500ms;
  animation-duration: 500ms;
}

.animated.slow {
  -webkit-animation-duration: 2s;
  animation-duration: 2s;
}

.animated.slower {
  -webkit-animation-duration: 3s;
  animation-duration: 3s;
}

/* 这个是媒体查询相关的 */
/* prefers-reduced-motion 用于检测用户的系统是否被开启了动画减弱功能。 
reduce
这个值意味着用户修改了系统设置，将动画效果最小化，最好所有的不必要的移动都能被移除。
*/
@media (print), (prefers-reduced-motion: reduce) {
  .animated {
    -webkit-animation-duration: 1ms !important;
    animation-duration: 1ms !important;
    -webkit-transition-duration: 1ms !important;
    transition-duration: 1ms !important;
    -webkit-animation-iteration-count: 1 !important;
    animation-iteration-count: 1 !important;
  }
}

````
2. infinite类
- 这个的源码就在上面了
````css
.animated.infinite {
  -webkit-animation-iteration-count: infinite;
  /* 指定动画循环可以是无限的(infinite也可以指定具体的循环次数比如9) */
  animation-iteration-count: infinite;
}
````

3. 动画名类，比如bounce
- 以下是定义具体的动画代码，

>  注意,高大上的bezier曲线。这个timing-function是一个检索或设置对象动画的过渡类型的东西，cubic-bezier是贝塞尔曲线，取值在[0,1]之间，需要指定四个值，那么什么是贝塞尔曲线呢？哈哈哈如果你高数没白学，那你应该能明白这个的工作原理，如果你还不是很清楚，请点击这里
>    [bezier曲线？计算机图形原理](https://blog.csdn.net/syx1065001748/article/details/70313523?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)
>    [体会以下bezier曲线](https://cubic-bezier.com/#.82,.84,.4,.6)
````css
@keyframes bounce {
  from,
  20%,
  53%,
  80%,
  to {
    animation-timing-function: cubic-bezier(0.215, 0.61, 0.355, 1);
    
    transform: translate3d(0, 0, 0);
  }

  40%,
  43% {
    animation-timing-function: cubic-bezier(0.755, 0.05, 0.855, 0.06);
    transform: translate3d(0, -30px, 0);
  }

  70% {
    animation-timing-function: cubic-bezier(0.755, 0.05, 0.855, 0.06);
    transform: translate3d(0, -15px, 0);
  }

  90% {
    transform: translate3d(0, -4px, 0);
  }
}

.bounce {
  animation-name: bounce;
  transform-origin: center bottom;
}
````
- 你可以看得到，这些代码，在项目文件夹结构中，animate把source(意为“源”)里面的具体实现代码合并到一个css文件中（animate.css)，在source文件夹下，这些具体的动画效果被做了具体的分类与归档。



# 后续给大家带来一个更加更加实用的css库使用以及源码剖析，敬请期待
