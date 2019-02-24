# 开源机器人学学习指南 <!-- omit in toc -->

# 目录 <!-- omit in toc -->
- [零. 前前言](#零-前前言)
- [一. 前言](#一-前言)
- [二. 先修知识](#二-先修知识)
- [三. 入门](#三-入门)
  - [3.1 空间变换](#31-空间变换)
  - [3.2 运动学](#32-运动学)
  - [3.3 雅可比矩阵](#33-雅可比矩阵)
  - [3.4 动力学](#34-动力学)
  - [3.5 控制](#35-控制)
- [勇者斗恶龙](#勇者斗恶龙)
- [参考文献](#参考文献)


## 零. 前前言

本文中含有不少公式，为了方便编辑，采用的是 Latex 格式书写。但是，Github Markdown 不支持 Latex，为了方便阅读，建议采用 Chrome 浏览器，并安装插件 [TeX All the Things](https://chrome.google.com/webstore/detail/tex-all-the-things/cbimabofgmfdkicghcadidpemeenbffn)。安装后，即可将网页上的 Latex 公式转换成大多数人能看懂的形式。

本教材写作时间很短，所以肯定有疏漏。因此放在这里作为一个开源项目，大家可以随时修改并提交 Pull Request；有问题也可以提 issue。

## 一. 前言

**[RVBUST INC.](http://www.rvbust.com)** 成立半年有余。面试过不少从事机器人研究的小伙伴后，我发现一个问题：**绝大多数大陆的毕业的学生都不像是「科班出身」的**。

当然，如果仅从工作教育经历上看 —— 大部分毕业于机电、计算机专业，甚至是研究机器人的实验室，有过机器人公司的工作经历 —— 这些人应该都算是「专业选手」。

但是，从面试情况上看，绝大多数人都不具备机器人学的完整知识体系：画电路板的小伙伴不知道怎么进行机器人工作空间分析；设计机构的小伙伴不知道怎么把动力学用在控制上；做控制算法的小伙伴不知道什么的构型空间（Configuration Space）；做运动规划的小伙伴不知道什么是Q-learning；做深度强化学习的小伙伴不知道学习到的控制指令要怎么让实际机器人运动起来。

从我这几年的学习经历上看，我是能理解这一现象的。博士刚入学的时候，我接下了师兄的 SmartPal 机器人。靠着师兄的「祖传代码」，也曾狐假虎威地在外宾面前做过一些演示：

<p align="center">
  <img width="500" src="./Pics/SmartPalAndMe.jpg"/>
</p>

但是，当我后来真正开始看这些「祖传代码」的时候，我发现实际发给机器人的只有几个关节**位置**点而已。

**「PID 在哪里？？？」**

这是我当时产生的最大疑问。这个代码逻辑跟我本科玩得四旋翼、智能车等都完全不一样！

于是，拿着这个疑问，我在实验室问了一圈，没有得到答案。即使后来，我选修了好几门跟机器人相关的研究生课程。经过一年的学习，我还是没有得到答案。

是的，作为国内最早开展机器人研究的院校之一，这里的机器人研究生课程只教我们如何建立 DH 坐标系，动力学只是简单计算了一个平面三连杆。根本没有涉及控制、轨迹规划的内容，甚至连运动学逆解也没有要求大家计算。

据我所知，很多其他研究机构也是如此，机器人学这块还没有形成完整的教学体系。所以，基本上学生都没有接受过完整的机器人学系统教育，只有在做项目的时候通过自学掌握项目所需的内容。这也就造就了一大批没有算过机器人运动学逆解的机器人专业博硕士生。

当然，并不是说「运动学逆解」、「轨迹插补」之类的知识有多难。我想强调的是，在大陆，一个学生只通过上课，无法掌握、甚至是无法接触到这些机器人学中非常基础的知识。

当然，这一情况在大陆比较普遍，而国外或者港台高校毕业的学生，基本上都没有这个问题。国外或者港台高校在机器人学这块的教学体系相对比较完整，基本上大作业都会覆盖主要的知识点，并且大都要求编程实现。

虽然，大多数小伙伴都是「非科班出身」的，但是，根据我的经验，大陆的学生还是非常聪明的，基本只要得到一些简单的正确引导，就能很快通过自学掌握这些知识。所以，我们不妨来看看「非科班出身」如何学习机器人学吧。

## 二. 先修知识

当然，先修知识会随着研究深度的变换而不同，尤其是数学，数学就像是写轮眼，看同一个石碑，不同层次的「写轮眼」所看到的内容也完全不同。

<p align="center">
  <img width="800" src="./Pics/Sharingan.jpg"/>
</p>

但是，由于机器人学涉及面广，不同方向所需要的基础知识也完全不同，如果一开始就陷入「先修知识」的泥潭中，可能就得不偿失了。

所以，我认为，可以先列一些真正必须掌握的先修知识，其他的在后续相应部分提及即可：

1. **基本的英文**：在机器人方面，目前基本上没有非常合适的中文教材可以推荐。写得深入浅出的教材都是国外的，大家**必须**学会阅读英文文献。这个过程一开始肯定是痛苦的，但是，基本上坚持一个月就会习惯了。

2. **学会使用 VPN**。原因同上，基本上有用的资料都需要通过 Goolge 或 Youtube 获取。

3. **线性代数**：所有的空间变换、机器人相关计算都依赖于线性代数，甚至需要有一些基本的"线性空间"思维。对于线性代数，我首推 Prof. Gilbert Strang 的《Linear Algebra》，在 [Youtube](https://www.youtube.com/watch?v=hNDFwVVKVk0&list=PL221E2BBF13BECF6C) 和[网易公开课](http://open.163.com/special/opencourse/daishu.html)上可以找到视频。这门课一开始就引导大家从空间的角度看待问题，而不像国内高校，只要强调如何计算。而且，网易公开课上有中文字幕，对于初学者也还算友好。

4. **微积分**：机器人里，所有涉及到导数、积分、优化的地方，都会有微积分的影子。所以，这门数学课也是一开始就绕不开的。我没有太好的视频推荐，不妨也看看 Gilbert Strange 的[微积分重点](http://open.163.com/special/opencourse/weijifen.html)？

5. **理论力学**：机器人学就是每天与力打交道。但是一般机器人教材里都不会仔细推导空间变换、虚功原理、拉格朗日等力学理论，而且这些东西又相对抽象，很多初学者的自学过程就是被截杀在动力学章节的。当然，这部分我也没有太好的推荐资料，学堂在线上有清华高云峰老师的[理论力学](https://www.xuetangx.com/courses/TsinghuaX/20330334X/_/about)公开课，也可以参考一下。（但至少我当年上他的课总是犯困）。
   
6. **Matlab or Python**：这两个是非常容易上手，且非常方便数据可视化的编程语言。大家在学习机器人学的过程中，能非常容易地通过这类脚本语言实现一些算法，从而用于验证自己的推导结果。当然，这两部分只要掌握基本的矩阵操作和可视化操作就可以了。其他更高级的用法可以之后再学习。Coursera 上很容易找到这两门语言的入门课程 [Matlab](https://www.coursera.org/learn/matlab)、[Python](https://www.coursera.org/specializations/python)。

<p align="center">
  <img width="500" src="./Pics/IngenieriaElectrica.jpg"/>
</p>

7. **数字电路与模拟电路**：机器人是一门实践科学，只有当你把你推导的公式写成代码、并最终让实际机器人按照你的想法动起来的时候，才说明你掌握了相关知识。数电模电的知识可以让你对逻辑电路有个基本了解，不至于后面连为什么电机前面要加一个驱动器都不知道；同时，在身边没有实际机器人的情况下，自己搭个小电路做一些控制实验也是非常方便的。这块知识可以随便找本教材看看，例如我当时上的是唐庆玉老师的教材。

8. **一点点单片机**：要想制作简单的实验用控制电路，只有数电模电知识是不够的，还要能将这些知识转换成实际的电路，并且能将运行代码，那么就需要会单片机。对于单片机，可以网上随便买一些带伺服电机控制教程的最小系统板，学学 Arduino 或 STM32，当然，如果能参加个 RoboMaster 或者飞思卡尔智能车大赛什么的是最好了，可以对嵌入式的各个模块有个基本了解。

9. **Linux 和 C 语言**：现在有了电路部分，我们需要将公式代码变成电路指令，这就涉及嵌入式的编程了。这块建议学一点 C 语言。嵌入式对 C 的要求其实并不高，随便学点语法就够了，例如[C语言入门](https://akaedu.github.io/book/pt01.html)。但是，如果未来想做一些更加上层的工作，最好一开始就把 C 学好。学编程，Linux 是个不错的选择，所以，这时候，可以尝试按照个 Linux 系统，在上面学习 C 语言。

10. **基本的3D设计**：在制作实验平台的时候，经常会遇到需要加工设计小零件的情况，这时候掌握一个3D设计软件可以大大提高开发速度，例如 SolidWorks 就是个不错的选择。配合上 3D 打印机之类的工具就可以实验快速原型设计了。（即使没有 3D 打印机，在网上也可以很容易找到 3D 打印服务，把你设计的 3D 文件发过去就可以了）。

上面这些知识，基本是一个自动化专业或者机电专业大三学生应该达到的水平。如果对上述几部分有了基本了解，就可以开始看机器人学的知识了。

## 三. 入门

对于入门部分，实际上就是了解如何让一个工业机器人动起来。这方面其实研究已经非常成熟了，大家看上个世纪的教材就行，个人推荐的是 John Craig 的教材 《Introduction to Robotics: Mechanics and Control》<sup>[1]</sup>，在 [Youtube](https://www.youtube.com/watch?v=0yD3uBshJB0&list=PL64324A3B147B5578) 和[网易公开课](http://open.163.com/special/opencourse/robotics.html)都可以找到斯坦福 Oussama Khatib 大神的视频，基本与 Craig 的教材内容相匹配。

<p align="center">
  <img width="300" src="./Pics/CraigBook.jpg"/>
</p>

建议从 Craig 的教材开始就看英文版本，Google 一下，很容易找到 PDF 版本。作为一本入门教材，Craig 的教材是相当深入浅出的，配合着 Khatib 的视频，可以快速掌握机器人学的基础。

我常对刚入学的师弟们说，「如果你把这本书的内容掌握了，就已经超过实验室绝大多数师兄师姐了。」

然而，真正把教材啃下来的并不多。

所以，我在这里要换个说法了，「如果你把这本书的内容掌握了，就可以胜任国内绝大多数机器人公司的开发工作了。」

这里，我会大概把基础的知识列一下，时间有限，暂时不会过多展开。顺序可能不完全与 Craig 教材相同。

### 3.1 空间变换

对于这部分内容，如果理论力学学得好的小伙伴，基本是没有太大问题的。问题是，有些小伙伴没有学好。

当然，其中齐次变换什么的是机器人学中非常基础和重要的内容。其中需要注意的地方有：

- 熟悉坐标表示方式：坐标系 {B} 在坐标系 {A} 下的位姿为 ${^A_B}T$ 等
- 左乘与右乘的区别
- 了解旋转矩阵每一列的含义，学会如何通过「目测」写出两个坐标系之间的旋转矩阵；
- 姿态的表示方式：RPY 角、各种欧拉角、轴角（Angle-Axis）表示、旋转矩阵，除了书上的内容，可以顺便看看四元数（Quaternion）表示和欧拉角的 Gimbal lock （知道三参数表示的问题，才能更容易接受四元数这样的新事物）。
- 如果有可能，试着了解一下角速度。

### 3.2 运动学

<p align="center">
  <img width="600" src="./Pics/RobotKinematics.jpg"/>
</p>

对于机器人来说，一个基本工作就是计算运动学：

- 正运动学：根据关节角度，计算机器人工具坐标系（末端）在机器人基座坐标系（底座）下的位姿；
- 逆运动学：给定一个末端位姿，计算达到这个位姿的关节角度。

前面，你知道了可以用一个 4x4 矩阵来描述两个坐标系之间的关系。对于机器人正运动学，如果我们知道每个连杆两两之间的坐标变换，就可以通过矩阵乘法计算出最后的末端位姿了。

为了方便计算两个连杆之间的相对位姿，你就需要学习一个叫做 DH 的建模方法，简而言之，就是按照一定规则建立每个关节坐标系，然后每个坐标系可以用四个参数（DH参数）来确定。

当然，你网上一搜，就会发现 DH 也有好几种，什么 Standard DH， Modified DH 之类的。

这不重要，你只要知道它是帮你确定两个连杆之间的相对关系就行。不妨掌握 Craig 书上的那种就行（Wikipedia上称为 Modified DH)：

<p align="center">
  <img width="500" src="./Pics/ModifiedDH.jpg"/>
</p>

1）建立坐标系：

- $z_i$ 轴与第 $i$ 个关节重合，关节转动方向遵照右手定律；

- $x_i$ 平行于 $z_i$ 和 $z_{i+1}$ 的公垂线：$x_i = z_i \times z_{i+1}$。如果两 $z$ 轴平行，则让 $x_i$ 从 $z_{i}$ 指向 $z_{i+1}$；

- 有了 $x$、 $z$ 轴后，就可以用右手定律定义 $y$ 轴方向。

- 除了与每个关节固连的坐标系外，还有可能额外在机器人基座 {B} 与末端工具 {E} 上固连两个坐标系。

2）计算 DH 参数:

- $a_i$ 是沿着 $x_i$，从 $z_i$ 到 $z_{i+1}$ 的距离；

- $\alpha_i$ 是绕着 $x_i$，从 $z_i$ 到 $z_{i+1}$ 的角度；

- $d_i$ 是沿着 $z_i$，从 $x_{i-1}$ 到 $x_i$ 的距离；

- $\theta_i$ 是绕着 $z_i$，从 $x_{i-1}$ 到 $x_i$ 的角度。

3）计算变换矩阵：

$${^i_{i-1}}T = Rot(x_{i-1}, \alpha_{i-1}) \cdot Trans(x_{i-1},a_i) \cdot Rot(z_i, \theta_i) \cdot Trans(z_i, d_i)$$

<!-- $${_{i-1}^{i}}T = \begin{bmatrix} cos(\theta_i) & -sin(\theta_i) & 0 & a_{i-1} \\ sin(\theta_i)cos(\alpha_{i-1}) & cos(\theta_i)cos(\alpha_{i-1}) & -sin(\alpha_{i-1}) & -d_i sin(\alpha_{i-1}) \\ sin(\theta_i)sin(\alpha_{i-1}) & cos(\theta_i)sin(\alpha_{i-1}) & cos(\alpha_{i-1}) & d_i cos(\alpha_{i-1}) \\ 0 & 0 & 0 & 1 \end{bmatrix}$$ -->

<p align="center">
  <img width="600" src="./Pics/Equations/DHMatrix.gif"/>
</p>

<a href="https://www.codecogs.com/eqnedit.php?latex=\bg_white&space;\huge&space;{^i_{i-1}}T&space;=&space;\begin{bmatrix}&space;cos(\theta_i)&space;&&space;-sin(\theta_i)&space;&&space;0&space;&&space;a_{i-1}&space;\\&space;sin(\theta_i)cos(\alpha_{i-1})&space;&&space;cos(\theta_i)cos(\alpha_{i-1})&space;&&space;-sin(\alpha_{i-1})&space;&&space;-d_i&space;sin(\alpha_{i-1})&space;\\&space;sin(\theta_i)sin(\alpha_{i-1})&space;&&space;cos(\theta_i)sin(\alpha_{i-1})&space;&&space;cos(\alpha_{i-1})&space;&&space;d_i&space;cos(\alpha_{i-1})&space;\\&space;0&space;&&space;0&space;&&space;0&space;&&space;1&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\bg_white&space;\huge&space;{^i_{i-1}}T&space;=&space;\begin{bmatrix}&space;cos(\theta_i)&space;&&space;-sin(\theta_i)&space;&&space;0&space;&&space;a_{i-1}&space;\\&space;sin(\theta_i)cos(\alpha_{i-1})&space;&&space;cos(\theta_i)cos(\alpha_{i-1})&space;&&space;-sin(\alpha_{i-1})&space;&&space;-d_i&space;sin(\alpha_{i-1})&space;\\&space;sin(\theta_i)sin(\alpha_{i-1})&space;&&space;cos(\theta_i)sin(\alpha_{i-1})&space;&&space;cos(\alpha_{i-1})&space;&&space;d_i&space;cos(\alpha_{i-1})&space;\\&space;0&space;&&space;0&space;&&space;0&space;&&space;1&space;\end{bmatrix}" title="\huge {^i_{i-1}}T = \begin{bmatrix} cos(\theta_i) & -sin(\theta_i) & 0 & a_{i-1} \\ sin(\theta_i)cos(\alpha_{i-1}) & cos(\theta_i)cos(\alpha_{i-1}) & -sin(\alpha_{i-1}) & -d_i sin(\alpha_{i-1}) \\ sin(\theta_i)sin(\alpha_{i-1}) & cos(\theta_i)sin(\alpha_{i-1}) & cos(\alpha_{i-1}) & d_i cos(\alpha_{i-1}) \\ 0 & 0 & 0 & 1 \end{bmatrix}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\huge&space;{^i_{i-1}}T&space;=&space;\begin{bmatrix}&space;cos(\theta_i)&space;&&space;-sin(\theta_i)&space;&&space;0&space;&&space;a_{i-1}&space;\\&space;sin(\theta_i)cos(\alpha_{i-1})&space;&&space;cos(\theta_i)cos(\alpha_{i-1})&space;&&space;-sin(\alpha_{i-1})&space;&&space;-d_i&space;sin(\alpha_{i-1})&space;\\&space;sin(\theta_i)sin(\alpha_{i-1})&space;&&space;cos(\theta_i)sin(\alpha_{i-1})&space;&&space;cos(\alpha_{i-1})&space;&&space;d_i&space;cos(\alpha_{i-1})&space;\\&space;0&space;&&space;0&space;&&space;0&space;&&space;1&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\huge&space;{^i_{i-1}}T&space;=&space;\begin{bmatrix}&space;cos(\theta_i)&space;&&space;-sin(\theta_i)&space;&&space;0&space;&&space;a_{i-1}&space;\\&space;sin(\theta_i)cos(\alpha_{i-1})&space;&&space;cos(\theta_i)cos(\alpha_{i-1})&space;&&space;-sin(\alpha_{i-1})&space;&&space;-d_i&space;sin(\alpha_{i-1})&space;\\&space;sin(\theta_i)sin(\alpha_{i-1})&space;&&space;cos(\theta_i)sin(\alpha_{i-1})&space;&&space;cos(\alpha_{i-1})&space;&&space;d_i&space;cos(\alpha_{i-1})&space;\\&space;0&space;&&space;0&space;&&space;0&space;&&space;1&space;\end{bmatrix}" title="\huge {^i_{i-1}}T = \begin{bmatrix} cos(\theta_i) & -sin(\theta_i) & 0 & a_{i-1} \\ sin(\theta_i)cos(\alpha_{i-1}) & cos(\theta_i)cos(\alpha_{i-1}) & -sin(\alpha_{i-1}) & -d_i sin(\alpha_{i-1}) \\ sin(\theta_i)sin(\alpha_{i-1}) & cos(\theta_i)sin(\alpha_{i-1}) & cos(\alpha_{i-1}) & d_i cos(\alpha_{i-1}) \\ 0 & 0 & 0 & 1 \end{bmatrix}" /></a>

4）正解：

$${^b_e}{T}={^b_1}T\cdot{^1_2}T\cdot{...}\cdot{^n_e}T$$

5）逆解：

就是通过不断调整上面几个矩阵的位置，尝试找到可以单独求解的未知数即可。虽然有些繁琐，但是各位初学者一定要正在亲手推一便六轴机械臂的运动学逆解公式，并**编程实现**。

### 3.3 雅可比矩阵

雅可比矩阵 $J$ 是机器人学中一个非常重要的东西。它表示机器人关节速度 $\dot{q}$ 到末端速度 $\dot{x}$ 之间的关系:

$\dot{x}=J\cdot \dot{q}$

- 如果你前面没有弄清楚角速度，建议在这章仔细思考。例如，「为什么不能直接对欧拉角求导获得速度？」；

- 了解教材中的雅可比计算方式，并思考「直接为什么不直接对运动学正解的结果求偏导？」

- 这边需要掌握的就是它的计算方法了，**一定要**编程计算机器人的雅可比矩阵；

- 如果你用 Matlab 或 Python，你可以利用他们的符号运算工具来验证我上面几个问题。从而加深对角速度的理解。

- （PS：姿态和角速度无法直观理解很正常，因为它们不是在笛卡尔空间内，等后面学到更多数学，你们才能真正理解它。）；
  
- 如果你了解虚功原理，那么你又会知道雅可比也可以表示末端力与关节力矩的关系（这在以后力控等方面很有用）。

这时候，你有了雅可比矩阵，你就会发现，你知道怎么通过调节角度来控制末端运动了。这时候我们再回头看运动学逆解的问题。你会发现：「让机器人末端朝着目标位姿运动不就可以了？」。

<p align="center">
  <img width="500" src="./Pics/JacobianIK.jpg"/>
</p>

是的，这就是机器人运动学的数值计算方式，你可以利用这个方法写一个机器人运动学的通用求解算法。具体可以看我在知乎上的回答 [MATLAB机器人工具箱中机器人逆解是如何求出来的](https://www.zhihu.com/question/41673569/answer/129670927)。

各位初学者**务必**亲手实现一遍这种算法，还有有些坑需要踩的。

当然，这个方法很简洁，但是也有它本身的问题：

- 计算速度慢，需要多次迭代；
  
- 一次只能返回一组解；
  
- 可能会遇到奇异点等，结果无法收敛。

这时候，你可以顺便去了解一些奇异（Singularity）的问题，理解奇异性是机器人构形相关的属性，无法通过建模方式来消除。

### 3.4 动力学

我相信，80% 的小伙伴是在这一章放弃的。

<p align="center">
  <img width="500" src="./Pics/NewtonEulerDynamics.jpg"/>
</p>

对于多轴机器人的动力学，不论是采用牛顿欧拉还是拉格朗日法，都会**显得**异常复杂。再加上如果之前没学好理论力学，那么基本上是举步维艰了。

所以，我个人认为，先对这个部分有个基本概念就行，暂时不需要直接去碰六轴机器人的动力学：

- 会用拉格朗日法计算三轴机械臂的动力学模型（三轴的求解还是在可接受范围内的）；

- 用牛顿欧拉法计算三轴机械臂的动力学模型，一定要**编程实现**（因为在高自由度情况下，牛顿法更容易通过编程实现，未来如果要做动力学，更可能是用牛顿欧拉、而非拉格朗日）；

- 了解转动惯量之类的物理意义，（在上面编程实现过程中，肯定会有相应的问题发生，如角速度与转动惯量的参考坐标系问题）；

- 大概知道机器人动力学都包含哪些部分（公式的形式、连杆动力学、关节动力学、重力影响、关节摩擦力、电机动力学等）。

### 3.5 控制

这时候，我们有了各种工具来机器人机器人的运动学问题了，我们知道要让机器人到达任意状态的关节角度值。但是，如何让这些关节动起来？

首先，我们要知道，日常生活中的世界还是受牛顿力学统治的。

$F = m \cdot a$

要让一个东西动起来，就要给它施力。

<p align="center">
  <img width="800" src="./Pics/Slider.jpg"/>
</p>

如果我们给定一个滑块的运动轨迹 $s(t)$，我们就可以计算出它整个轨迹的加速度 $\ddot{s}(t)$，进而计算出让滑块按照我们设想运动所需的力 $F(t) = m \cdot \ddot{s}(t)$。

换句话说，我们可以通过动力学计算出让机器人运动所需的每个关节力矩。

<p align="center">
  <img width="500" src="./Pics/DCMotor.jpg"/>
</p>

而关节力矩，可以通过电机提供，对于直流电机，输出力矩与电流成正比。

但是，有几个问题：

- 动力学好难算；

- 动力学参数好不准（转动惯量不好测、关节摩擦力不好算）；

- 还可能有各种外力（抓持的物体，关节动力学属性变化等）。

于是，这就是控制算法的工作了。于是，大家会接触到传说的 PID 控制。

但是，又有问题：如果我们直接把关节目标位置发给 PID 控制器，那么每次都是一次阶跃响应。

在这里，建议有条件的小伙伴用单片机弄一个单轴伺服控制系统，有伺服电机（你可以控制电流、力矩、或者 PWM 占空比；步进电机、舵机就算了）、有编码器（可以反馈电机的角度）、有驱动器（能将数字指令转换成电机控制信号）、有控制器（STM32等单片机，可以给驱动器提供控制指令）。顺便可以了解一些通讯、中断、实时性的内容。

<p align="center">
  <img width="500" src="./Pics/StepAndTraj.jpg"/>
</p>

但是，感觉好像还是有什么不对，机器人运动好像是有加减速过程（右）的，而非一次阶跃（左）。

<p align="center">
  <img width="500" src="./Pics/TrajPlanning.jpg"/>
</p>

这就是轨迹规划（Trajectory Planning），给定一些轨迹点，利用不同的函数来拟合这些轨迹。

这时候你又想到，既然 PID 和动力学都可以计算让机器人运动所需的力，只是动力学稍微有一些不准，那么有没有可能把它们结合在一起，先用动力学算一个基本准确的力矩，然后用 PID 消除不准确性造成的微小误差？

<p align="center">
  <img width="500" src="./Pics/FeedforwardPID.jpg"/>
</p>

是的，于是你发现了基于动力学前馈的 PID 控制算法。

Craig 书上剩下的其他一些部分，可以大概浏览一下，因为有不少内容已经比较旧了。


## 勇者斗恶龙

🐀🐂🐅🐇🐉🐍🐎🐏🐒🐓🐕🐖


$S = 2\cdot \pi \cdot r^2$

## 参考文献
[1] John J. Craig. Introduction to Robotics: Mechanics and Control[M]. 1986.



「」