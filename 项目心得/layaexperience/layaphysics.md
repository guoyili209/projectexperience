Laya物理
==

##### 1、刚体及常用刚体属性
###### 1、isKinematic 是否为运动刚体
运动刚体运动刚体可以触发第三方的物理反馈，自己却不受物理影响。运动刚体不会受力的影响，不会产生受力位移，运动刚体的位移只能通过transform改变节点坐标。
###### 2、mass质量
质量是物质的量的量度，Bullet引擎中的质量单位为kg。刚体的质量越大，运动状态改变越难。
###### 3、gravity重力
动力学刚体在同等的质量下，重力越大，下落的加速度越大。
###### 4、angularVelocity 角速度
刚体的angularVelocity属性是角速度， 角速度简单理解就是单位时间的角位移，以弧度每秒进行旋转 。当我们设置动力学刚体angularVelocity属性为正值的时候，则按顺时针旋转位移。angularVelocity属性为负值的时候，则按逆时针旋转位移。属性值的绝对值越大，旋转位移速度越快。
###### 5、angularDamping 角阻尼
刚体的角阻尼相当于是为角速度旋转方向施加了相反的力，使得旋转速度衰减。角阻尼为1则完全衰减。
###### 6、linearVelocity 线性速度
刚体的linearVelocity属性称为线速度或者线性速度，是指物体的直线运动速度。
动力学刚体的线速度是3维向量Vector3类型值，向量的方向即速度的方向，向量的长度即速度的大小。
###### 7、linearDamping 线性阻尼
刚体的linearDamping属性，是指线性速度的阻尼系数，使得线性速度衰减。阻尼为1则完全衰减。
##### 2、物理碰撞
###### 1、碰撞器
完整的3D碰撞器，由碰撞器和碰撞器形状两部分组成。
3D碰撞器根据特点的不同，分为静态碰撞器、刚体碰撞器、角色碰撞器。
这些碰撞器必须要添加三维碰撞器形状（例如：盒形、球形、圆锥形、圆柱形、胶囊形、平面、混合、模型网格），才可以实现有范围的物理碰撞。
###### 2、触发器
触发器是碰撞器的一个属性，任何碰撞器的触发器属性设置生效后，当前的碰撞器即转变为触发器（比如，刚体碰撞器设置触发器后可称为刚体触发器）。即使发生物体接触，也不会产生碰撞的物理反馈。
设置触发器后，虽然失去了物理引擎反馈，但是可以激活触发器的碰撞生命周期方法，用于检测物体间碰撞接触的发生。
通过代码设置触发器的方式：
```ts
//创建盒型MeshSprite3D
let box = scene.addChild(new Laya.MeshSprite3D(Laya.PrimitiveMesh.createBox(sX, sY, sZ))) as Laya.MeshSprite3D;
//创建静态碰撞器
let staticCollider:Laya.PhysicsCollider = box.addComponent(Laya.PhysicsCollider);
//设置为触发器,取消物理反馈
staticCollider.isTrigger = true;
```
###### 3、静态碰撞器 PhysicsCollider
它的特性是不受力，不会产生物理移动。
当其与动力学刚体碰撞器或角色碰撞器发生物理碰撞后，可以触发物理碰撞生命周期方法，但不会产生物理的受力位移。
###### 4、刚体碰撞器 Rigidbody3D
Rigidbody3D类即是刚体也是碰撞器，我们可称为刚体碰撞器。
默认情况下，Rigidbody3D是动力学类型的刚体碰撞器，这是可以受力影响的刚体类型碰撞器，所以我们通常用动力学刚体碰撞器进行受力的交互反馈。
当我们将刚体Rigidbody3D的isKinematic设置为true后，那么默认的动力学刚体碰撞器就转变为运动刚体碰撞器。
运动刚体碰撞器从表象上看，与静态碰撞器基本上没有什么区别。都是不受重力、不受速度、不受其它力的影响，在物理世界中永远处于静止，只能通过transform去改变节点坐标来移动。

但实质上，运动刚体有物理特性，它可以是施力物体，可以对非运动刚体产生力，例如通过控制节点去移动运动刚体，会推着挡在前面的动力学刚体移动。而静态碰撞器的应用场景则是要永远不动，也无法施加力。并且，通过节点去移动静态碰撞器，也比较消耗性能。如果有移动的碰撞器需求，例如来回移动的跳板或障碍，使用运动刚体碰撞器就可以了。

