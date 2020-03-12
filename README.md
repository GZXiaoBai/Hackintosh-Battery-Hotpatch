# Hackintosh-Battery-Hotpatch
Hotpatch for some computer‘s Battery
## 说明

这里面我会收集各种电池热补丁以方便大家取用！
其中有包括从网上收集来的，以及自己制作的补丁。

## 鸣谢

特别感谢以下大佬：

**iStar丶Forever**

**宪武大大**

**Bat.Bat**

## 热补丁需要更名，才能生效
这里面的更名我并没有逐个给出，因此需要大家自行更名。

**更名方法如下**
1、利用Hackintool计算Method名称对应的十六进制代码
2、打开maciASl，确认需要更名的Method的参数个数和是否可序列化

- Method(xxxx,a,N) = xxxx的十六进制代码 + a的十六进制代码
- Method(xxxx,b,S) = xxxx的十六进制代码 + (b+8)的十六进制代码
- 示例：Method(MHPF,1,N) = `4D485046 01` , Method(BTST,2,S) = `42545354 0A`
- 将该方法更名为一个未被利用的名称，通常习惯以X替换第一个字母，只要不出现重复定义即可
- 利用利用Hackintool计算XHPF的十六进制代码，最终config的更名应填
  - Comment     change MHPF,1,N to XHPF
  - Find        42545354 0A
  - Replace     58545354 0A
3、同时你的更名需要确保具有唯一性，不能有重复及相同的出现

## 检查Mutex是否置0
如果出现电量显示0%的情况，你需要检查Mutex，确保他们全部为0
在当前使用的DSDT文件里搜索Mutex，看出现的几个变量是否为0，目前所知道的绝大多数机器的ACPI Mutex都是默认置0的，但是对于一些联想机器，它们往往有几个Mutex初始值并不是0，这里我们利用ACPI二进制更名的方法实现置0

以Mutex (BATM, 0x07)为例，先转换BATM为十六进制代码，得到 `42 41 54 4D`

在前后加上完整定义的十六进制代码，01代表Mutex，07则代表默认值，最终得到 `01 42 41 54 4D 07`

我们的目的是使Mutex对象置0，所以config的更名应填 

- Comment  Set Mutex BATM, 0x07 to 0x0
- Find     01 42 41 54 4D 07
- Repalce  01 42 41 54 4D 00

其它Mutex对象按照同样的方法处理即可
