#### Rigidbody 刚体

>Is a object add to world physics simulation.

>是一个对象添加到世界物理模拟。

#### Attribute is a object with this options

##### type 类型 物理物体的类型，球体、长方体、圆柱体
>the type of shape 'box' 'sphere' 'cylinder' can be a array of shape for compound object

>类型可以是复合对象的形状数组

##### move 移动 物理物体是否是静态的
>static od dynamic object

>静态od动态对象

##### size 尺寸  物理物体的大小
>array of size of shape [ x, y, z ]

##### pos 位置 物理物体的位置
>array of position of shape [ x, y, z ]

##### rot 旋转 物理物体的旋转角度
>array of rotation in degree of shape [ x, y, z ]

##### density 密度 物理物体的密度，可以用来增加物体的质量
>density of the shape

##### friction 摩擦 物理物体的摩擦系数
>coefficient of friction of the shape

>摩擦系数

##### restitution 恢复  物理物体的弹性系数
>coefficient of restitution of the shape

>恢复系数

##### belongsTo 属于 物理物体所属的组别
>bits of the collision groups to which the shape belongs

>形状所属的碰撞组的位

##### collidesWith
>bits of the collision groups with which the shape collides

>与该形状碰撞的碰撞组比特

***
####  body.prototype 常用属性

##### allowSleep ----- 是否允许刚体休眠
##### currentRotation ------ 当前的旋转角度（使用欧拉角表示）
##### isStatic ------ 是否静态
##### newOrientation ------ 用来更新物理物体的四元数时的暂存
##### newPosition ------ 用来位置更新，功能同上
##### newRotation ------ 用来角度更新，功能同上
##### orientation ------ 当前的四元数，可以直接修改
##### parent ------ 物理节点的父节点
##### position ------ 当前的位置，可以直接修改
##### rotation ------ 当前旋转角度，可以直接修改
##### sleeping ------ 当前休眠状态

***
#### Functions

##### obj.remove()
>remove object from World

>从世界中删除对象

##### obj.getPosition()
>return vector of object position

>返回对象位置向量

##### obj.getQuaternion()
>return quaternion of object rotation

>返回对象旋转的四元数

***
#### 常用方法：

##### addShape ------ 给刚体添加一个形状，添加完以后必须调用setupMass才能生效
##### awake ------ 强行唤醒休眠中的刚体，防止当物体进入休眠后就不会再动的问题
##### getAxis ------ 获取物理物体的旋转轴
##### getPosition ------ 获取物理物体的位置
##### getQuaternion ------ 获取物理物体的四元数
##### isLonely ------- 判断物理物体是否和别的物体没有接触
##### remove ---- 删除物理物体
##### removeShape ------ 删除形状
##### resetPosition ------ 重置位置
##### resetQuaternion ------ 重置四元数
##### resetRotation ------ 重置角度
##### setPosition ------ 设置位置
##### setParent ------ 设置父级
##### setRotation ------ 设置角度
##### setQuaternion ------ 设置四元数
##### sleep ------ 强制进入刚体休眠
##### world.getContact(xxx,xxx) ------ 碰撞检测函数，只返回true和false，无法知晓是谁碰了谁
***

```ts
var box, mbox;

function demo() {

    cam ( 0, 20, 40 );

    world = new OIMO.World();
    world.add({ size:[50, 10, 50], pos:[0,-5,0] }); // ground 地面

// 向物理世界添加物理物体
    var options = {
        type:'box',
        size:[10, 10, 10],
        pos:[0,20,0],
        density:1,
        move:true
    }

    box = world.add( options );
    mbox = view.add( options ); // three mesh

};

function update () {

    world.step();
    mbox.position.copy( box.getPosition() );
    mbox.quaternion.copy( box.getQuaternion() );

}
```

***

#### 总结
>1.物理世界尽量保持60帧的刷新频率才能计算精准

>2.物理世界的算法尽量使用2号算法，比较稳定

>3.物理世界改变物体时必须每帧调用 world.step() 否则不会生效

>4.当物理物体进入刚体休眠是，必须重新唤醒或使用另一个非休眠的刚体与之发生碰撞才可以唤醒

>5.所有在赋值四元数类型或欧拉角类型的变量时，必须使用oimo自己定义的四元数和欧拉角（new OIMO.Quat，new OIMO.Vec3）

>6.当物体设置为静态时，不会产生物理变化，必须设置为动态

>7.尽量让物理世界的坐标系和实际坐标系的构建方式一样，否则使用起来会有很多坑

>8.看实际的物理表现时，需要实时的将物理物体的属性赋值给可见物体

>9.当物理世界暂时不需要时请及时关闭物理世界的计算，以免引起不必要性能开销

>10.当开启重力时，物理世界的所有物体都会受到重力影响，如需去掉影响，需要手动调整该物体的表现

>11.注意无效物理物体的即时回收