通过代码设置运动刚体的方式：
```ts
//创建刚体碰撞器
let _rigidBody = sphere.addComponent(Laya.Rigidbody3D) as Laya.Rigidbody3D;
//开启运动类型刚体
_rigidBody.isKinematic = true;
```
由于LayaAir的3D物理中有了静态碰撞器PhysicsCollider，所以并没有在Rigidbody3D中去实现静力学类型的刚体碰撞器。有静止的碰撞反馈需求，直接使用静态碰撞器即可。
###### 5、角色碰撞器 CharacterController
角色控制器类CharacterController常用于对第一人称和第三人称游戏角色的控制，可以方便的控制角色的跳跃、跳跃速度、降落速度、行走、等。
由于角色控制器继承于PhysicsComponent，也具有碰撞器的特性，可以添加三维碰撞形状，产生碰撞的反馈，因此也称为角色碰撞器，属于碰撞器之一。
与静态碰撞器和刚体碰撞器都继承自物理触发器组件PhysicsTriggerComponent不同，角色控制器直接继承于物理组件的父类PhysicsComponent。所以，角色控制器是无法设置为触发器的。但是，角色碰撞器与触发器进行接触，仍然可以激活触发器的生命周期方法。
###### 6、碰撞形状
碰撞形状是用于检测碰撞接触的范围，只有添加了形状，碰撞器和触发器才能触发物理反馈和生命周期。
LayaAir引擎支持8种3D碰撞形状，分别为：
盒形BoxColliderShape、球形SphereColliderShape、圆柱形CylinderColliderShape、胶囊形CapsuleColliderShape、圆锥形ConeColliderShape、平面形状StaticPlaneColliderShape、复合形状CompoundColliderShape、网格形状MeshColliderShape。
Unity中的盒形碰撞体Box collider、球形碰撞体Sphere Collider、胶囊形碰撞体Capsule Collider、网格碰撞体 Mesh Collider，这4种组件是可以通过LayaAir导出插件直接导出使用的。
这些组件包括了碰撞形状，无需通过引擎代码添加碰撞形状，所以对于盒形、球形、胶囊形、网格形、以及由以上基础形状碰撞体组合而成的复合碰撞形状。都建议在Unity里编辑导出使用。

Unity导出的碰撞组件使用起来最简单，由于组件已经整合了碰撞器和碰撞形状，直接加载就可以使用了。
```ts
Laya.Scene3D.load("Conventional/SampleScene.ls", Laya.Handler.create(null, function (_Scene3D: Laya.Scene3D) {
            //添加3D场景到舞台
            Laya.stage.addChild(_Scene3D);
            let _camera = _Scene3D.getChildByName("Main Camera") as Laya.Camera;
            _camera.clearFlag = Laya.CameraClearFlags.Sky;
            //从场景中找到圆柱对象
            let _cylinder = _Scene3D.getChildByName("Cylinder");
            //从圆柱对象上获得刚体碰撞器（对应Unity的刚体组件）
            let cyRigid = _cylinder.getComponent(Laya.Rigidbody3D) as Laya.Rigidbody3D;
            //创建圆柱体形状（通常与圆柱对象的大小保持一致）
            let cyShape = new Laya.CylinderColliderShape(0.5,2);
            //为刚体碰撞器添加碰撞形状
            cyRigid.colliderShape = cyShape;
        }));
```

