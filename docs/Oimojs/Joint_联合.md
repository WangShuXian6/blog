#### Joint 联合

>Is link between objects add to world physics simulation.
>对象之间的链接是否添加到世界物理模拟中。

#### Attribute is a object with this options

##### type 类型
>the type of joint 'jointDistance' 'jointHinge' 'jointPrisme' 'jointSlide' 'jointBall' 'jointWheel'

>关节的类型

##### body1
>link attach can be object name or id 

>链接附加可以是对象名称或ID 

##### body2
>link attach can be object name or id

>链接附加可以是对象名称或ID

##### axe1
>array for body1 normal link axis [ x, y, z ]

>body1法线链接轴[x，y，z]的数组

##### axe2
>array for body2 normal link axis [ x, y, z ]

>body2法线链接轴[x，y，z]的数组

##### pos1
>array for position from center of body1 [ x, y, z ]

>body1中心位置的数组[x，y，z]

##### pos2
>array for position from center of body2 [ x, y, z ]

>body2中心位置的数组[x，y，z]

##### collision 碰撞
>active collision between body1 and body2

>body1和body2之间的主动碰撞

#### Functions

##### obj.remove()
>remove object from World

>从世界中删除对象

##### obj.getPosition()
>return array of vector joint points position [ pos1, pos2 ]

>返回矢量关节点位置数组[pos1，pos2]

***

```ts
var box, mbox, base;

function demo() {

    cam ( 0, 20, 40 );

    world = new OIMO.World();
    base = world.add({ size:[10, 10, 10], pos:[0,20,0] }); // ground
    var o = { type:'box', size:[10, 10, 10],  pos:[0,0,0], density:1, move:true };
    box = world.add( o );
    mbox = view.add( o ); // three mesh

    world.add({
         type:'jointHinge',
         body1:base, 
         body2:box,
         pos1:[0,-5,0],
         pos2:[0,5,0]
    });

};

function update () {

    world.step();
    mbox.position.copy( box.getPosition() );
    mbox.quaternion.copy( box.getQuaternion() );

}
```