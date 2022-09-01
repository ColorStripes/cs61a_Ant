# CS61A

## OK系统的使用

- 虚拟机翻墙链接[Ubuntu22.04 vm虚拟机使用宿主机Clash for Windows - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/518319836)

- 比较好的参考代码:

  - [cs61a-spring-2021/project/ants at main · wuxueqian14/cs61a-spring-2021 (github.com)](https://github.com/wuxueqian14/cs61a-spring-2021/tree/main/project/ants)
  - [[2021 Spring\] CS61A Project 3: Ants Vs. SomeBees (Phase 4) - ikventure - 博客园 (cnblogs.com)](https://www.cnblogs.com/ikventure/p/14994734.html)
  - [[2021-Fall\]Project3. Ants vs. Bees of CS61A of UCB - 点击领取 (dianjilingqu.com)](https://www.dianjilingqu.com/86388.html)

- `python3 -m doctest lab04.py`  测试所有文本里的测试是不是通过，以lab04为例

  - 对于**`WWPD`**问题（What Would Python Display?）

    **Fuction**代表是函数调用，**Error**代表错误，**Nothing**代表无输出

    For all WWPD questions, type **Function** if you believe the answer is <function...>, **Error** if it errors, and **Nothing** if nothing is displayed.

  - 对于解锁测试用例 

    - 判断题用 **True** or **False** 
    - 其他题目 **Number** or **String**

  - **Example**：

    ~~~python
    ```
    #问题
    1+2+3  
    >>>
    #答案
    6   
    ```
    ~~~

- 你需要做的是 在每个问题的后面加`-u` 选项，去回答其中的问题，然后写代码解决problem，运行无`-u`选项的命令测试代码





## Ants

- 官方链接：[Project 3: Ants Vs. SomeBees | CS 61A Fall 2021 (berkeley.edu)](https://inst.eecs.berkeley.edu/~cs61a/fa21/proj/ants/#project-submission)

- 每回合每只蚂蚁都会使食物增长
- 同一类蚂蚁放置成本相同
- 类似植物大战僵尸

![Ants vs. Somebees](CS61A/splash.png)



### part1

#### problem1 (Initial)

- **01.py** 就是让按照此表填充数字回答问题，而**partB**，则是在*ants/ants.py*中添加**class HarvesterAnt**的食物增长模式

  ![image-20220830220853203](CS61A/image-20220830220853203.png)

- 收割者每回合+1食物

  投石者向前方扔石头攻击蜜蜂

![image-20220830213138067](CS61A/image-20220830213138067.png)



#### problem2 (Place)

- ![image-20220830213938717](CS61A/image-20220830213938717.png)

- Place 就是一个类它需要记录两个东西，一个是entrance,另一个是exit. 有点类似双向链表。它还可以帮助Ant确定它前面有多少Bees

  In this problem, you'll complete `Place.__init__` by adding code that tracks entrances. Right now, a `Place` keeps track only of its `exit`. We would like a `Place` to keep track of its entrance as well. A `Place` needs to track only one `entrance`. Tracking entrances will be useful when an `Ant` needs to see what `Bee`s are in front of it in the tunnel.

- 新创建的Place的entrance 是 None（空）

  A newly created `Place` always starts with its `entrance` as `None`.

- 如果Place有exit成员，将这个exit的entrance设置为这个Place

​		If the `Place` has an `exit`, then the `exit`'s `entrance` is set to that `Place`.

- *Hint:* Try drawing out two `Place`s next to each other if things get confusing. In the GUI, a place's `entrance` is to its right while the `exit` is to its left.![image-20220830222457012](CS61A/image-20220830222457012.png)

#### problem3 (ThrowerAnt)

- 从当前投石者的位置开始向前遍历

​		Start from the current `Place` of the `ThrowerAnt`.

- 对于每个地方，如果有很多蜜蜂则返回随机一只幸运儿，如果没有Bee则向前遍历，也就是向entrance方向遍历

  For each place, return a random bee if there is any, and if not, inspect the place in front of it (stored as the current place's `entrance`).

- 前面没蜜蜂则返回None

  If there is no bee to attack, return `None`.

- solution:

  ![image-20220831001648321](CS61A/image-20220831001648321.png)

  - 值得注意的是这里利用了Place的属性，`self.place`在循环当中不能改变，所以需要创建新的变量
  - .bees成员为空时是【】，python中认为【】为假
  - 利用next_place 向entrance方向递进
  - python3 文件问题![image-20220831003442781](CS61A/image-20220831003442781.png)

### part2

#### problem4（Inherit）

- 继承的之前投石者的类，只改变其中的两个range成员

- float('inf')    正无穷

  float('-inf')   负无穷

- 加入新变量 `distance` 判断攻击距离

  ![image-20220831011316167](CS61A/image-20220831011316167.png)

- ![image-20220831011816025](CS61A/image-20220831011816025.png)

#### problem5 (FireAnt)

- 首先要知道self指的是![image-20220831134411899](CS61A/image-20220831134411899.png)类

- 其次，![image-20220831135153567](CS61A/image-20220831135153567.png)遍历所有list的方法（这里的list指的是Bees)

  ​																		 **！！！重点来了！！！**

- 提示：在遍历列表时，如果同时修改列表，则无法遍历全部内容。需要使用list副本遍历，list本身的改变不会影响list副本。

  *Hint:* Damaging a bee may cause it to be removed from its place. If you iterate over a list, but change the contents of that list at the same time, you [may not visit all the elements](https://docs.python.org/3/tutorial/controlflow.html#for-statements). This can be prevented by making a copy of the list. You can either use a list slice, or use the built-in `list` function.

  - *因为list是由下标索引的，remove的时候会将后面元素的下标前移来补充空缺下标，而for却是由下标递增来进行遍历，导致跳过remove后的一个元素*
  - list副本创建两个方法：
    - `lst[:]`
    - `list(lst)`
    - https://blog.csdn.net/qianlixiushi/article/details/124341462

- 提示：不要调用 `self.reduce_health`，否则将会陷入死循环

​		*Hint:* Do *not* call `self.reduce_health`, or you'll end up stuck in a recursive loop.

- 当火蚁死亡回收蚂蚁：（只要调用`reduce_health`火蚁就会死，它只有1滴血）

  需要用`super（）`去调用FireAnt的超类的方法（**Ant类（父类）->Insect类（Ant的父类）的方法 `health_reduce`**）

  To implement this, override FireAnt's reduce_health method. <u>Your overriden method should call the `reduce_health` method inherited from the **superclass** (Ant) to reduce the current FireAnt instance's health.</u> Calling the inherited reduce_health method on a FireAnt instance reduces the insect's health by the given amount and removes the insect from its place if its health reaches zero or lower.

  - 火蚁回收必须调用`Insect`的`reduce_health`方法，其中判断血量并使用death_callback回收

    ![image-20220831193524469](CS61A/image-20220831193524469.png)

  - 这里还需要注意，父类中没有重写`health_reduce`，父类是从超类中继承而来，而超类方法的作用为：

    ![image-20220831145210177](CS61A/image-20220831145210177.png)

  - 这里的![image-20220831145452111](CS61A/image-20220831145452111.png)则是Bees元素直接调用`Insect`类的`health_reduce`方法

  - 这里的![image-20220831145700757](CS61A/image-20220831145700757.png)是FireAnt调用超类`Insect`类的`health_reduce`方法

  - 而FireAnt中的`health_reduce`方法则是对从Ant类继承的方法进行重定义

  - [(48条消息) Python基础：super()用法_硝烟_1994的博客-CSDN博客_python super()](https://blog.csdn.net/qq_44804542/article/details/116173195)

- ![image-20220831140529499](CS61A/image-20220831140529499.png)

### part3

#### problem6（WallAnt）

- no problem because it is so easy,just to type it samply as FireAnt and no `def` 

- ![image-20220831151413546](CS61A/image-20220831151413546.png)

#### problem7（HungryAnt）

- 需要实现`chew_duration`和`chew_countdown`

  Give `HungryAnt` a `chew_duration` **class** attribute that stores the number of turns that it will take a `HungryAnt` to chew (set to 3). 

- 实现`action`方法

  implement the `action` method of the `HungryAnt`

- 将Bee的生命值瞬间降为0 ： die_bee.reduce_health(die_bee.health)    **减少当前全部血量**

- ![image-20220831162001334](CS61A/image-20220831162001334.png)

#### problem8（BodyguardAnt）

第一部分：`ContainerAnt`类

- `can_contain`：是否可以被包含
  - This ContainerAnt does not already contain another ant.
  - The other ant is not a container.

- `store_ant`：保存要保护的蚂蚁 （注意这里的`store_ant`是方法，改变的是`ant_contained`变量）

- `ant_contained` that stores the ant it contains，initial is `None`

-  `action` method to perform its `ant_contained`'s action if it is currently containing an ant

  （也就是说action的行为是它所保护蚂蚁的行为）

第二部分：`Ant`类

- `add_to`方法重构：   用  是否可以被保护方法  `can_contain`
  - original ant contains the ant being added(self)      **原来在此位置的蚂蚁可以包含被添加的蚂蚁**
  - the (container) ant being added contains the original ant   **被添加的蚂蚁可以包含原来在此位置的蚂蚁**
  - **Assert**： neither Ant can contain the other      **以上两条不成立assert**

第三部分：`BodyguardAnt`类

- 只需要传入BodyguardAnt的血量，初始化构造函数完成

  ![image-20220831190420434](CS61A/image-20220831190420434.png)

- ![image-20220831170958050](CS61A/image-20220831170958050.png)

#### problem9（TankAnt）

- 坦克蚁除了血量和food需要重写外，`action`相对于警卫蚁要增加对此处Bees的攻击行为

  ![image-20220831193927563](CS61A/image-20220831193927563.png)

- ![image-20220831190618608](CS61A/image-20220831190618608.png)

### part4

#### problem10（WaterPlace）

- `is_watersafe` 在`Insect`类里是False       (不防水)

- `is_watersafe`在`Bee`类里是True             （防水）

- `Water`

  - First, add the insect to the place regardless of whether it is waterproof.
  - Then, if the insect is not waterproof, reduce the insect's health to 0.

  - ![image-20220831211054362](CS61A/image-20220831211054362.png)

#### problem11(ScubaThrower)

- 潜水蚁直接继承投石者的属性，只不过将`is_waterproof`属性改为True，同时潜水蚁和长投蚁一样，初始血量为1，和`Ant`的初始化保持一致，所以不用重定义`__init__`方法

- ![image-20220831211433241](CS61A/image-20220831211433241.png)

#### problem12（QueenAnt）

- `construct`： （这里和`Water`一样，先创建，然后不满足条件删除）

  - first intance is True

  - secound and other intance is False

  - 根据以上规则，我们首先要确定，怎么追踪Queen是否存在

    - 在`GameState`类里添加`have_queen`属性，用于追踪场上是否有蚁后

      To keep track of whether a queen has already been created, you can use an instance variable added to the current `GameState`.

    - 根据是否有蚁后，在QueenAnt里的`construct`方法中调用父类（Ant）方法`construct`

      **注意这里必须return调用结果，否则queen的第5个测试将出现NoneType调用，因为queen无返回值**

- `action`：

  - 需要根据 `exit`来遍历所有位置，确定蚁后后面的所有蚂蚁
  - 为了标记是否已经强化过damage，需要在`Ant`类做标记is_buff，避免多次强化同一只蚂蚁
  - 在`Ant`类中添加需要的`buff`方法行为，也就是*增强* 和*标记* 行为
  - 然后需要对容器Ant做判断，`Ant`的list中如果有容器蚁只显示容器蚁，所以需要对容器内的蚂蚁，进行也强化

- `reduce_health`：

  - 如果蚁后自身血量为0，则调用ants_lose()游戏结束

    If the queen ever has its health reduced to 0, the ants lose. You will need to override `Ant.reduce_health` in `QueenAnt` and call `ants_lose()` in that case in order to signal to the simulator that the game is over. 

- `remove_from`：

  - 确保场上只有一只蚂蚁，则该方法没有任何作用，不进行任何操作，只是为了重定义继承的方法

- ![image-20220831215304606](CS61A/image-20220831215304606.png)

#### Extra Credit （NewInherit）

- 这里要设置标志，标志的设置跟`HungryAnt`的`chew_countdown`一样
  - `is_slow`:                   if bee is slow          在`action`里的标记
  - `is_scared`:               if bee is scared      在`action`里的标记
  - `slow_duration`:       slow time
  - `scared_duration`:   scared time
  - `have_scared`:      can't be scard twice
- `slow`和`scared`方法只是按规则改变定义的标志

- 现在，我们来修改`action`方法

  - 先设置scared的行为，不考虑减速：

    - 小蜜蜂很害怕，每回合后退一格，要后退两回合

       Bees remain scared until they have tried to back away twice. 

      - 这里的后退只是相对于前进的方向发生改变，只需改变`move_to`方向

    - 检查`Bee`是否已经滚回老巢，如果是则不能后退

      To check if a bee is next to the Hive.

    - 一只蜜蜂只能被吓唬一次，下一次它就不害怕了，scared就不会起作用了

      Once a bee has been scared once, it can't be scared ever again.

    - 如果`Bee`**被减速**并且**全局游戏时间**是奇数，则不能后退

      Bees cannot try to back away if they are slowed and `gamestate.time` is odd

  - 再设置`slow`的行为：

    - 减速3周期

      slowing it for 3 turns.

    - 如果Bee被减速，**全局游戏时间**为偶数时才能向前移动

      When a bee is slowed, it can only move on turns when `gamestate.time` is even.

    - 减速周期可以无限叠加 ，每次被减速，减速周期+3

      If a bee is hit by syrup while it is already slowed, it is slowed for an additional 3 turns.

- ![image-20220901013507026](CS61A/image-20220901013507026.png)

### Optional Problems

#### Optional Problem 1（NinjaAnt）

- 首先，在`Ant`类里添加`blocks_path`属性并设置为`True`

  first modify the `Ant` class to include a new class attribute `blocks_path` that is set to `True`.

- 之后在`NinjaAnt`类里修改为`False`

  then override the value of `blocks_path` to `False` in the `NinjaAnt` class.

- 修改`Bee`类的方法，如果有蚂蚁并且蚂蚁的`blocks_path`属性不为`False`，返回True

  modify the `Bee`'s method `blocked` to return `False` if either there is no `Ant` in the `Bee`'s `place` or if there is an `Ant`, but its `blocks_path` attribute is `False`.

- 在`NinjaAnt`类重写`action`方法：

  - 伤害和忍者蚁同位置的所有`Bee`，遍历`Bees[:]`并`reduce_health`减少`damage`点数

- ![image-20220901181837043](CS61A/image-20220901181837043.png)

#### Optional Problem 2（LaserAnt）

- 伤害它前面所有蚂蚁蜜蜂，除了Hive里的蜜蜂

   it will damage all `Insect`s in its place and the `Place`s in front of it, excluding the `Hive`.

- 除了同自己，它连保护自己的容器蚁都伤害（典型六亲不认）

  excluding itself, but including its container if it has one.

- `LaserAnt`类：

  - 初始伤害2

  - `insects_in_front`方法：

    - 需要返回一个字典，包含激光蚁前所有昆虫的距离信息（除自己）

      returns a dictionary where each key is an `Insect` and each corresponding value is the distance (in places) that that `Insect` is away from `LaserAnt`.

    - 与激光蚁同位置的蜜蜂距离为0

      bee in same place's distance with LaserAnt is 0.

    - 考虑激光蚁是否有容器蚁保护，距离也为0

      container's information.

    - 遍历所有前面的昆虫，记录距离信息

      distance (in places) that that `Insect` is away from `LaserAnt`.

    - 遍历规则是：

      - 判断当前不是蜂巢，将当前位置的`Bee`的距离记录

      - 然后记录当前位置的`Ant`的距离

      - 再判断当前位置的`Ant`的容器蚁里面蚂蚁的距离

        注意此处判断需要判断Ant是否是容器蚁并且里面包含的蚂蚁不为`None`

      - 距离+1

      - 下一个位置为：当前位置的`entrance`

  - `calculate_damage`方法：

    - 初始伤害为2

      The `LaserAnt` has a base damage of `2`.

    - 离激光蚁的距离乘以0.25为衰减伤害

      The laser is weakened by `0.25` each place it travels away from `LaserAnt`'s place. 

    - 激光蚁此回合如果打中一个`Insect`则激光蚁伤害降低0.0625

      Each time `LaserAnt` actually damages an `Insect` its laser's total damage goes down by `0.0625` (1/16).

    - 如果伤害降低于0，则伤害为0 

      If `LaserAnt`'s damage becomes negative due to these restrictions, it simply does 0 damage instead.

    - 我们需要知道激光蚁打中多少`Insect`，则要调用`insects_shot`

    - 返回计算的最终伤害

- ![image-20220901201408148](CS61A/image-20220901201408148.png)

### Project submission

- run with command ： `python3 ok --local`

  you will see this

  ![image-20220901223337622](CS61A/image-20220901223337622.png)

- Congratulations！ All test had passed！And you got all score！

  ![image-20220901223653756](CS61A/image-20220901223653756.png)

- 运行界面演示

  ![image-20220901233334135](CS61A/image-20220901233334135.png)

  

  ​												       	                    **You are now done with the project!**

  

### PlayGame

#### GUI game cmd：

```shell
 python3 gui.py [-h] [-d DIFFICULTY] [-w] [--food FOOD]
```

-   -h, --help    								 **show this help message and exit**
    -d DIFFICULTY  						  **sets difficulty of game (test/easy/normal/hard/extra-hard)**
    -w, --water   							   **loads a full layout with water**
    --food FOOD    					      **number of food to start with when testing**

#### All Ant

![image-20220901225944109](CS61A/image-20220901225944109.png)

![image-20220901230010100](CS61A/image-20220901230010100.png)

![image-20220901230031313](CS61A/image-20220901230031313.png)

![image-20220901230101996](CS61A/image-20220901230101996.png)

![image-20220901230122060](CS61A/image-20220901230122060.png)

![image-20220901230143470](CS61A/image-20220901230143470.png)

![image-20220901230204749](CS61A/image-20220901230204749.png)

![image-20220901230241684](CS61A/image-20220901230241684.png)

![image-20220901230300533](CS61A/image-20220901230300533.png)

![image-20220901230317330](CS61A/image-20220901230317330.png)

![image-20220901230329192](CS61A/image-20220901230329192.png)

![image-20220901230345776](CS61A/image-20220901230345776.png)

## Github URL

- 本项目仓库地址 ：https://github.com/ColorStripes/cs61a_Ant

  First commit by xu xin at **2022.9.1** 