**LayaAir内置的基础碰撞形状使用示例**
我们以创建圆锥形刚体碰撞器为例，编写代码如下所示：
```ts
/**增加圆锥形刚体碰撞器 */
    private addCone(): void {
        //生成随机值半径和高
        let raidius = Math.random() * 0.2 + 0.2;
        let height = Math.random() * 0.5 + 0.8;
        //创建圆锥形3D模型节点对象
        let cone = new Laya.MeshSprite3D(Laya.PrimitiveMesh.createCone(raidius, height));
        //把圆锥形3D节点对象添加到3D场景节点下
        this.newScene.addChild(cone);
        //设置随机位置
        this.tmpVector.setValue(Math.random() * 6 - 2, 6, Math.random() * 6 - 2);
        cone.transform.position = this.tmpVector;
        //为圆锥形3D节点对象创建刚体碰撞器
        let _rigidBody = <Laya.Rigidbody3D>(cone.addComponent(Laya.Rigidbody3D));
        //创建圆锥形碰撞器形状（使用节点对象的值，保持一致性）
        let coneShape = new Laya.ConeColliderShape(raidius, height);
        //为刚体碰撞器添加碰撞器形状
        _rigidBody.colliderShape = coneShape;
    }
```
###### 7、碰撞生命周期
碰撞事件生命周期方法说明：
碰撞器之间发生碰撞后，自动激活的事件虚方法。
onCollisionEnter 刚发生物理碰撞时
onCollisionStay 持续的物理碰撞时
onCollisionExit 物理碰撞结束时

触发事件生命周期方法说明：
设置为触发器之后，因物体接触而自动激活的事件虚方法。
onTriggerEnter	刚发生物体接触时
onTriggerStay	持续的物体接触时
onTriggerExit	物体接触结束时

**碰撞事件生命周期方法的触发条件**
静态碰撞器和静态碰撞器，静态碰撞器和运动刚体碰撞器，运动刚体碰撞器和运动刚体碰撞器,不能触发

**触发事件生命周期方法的触发条件**
碰撞器是只能与碰撞器之间碰撞，才有可能进入碰撞器的生命周期，
而触发器则不然，触发器不仅与触发器之间有可能进入触发器的生命周期，当触发器与碰撞器之间接触，也有可能进入触发器的生命周期

触发器与触发器之间：
静态触发器和静态触发器，不能触发

触发器与碰撞器之间：
静态触发器和静态碰撞器，不能触发

###### 8、碰撞生命周期
####### 1、 碰撞组 collisionGroup
LayaAir引擎内置了17个碰撞组属性值，用于过滤不需要的碰撞。
**全部可碰撞的组**
由于碰撞组之间的碰撞依据是位运算的按位与，按位与的计算结果非0则可以碰撞，为0则不可碰撞。
Physics3DUtils工具类的COLLISIONFILTERGROUP_ALLFILTER属性值为-1，-1与任何2的幂值进行按位与都非0，所以取该属性值为分组时，则所有的碰撞组都可碰撞。
```ts
//指定xxx碰撞器所属哪个碰撞组（-1组与LayaAir任何内置组都可碰撞）
xxx.collisionGroup = Laya.Physics3DUtils.COLLISIONFILTERGROUP_ALLFILTER;
```
**自定义碰撞分组**
LayaAir内置的碰撞组，不包括刚刚讲的-1（COLLISIONFILTERGROUP_ALLFILTER），我们可以用的还有10个，分别是COLLISIONFILTERGROUP_CUSTOMFILTER1......10。全都是2的幂，从64到32768。
为了方便记忆，我们可以不记实际值，记住CUSTOMFILTER后1到10的ID号区别即可。
```ts
//指定xxx碰撞器所属哪个碰撞组（COLLISIONFILTERGROUP_CUSTOMFILTER2对应的值为128）
xxx.collisionGroup = Laya.Physics3DUtils.COLLISIONFILTERGROUP_CUSTOMFILTER2;
```
**特定的碰撞分组**
除了以上的分组外，LayaAir也对接了一些Bullet物理引擎预留的特定分组，用于比较简单的碰撞过滤需求。
碰撞组属性名	属性值	说明
COLLISIONFILTERGROUP_DEFAULTFILTER	1	默认碰撞组
COLLISIONFILTERGROUP_STATICFILTER	2	静态碰撞组
COLLISIONFILTERGROUP_KINEMATICFILTER	4	运动学刚体碰撞组
COLLISIONFILTERGROUP_DEBRISFILTER	8	碎片碰撞组
COLLISIONFILTERGROUP_SENSORTRIGGER	16	传感器触发器
COLLISIONFILTERGROUP_CHARACTERFILTER	32	字符过滤器

