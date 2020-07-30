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
