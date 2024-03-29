# Kicad设计规范

## PCB版本和日期丝印使用变量

版本的变量是：`${REVISION}`

日期的变量是：`${ISSUE_DATE}`

如PCB的丝印设置如下：

![image](image/kicad-design-spec-1.png)

则对应变量会被实际的页面设置值替代：

![image](image/kicad-design-spec-2.png)

显示的效果如下：

![image](image/kicad-design-spec-3.png)

## 设计使用英制单位mil

常见的对应关系如下：

| 英制 | 公制 |
| ----------- | ----------- |
| 10mil | 0.254mm |
| 20mil | 0.508mm |
| 30mil | 0.762mm |
| 40mil | 1.016mm |
| 100mil | 2.54mm |

一般密度的板子信号线使用8mil，电源线根据走电流的大小设置，一般原则大于信号线的宽度。

可以使用如下Kicad的计算工具计算：

![image](image/kicad-design-spec-4.png)

得到的结果向上取整，如上可以设置为15mil。一般取值是10mil、15mil、20mil等5的倍数。

过孔一般使用24/12mil，即外径24mil，内孔直径12mil。