**过滤碰撞组 canCollideWith**
```ts
//指定xxx碰撞器可以与其发生碰撞的碰撞组(本例只与自定义组1碰撞)
xxx.canCollideWith = Laya.Physics3DUtils.COLLISIONFILTERGROUP_CUSTOMFILTER1;
```
**指定碰撞多个组**
```ts
//指定xxx碰撞器可以与其发生碰撞的碰撞组(本例只与自定义组1、2、5进行碰撞)
xxx.canCollideWith = Laya.Physics3DUtils.COLLISIONFILTERGROUP_CUSTOMFILTER1 | Laya.Physics3DUtils.COLLISIONFILTERGROUP_CUSTOMFILTER2 | Laya.Physics3DUtils.COLLISIONFILTERGROUP_CUSTOMFILTER5;
```
**指定不可碰撞的组**
```ts
//指定不可以与其发生碰撞的碰撞组(本例将不与自定义组2、5进行碰撞，除自定义2与5组之外，都可以发生碰撞)
xxx.canCollideWith = Laya.Physics3DUtils.COLLISIONFILTERGROUP_ALLFILTER ^ Laya.Physics3DUtils.COLLISIONFILTERGROUP_CUSTOMFILTER2 ^ Laya.Physics3DUtils.COLLISIONFILTERGROUP_CUSTOMFILTER5;
```
##### 3、物理射线
在LayaAir引擎中，射线常用于基础的碰撞检测，所以具有射线的发射特性，但用于碰撞检测功能的射线称为物理射线。
*需要注意的是，射线可以用于物理射线检测，但是物理射线并不等同于射线。*

LayaAir引擎提供了创建3D空间射线的类Laya.Ray()，以及通过摄像机从屏幕空间点去生成这个射线的方法viewportPointToRay()。
```ts
//创建一个屏幕点
let point = new Laya.Vector2();
//创建一个射线 Laya.Ray(射线的起点，射线的方向)
let ray = new Laya.Ray(new Laya.Vector3(0, 0, 0), new Laya.Vector3(0, 0, 0));
//以鼠标点击的点作为原点
point.x = Laya.stage.mouseX;
point.y = Laya.stage.mouseY;
//计算一个从屏幕空间生成的射线
_camera.viewportPointToRay(point, ray);
```

在LayaAir 3D中实现射线检测是使用物理模拟器类PhysicsSimulation。
射线检测的方法有4个，分别为射线检测第一个碰撞物体的方法raycast 和 raycastFromTo以及射线检测所有碰撞物体的方法raycastAll和raycastAllFromTo。

带FromTo的是使用两个点（射线的起始位置点和结束位置点）作为参数。
而不带FromTo的则是直接使用已经创建好的射线，不需要设置射线的结束位置点，但需要设置长度，如果我们不设置长度，则采用默认值长度2147483647。

```ts
//创建一个屏幕点
let point = new Laya.Vector2();
//创建一个射线 Laya.Ray(射线的起点，射线的方向)
let ray = new Laya.Ray(new Laya.Vector3(0, 0, 0), new Laya.Vector3(0, 0, 0));
//以鼠标点击的点作为原点
point.x = Laya.stage.mouseX;
point.y = Laya.stage.mouseY;
//计算一个从屏幕空间生成的射线
_camera.viewportPointToRay(point, ray);
//拿到3D场景中射线碰撞的物体
_scene3D.physicsSimulation.rayCastAll(ray,this.outs);
//如果射线碰撞到物体
if (this.outs.length !== 0) {
    for (let i = 0; i < this.outs.length; i++){
        //在射线击中的位置添加一个立方体
        this.addBoxXYZ(this.outs[i].point.x, this.outs[i].point.y, this.outs[i].point.z );
    }        
}
```
```ts
//进行射线检测,检测第一个碰撞物体
_scene3D.physicsSimulation.raycastFromTo(this.from, this.to, this.out);
//将射线碰撞到的物体设置为红色
((this.out.collider.owner as Laya.MeshSprite3D).meshRenderer.sharedMaterial as Laya.BlinnPhongMaterial).albedoColor = new Laya.Vector4(1.0, 0.0, 0.0, 1.0);
```

**设置射线碰撞组**
无论是普通射线还是异形射线，都可以设置碰撞组，以及指定射线可碰撞的组。

我们将值带入射线检测对应的方法即可实现射线的选择性碰撞。
射线检测里用于指定检测碰撞组的参数collisionMask对应的是前文介绍的canCollideWith