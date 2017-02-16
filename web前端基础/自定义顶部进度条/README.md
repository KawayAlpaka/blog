# 自定义顶部进度条
### 基础
html入门 css入门
### 关键知识点:
css3 transition 过渡
### 需求
在提交数据后等待服务器响应的过程中，如果时间太长，并且没有一个提示，用户就会疑惑`"系统现在到底在干什么，为什么没有响应我的操作"`。

为了解决这个问题，优化用户体验，我们决定设计一个__假的__进度条来告诉用户，系统正在与服务器交换数据。
### 开始开发
#### 1、创建一个html元素
在body中加入进度条的html元素

    <div class="l-progress-bar-01"></div>
#### 2、添加元素基本样式
    .l-progress-bar-01{
        position: fixed;
        top: 0;
        left: 0;
        background-color: #5eac82;
        height: 4px;
        z-index: 9999;
    }
这样就得到一个悬浮在页面顶部的元素
#### 3、进度条各状态的样式
进度条开始运行瞬间的，宽度为0

    .l-progress-bar-01.l-start{
        width: 0%;
    }
等待时间足够长，但还没有请求完成时，宽度为99%

    .l-progress-bar-01.l-start.l-full{
        width: 99%;
    }
请求完成时，宽度为100%
    .l-progress-bar-01.l-run.l-end{
        width: 100%;
    }
#### 4、添加控制按钮（用于测试）
    <button onclick="lStartSubmitProgress()">start</button>
    <button onclick="lEndSubmitProgress()">end</button>
    <button onclick="lResetSubmitProgress()">reset</button>
#### 5、实现控制方法
偷个懒，引入jq

    <script src="http://cdn.bootcss.com/jquery/1.12.4/jquery.js"></script>
    <script>
        function lStartSubmitProgress() {
            lResetSubmitProgress();
            setTimeout(function () {
                $(".l-progress-bar-01").addClass("l-start");
                $(".l-progress-bar-01").addClass("l-run");
                setTimeout(function () {
                    $(".l-progress-bar-01").addClass("l-full");
                },100);
            },100);
        }
        function lEndSubmitProgress() {
            $(".l-progress-bar-01").addClass("l-end");
            setTimeout(function () {
                lResetSubmitProgress();
            },200);
        }
        function lResetSubmitProgress() {
            $(".l-progress-bar-01")
                    .removeClass("l-run")
                    .removeClass("l-start")
                    .removeClass("l-full")
                    .removeClass("l-end");
        }
    </script>

到这里，进度条的长度已经可以被控制了，但是没有动画，下面来添加动画
#### 6、添加过渡动画效果
我们希望进度条从__开始状态__到__快结束状态__的时间为10s,并且速度是先快后慢

    .l-progress-bar-01.l-run{
        transition: width 10s;
        transition-timing-function: cubic-bezier(0,0,0,1);
    }
然后，我们希望进度条从__运行状态__到__结束状态__的时间为100ms

    .l-progress-bar-01.l-run.l-end{
        width: 100%;
        transition: width 100ms;
        transition-timing-function: cubic-bezier(0,0,0,1);
    }
### 原理
css3 的 transition 属性定义元素的部分样式变化过程所需要的时间和变化曲线。

定义好进度条的基础样式和过渡样式后。
* 当发起请求时，用js把先它的宽度设置为0%,然后马上再把它的宽度设置为99%，这时进度条就会从0%缓慢增长到99%。
* 当请求完成时，用js把先它的宽度设置为100%，然后把进度条隐藏。
