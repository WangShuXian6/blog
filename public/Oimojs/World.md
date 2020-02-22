#### World
>Is the container of physics simulation

#### Attribute is a object with this options

##### timestep: 时间步长
>the time between each step default 1/60 is 60 frame second

> 默认1/60的每一步之间的时间是60帧秒

>物理世界的刷新频率，通常为60帧每秒，之前在项目中为了提高性能，降低cpu的消耗，将此值改为1/30即30帧每秒，导致原先计算准确的物理碰撞发生计算不灵敏的情况，尤其是在开启重力感应后，和重力感应相关的物理碰撞计算，建议尽量维持60帧，除非你所需要计算的内容对精度要求真的不高，只要模拟个大概。

##### iterations: 迭代
>the number of iterations for constraint solvers.

> 约束求解器的迭代次数。

##### broadphase: 碰撞检测算法类型
>the simulation collision algorithm 1: brute force, 2: sweep  prune, 3: volume tree

> 模拟碰撞算法:   1：强力，2：扫描和修剪，3：体积树

>1 暴力算法 

>2 及／或扫掠裁减（sweep and prune）算法，这是目前市面上最常见的碰撞检测算法 

>3 volume tree算法

>目前探究发现，使用2号算法是最稳定的，但是所要花费的性能也是最高的

##### worldscale: 世界规模 物理世界的缩放
>world system units is 0.1 to 10 meters max for dynamic body can be multiplied by this number

> 世界系统单位最大0.1到10米，动态体可以乘以这个数

##### random: 随机 . 是否使用随机样本
>use extra random number in simulation

> 在模拟中使用额外的随机数

##### info: 
>enable timing for display accurate simulation statistic

> 启用计时显示精确模拟统计

##### gravity: 重力加速度的大小，x，y，z三个方向可设置
>a array to define the start gravity [ x, y, z ]

>用于定义起始重力[x，y，z] 
#### Functions
##### world.step(): 
>Proceed only time step seconds time of World

> 继续只进行世界时间步秒时间

##### world.clear(): 
>Reset the world and remove all rigid bodies, shapes, joints and any object from the world

> 重置世界并移除世界上所有刚体，形状，关节和任何物体

##### world.getInfo(): 
>return string of simulation statistic if enable

> 如果启用则返回模拟统计信息的字符串

##### world.setGravity( [ x, y, z ] ): 
>set array world gravity

> 设置阵列世界引力 

##### world.add({}): 
>add someting to world

>添加一些东西到世界，返回刚体或关节

***

```ts

function demo() {

    cam ( 0, 20, 40 );

// 创建一个Oimo物理世界
    world = new OIMO.World({ 
        timestep: 1/60, // 时间步长
        iterations: 8, // 迭代
        broadphase: 2, // 广角
        worldscale: 1, // 世界规模
        random: true, // 随机
        info:false,
        gravity: [0,-9.8,0] // 重力
    });

};

function update () {

    world.step();

}
```