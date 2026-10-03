由于笔记本CPU设计的冗余电压给的很大，再加上出厂即灰烬，散热技术也没有新突破，三管齐下就轻松顶着90度跑了  
想要可观降低发热，就需要把达到边际效应的频率给取舍掉  
也就是损失性能上限换更低温度和更稳定的供电分配  
如果你的笔记本不支持频率调节或者限制功率，或者不想用工具限制，就可以用这套操作  
如果你想，台式也可以用  

## CPU降频命令
```
powercfg -SetAcValueIndex Scheme_Current Sub_Processor ProcFreqMax 4000
powercfg -SetAcValueIndex Scheme_Current Sub_Processor ProcFreqMax1 4000
powercfg -SetAcValueIndex Scheme_Current Sub_Processor ProcFreqMax2 4000
```

| CPU大小核类型 | ProcFreqMax | ProcFreqMax1 | ProcFreqMax2 |
| :------------ | :---------- | :----------- | :----------- |
| P             | P核频率     | 无作用       | 无作用       |
| P+E           | E核频率     | P核频率      | 无作用       |
| P+E+LPE       | LPE核频率   | E核频率      | P核频率      |

> [!NOTE]  
> `SetAcValueIndex`修改的是AC状态，如果要修改DC，就改成`SetDcValueIndex`  
> 后面的`4000`就是频率上限，单位是`MHz`  
> 每个人的CPU的电压频率曲线都不同，通用技巧是摸索出一个满载时电压在`1.1V~1.2V`的频率  

> [!IMPORTANT]  
> 需要管理员权限运行  

> [!IMPORTANT]  
> 需要注意OEM覆盖电源计划  

> [!NOTE]  
> 运行一次即整个电源计划生效，不需要加入开机自启  
> 除非切换电源计划，才需要给新电源计划重新运行一次  

## 重置CPU降频命令
```
powercfg -SetAcValueIndex Scheme_Current Sub_Processor ProcFreqMax 0
powercfg -SetAcValueIndex Scheme_Current Sub_Processor ProcFreqMax1 0
powercfg -SetAcValueIndex Scheme_Current Sub_Processor ProcFreqMax2 0
```

## NVIDIA GPU降频命令
```
nvidia-smi -lgc 0,1500
```

> [!NOTE]  
> 第1个数字是最低频率，第2个是上限频率（实际受出厂频率和温控影响）  
> 还是同一个套路，按照能接受的温度范围选择  

> [!IMPORTANT]  
> 需要管理员权限运行  

> [!NOTE]  
> 这个修改仅够维持本次开机，严格来说只能维持本次驱动会话，驱动状态更改后就被重置  
> 如果要永久生效，需要加入开机自启  

## 重置NVIDIA GPU降频命令
```
nvidia-smi -rgc
```

## 额外事项
- 影响温度的不是只有频率，也有硅脂老化的情况，如果压频率无效，就该考虑换硅脂了  
- 还有比较罕见的情况是静电破坏了NVRAM的有关硬件传感器的数据，导致一系列供电和节流控制不正常  
  如果出现这种情况，就需要完全放电才能解决  
