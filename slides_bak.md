---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /cover.png
# some information about your slides, markdown enabled
title: 视觉循迹遥控智能小车
info: 
# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
---

# 视觉循迹遥控智能小车

基于stm32F103C8T6核心板的综合课程设计{.font-size-6}

<div class="pt-10">曾道斌 徐俊 梁文涛 庞喆</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# 设计背景

<br>
&emsp;&emsp;在现代社会中，智能小车的应用越来越广泛，从工业自动化到智能家居，都需要具备自主运行和避免障碍物的能力。视觉巡迹避障遥控小车是一种基于图像处理和传感技术的智能小车，它可以自动识别路线、避开障碍物，并沿特定轨迹行驶
<br>
<br>

&emsp;&emsp;从本质上来讲，智能小车就是一个可以自主移动的机器人。它可在硬件电路和内部程序共同作用下，实现环境感知、自主规划、自主决策、自动驾驶等功能，可满足不同行业的智能化需求。智能小车是在无人驾驶车辆的基础上增添一些与其工作性质相关的智能化模块，从而使车辆达到智能自动化的目的。结合智能小车的特点、功能和开发意义，研究智能小车是很有必要的。

<!--
Here is another comment.
-->

---
transition: slide-up
---

# 项目目标

<br />

<n-gradient-text>
本课程设计旨在开发一款具备以下功能的智能小车
</n-gradient-text>

<div class="grid grid-cols-2 gap-4">
<div>

  <div class="text-sm">

  - 循迹功能：能够识别白色智能车赛道轨迹并沿着轨迹行驶。
  - 避障功能：能够检测前方障碍物并自动避开，确保安全行驶。
  - 遥控功能：能够蓝牙连接手机，来进行远程遥控前进后退，启停等。
  - 显示功能：利用OLED模块显示小车的运动参数，辅助调参
  - 导航功能：利用陀螺仪和编码器计算小车坐标以实现定点移动和智能回库功能等

  </div>

  <br />
  <br />

  **总的来说就是初阶的自制版！**

</div>

  
  <div>
  <img class="img" src="/IMG_20240319_165053.png"></img>
 </div>
</div>


<br>

---

# 方案设计

<br />
<n-gradient-text>
核心方案：通过摄像头采集图像，通过图像处理识别白色赛道轨迹,检测前方障碍物，通过蓝牙模块实现远程遥控。
</n-gradient-text>
<br />
<br />
<div class="columns-2">
<div class="text-sm">

- **软件设计**
<li>采用HAL库进行硬件驱动</li>
<li>采用OpenCV进行图像处理</li>
<li>采用PID闭环控制算法进行控制</li>
<li>采用Android Studio进行蓝牙遥控APP开发</li>
<li>采用Keil进行主控程序开发</li>
<li>采用Proteus进行电路仿真</li>

</div>
<div class="text-sm">

- **硬件设计**
<li>采用STM32F103C8T6核心板作为主控制器</li>
<li>采用TB6612FNG电机驱动模块驱动两个520直流电机</li>
<li>采用蓝牙模块实现蓝牙遥控</li>
<li>购买小车车模<li>包含两个直流电机，四个轮子，以及的是ds3120转向舵机</li></li>
<li>采用3S电池为主电源，并设计电路分别为系统各部分供电</li>
  

</div>
</div>
---

# 方案设计

<br />
<n-gradient-text>
系统框图
</n-gradient-text>

<br />
<br />

<div class="columns-2">

<div class="w-110 h-110">
<img class="img" src="/arch2.png"></img>
</div>

<div class="w-100 h-100">
  <img class="img" src="/IMG_20240319_141410.png"></img>
</div>

</div>

---

# 预算


|项目|价格/元|
|---|---|
|车架|30|
|编码器电机x2|50|
|轮胎x4|40|
|舵机|70|
|其他零件以及芯片若干|80|
|电池|70|
|核心板|20|

**总计360元**

<br />
---
