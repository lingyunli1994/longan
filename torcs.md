  # TORCS快捷键
  1：左上角显示时间等信息 <br>
  2：显示仪表盘 <br>
  3：左下角显示 <br>
  4：右下角显示转向信息 <br>
  0：切换仪表盘等显示方式 <br>
  F2：显示出车身（近景） <br>
  F3：显示出车身（远景） <br>
  F4：显示侧面场景 <br>
  F5：显示高空俯视图 <br>
  F7：显示出全局图 <br>
  F8：显示车来的方向的场景 <br>
  
  # DDPG玩TORCS问题 <br>
  * Q:在赛道1训练了一个模型，而且训练效果很好，但当改变赛道时，训练好的模型不起作用，赛车会跑出赛道，这是不是意味着对于每一个不同的赛道，我都需要训练不同的模型呢？<br>
  A：并不需要再训练，可能是你在赛道上过度使用了模型，建议在更困难的赛道上训练，在简单赛道上测试，将L2正则化应用到神经网络上保证不过度使用。<br>
  * Q:怎么改变赛道？<br>
    A:sudo torcs → click Race → click Practice → click Configure Race → Choose differtent track → click new Race <br>
  必须按ctrl+C才有效。
  * DeepMind团队在深度强化学习论文实验中通常会忽略车身和仪表盘，所以在实验中删除了这些。<br>
    恢复车身的办法：
     第280行，grscreen.cpp
     for(i=1;i<s->_ncars;i++){...}，将i=1改成i=0；<br>
    恢复后视镜：grscreen.cpp中第361-366行恢复注释；<br>
    恢复仪表盘：grboard.cpp中第841行-845行恢复注释。<br>
   编译：代码更改后，必须编译才有效<br>
    在vtorcs-RL-color下：①.make;②./configure;③sudo make install;④sudo make datainstall
  * 如何改变训练的环境，而不只是简单地改变赛道？目前是在"practice"环境下实验，如何添加多车训练，或者用"quick race"环境训练？<br>
    change 'autostart.sh' to <br>
     #!/bin/bash <br>
     xte 'key Return' <br>
     xte 'usleep 100000' <br>
     xte 'key Return' <br>
     xte 'usleep 100000' <br>
     xte 'key Return' <br>
     xte 'usleep 100000' <br>
     xte 'key Return' <br>
  * 如果你将vision写成False，则需要在训练中禁用了GUI模式，如果只改变vision变量，则不起作用。一种在训练时将TORCS切换到文本模式的方法：
  修改gym_torcs.py中第33行，os.system('torcs -T -nofuel -nolaptime &')。
  这里我们为torcs命令添加一个新选项"-T"以启动文本模式。
  * 如果想重新训练，应该怎么做？<br>
    删除.h5的文件。
  * 如果想将视觉作为输入，怎么改？<br>
       Observation = col.namedtuple('Observaion', names)  <br>
             # Get RGB from observation   <br>
            image_rgb = self.obs_vision_to_image_rgb(raw_obs[names[8]])  <br>
            image_rgb = self.obs_vision_to_image_rgb(raw_obs[names['img']])  <br>
  您是否将TORCS更改为64x64图像尺寸？ gym_torcs只支持64x64像素模型。 <br>
  图像从0.0到1.0归一化，使用RGB图像  <br>
 我没有执行4次并收集图像，而是保留最后3张图像并连接最新的图像以形成输出的观察结果。（所以尺寸是646412）<br>
 在插入全连接层之前，我使用三个带有ReLu的2D卷积层。 这样做会大大减少状态的维度并使训练可行。此外，每个卷积层后面是批量标准化图层。<br>
对于env E-Road，Agent将在200~300集左右开始执行合理的策略。  <br>
  
