基于Unity-Vuforia的AR光学实验Demo
=====================================
成员及分工
* 安焜豪 PB18020738 ：应用编写
* 黄志成 PB18151815 ：AR算法 

工程描述
-----------
从小学到大学，实验一直是科学教育的重要组成部分。然而由于经济原因，(至少在我老家)绝大部分中小学没有实验设备，对学生来说实验这一概念十分遥远。将脆弱、维护麻烦的实验设备虚拟化并部署到教育体系中，将有助于实验教育的广泛开展，激发中小学生的学习兴趣。

因而，我选择AR设计课题，用数张打印纸marker及已经普及的智能手机，通过自由摆放不同marker＂召唤＂不同的光学器件搭建光路，实现一些光学实验。目前只做了光的反射折射部分。

这一项目可以分解为两个部分，一个为marker识别及位置、姿态判定的AR部分，一个为AR物体之间互动的物理模拟部分。Unity引擎具有一套成熟方便的物理环境与对象交互体系，而且具有大量现成的扩展插件(包括AR部分使用的高通VuforiaAR插件)及方便强大的跨平台部署能力，故选择在这一平台上搭建项目

原理分析
----------------
AR部分:

AR部分需要识别marker并确定其坐标与朝向

物理部分:

获取marker的坐标与朝向后，在其上生成相应的光学器件，在Unity-Vuforia中，只需要将相应器件下挂作为marker的子物体即可。通过按钮向光源下达开火指令，根据光源位置朝向生成probe，probe运动生成光线，同时检测与其他光学器件的碰撞。其他光学器件将根据代码还原的光学定律改变probe的参数，如运动方向、速度。

代码实现
------------------
AR部分各脚本及其内容:
27
* orb.py 使用ORB特征检测器和描述符，计算关键点和描述符,利用暴力匹配BFMatcher，遍历描述符，确定描述符是否匹配，然后计算匹配距离并排序，显示最终匹配结果
* FLANN.py 使用sift算法检测角点，并利用FLANN匹配器和KNN算法实现特征匹配，去除错误匹配后显示最终结果

物理部分各脚本及其内容
* Fire.cs 下挂于开火按钮，负责命令光源发射probe(目前是遍历所有光源)，并允许在Unity编辑器中调节光波长等参数
* BulletFly.cs 下挂于probe，负责储存该光线参数，如速度、方向、波长、当前介质折射率，及未来可能的相位信息(未来这些信息可能会迁移到新建的BulletProperties)。此外probe运动的位置信息也在此每帧更新。
* BulletLifeTime.cs 下挂于probe，负责一定时间后销毁probe
* BulletReflect.cs 下挂于probe，负责检测与反射面的碰撞，并对probe施加反射现象带来的速度方向变化
* BulletRefraction.cs 下挂于probe，负责检测进出折射介质，并对probe施加折射与全反射现象带来的速度大小和方向变化
* RefractionIndexData.cs 下挂于折射体，负责储存调用不同的折射率-波长函数

效果展示
------------------
![image](https://github.com/USTC-Computer-Vision-2021/project-physicschicken/blob/main/readmeResources/result.gif)

工程结构
------------------
主目录
* Asset.zip-Unity工程文件的资源部分，删去了占地过大的vuforia源码部分，unity版本2021.2.5f1c1,vuforia10-3-2

* AROpticFin.apk-Demo的apk安装包

* ARproduction算法复现及简易结果

* markers-marker图像文件夹

* readmeResource-readme文件中图片保存目录

* readme.md
