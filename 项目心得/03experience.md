相关公式与算法
==

[toc]

#### 1、指定点缩放
指定点坐标与当前对象坐标的差值，乘以缩放值增量与当前缩放值的比值，最后用当前对象坐标值减去已差值与比值的乘积，就得出了缩放后，当前对象以指定点为基准的新坐标值。
```js
    let oldScale = this._levelSceneUI.scaleX;
    this._levelSceneUI.x = this._levelSceneUI.x - (pt.x - this._levelSceneUI.x) * scaleValue / oldScale;
    this._levelSceneUI.y = this._levelSceneUI.y - (pt.y - this._levelSceneUI.y) * scaleValue / oldScale;
```

#### 2、有关数值变化的动画，需要每帧都运行，才能保证动画的流畅
#### 3、摇杆角度转换对象移动坐标值
算出摇杆与中心点距离；计算出摇杆坐标值与摇杆中心坐标值的差值，再算出摇杆相对中心点的方位角。
```js
    let dis = MathUtils.dis(this.stickX, this.stickY, this.mCenterX, this.mCenterY);
    let deltaX = this.stickX - this.mCenterX;
    let deltaY = this.stickY - this.mCenterY;
    this.mAngle = Math.atan2(deltaX, deltaY);
```
将摇杆的方位角转换为弧度，再根据正弦、余弦函数求出x方向与y方向的单位偏移量，最后乘以偏移系数，控制偏移量大小。
```js
    this.mAngle = Math.PI / 180 * this.mAngle;
    this._disX = Math.sin(this.mAngle) * this._stickOffsetXY;
    this._disY = Math.cos(this.mAngle) * this._stickOffsetXY;
```
#### 4、屏幕适配相关
适配屏幕宽如下：
```js
let aspectRatio = 16/9;
let realAR = Laya.Browser.width/Laya.Browser.height;
let bool = realAR>aspectRatio;//true则为刘海屏（如果是手机）
let resultW = Math.floor(realAR/aspectRatio * Laya.stage.designWidth);
let offsetY = (realAR-aspectRatio)**Laya.stage.height*0.5;
```


#### 5、git添加忽略文件(.gitignore)
创建一个.gitignore文件，向该文件中添加要忽略的文件或目录。但在创建并编辑这个文件之前，一定要保证要忽略的文件没有添加到git索引中。使用命令git rm –cached filename将要忽略的文件从索引中删除。
```sh
git rm -r -cached .
```

#### 6、Button拓展类
比如集成按钮音效等
#### 7、复杂的子系统，应该继续细分成子系统
比如战斗系统里边的很多模块，动画管理，缓动管理，计时器管理等等；
数据持久模块也应该单独设计成一个系统，便于存取。

#### 8、多次重复执行的事件帧听，不要用闭包回调，最好监听和移除配对
#### 9、对定时器的管理要严谨，有中断性的逻辑要执行的，则需要把相关的延时定时器删除，以免引起逻辑混乱，如果是依次执行的任务，则最好是加入任务队列的方式，保证顺序正确
#### 10、游戏时间确定延时之类的方法，而不用client的自然时间，防止游戏卡顿带来的逻辑不同步
#### 11、资源加载策略，进行优先级管理，先显示的先加载，后显示的可以延后加载，尽可能减少用户进入游戏的等待时间

