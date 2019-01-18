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
