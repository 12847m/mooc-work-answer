# mooc-work-answer

[![stars](https://img.shields.io/github/stars/11273/mooc-work-answer)](https://github.com/11273/mooc-work-answer)
[![forks](https://img.shields.io/github/forks/11273/mooc-work-answer)](https://github.com/11273/mooc-work-answer)
[![issues](https://img.shields.io/github/issues/11273/mooc-work-answer?style=plastic)](https://github.com/11273/mooc-work-answer/issues)

+ __智慧职教 【考试+测验+作业+刷课】__

+ __仅适用于: <https://mooc.icve.com.cn/>__

+ __详细刷课技术参考__ [技术篇](https://www.52pojie.cn/thread-1338063-1-1.html)

 ---

## 已知问题

+ 不要开强制提交，目前填空题没有自动填入答案

## 更新状态

+ 2022/4/2: 移除自动识别验证码（识别验证码的库太大不好装，减少运行成本）

+ 2022/4/3: 代码优化，效率提高，配置项启用

## 实现功能

| 功能  | 介绍      | 完成  |
| :---: | --------- | :---: |
| 刷课  | 速刷模式  |   ✅   |
| 测验  | 实现100分 |   ✅   |
| 作业  | 实现100分 |   ✅   |
| 考试  | 实现100分 |   ✅   |

## 运行环境

+ python3
+ 运行所需 pip 包请自行切换到本项目根目录使用以下命令进行安装

  ```pip
   pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt
  ```

1. 需要两个账号（仅刷课可以只需要一个账号，不需要小号）
   + 一个是你需要刷课的账号，一般你自己都有，
   + 另一个需要你注册一个小号(<https://www.icve.com.cn/portal/register/register.html>) 随便注册一个，然后运行
2. 目前可能部分作业类型没有适配到，如出现异常请提交详细截图以及课程名称(截图最好)

## 技术简述

功能部分都是调用mooc的api模拟行为，当前已连续测试课程数38，测试均成功

### 1. 刷课

+ 调用api，类似于模拟浏览器行为，正常刷课，但是有api可以直接设置看课件的时长，一级一级的打开目录找到课件，然后直接用课件id去设置时长就可以达到秒刷，不像 [职教云](https://zjy2.icve.com.cn/) 秒刷会检测到，mooc相对老实一点，但也需要控制速度，慢一点就行
+ __时长问题__：没有和课件时长一致，会多加一点随机时长，当然也可以加几千个小时
  
### 2. 测验、作业、考试

+ 统称为 __答题__，因为只是作业类型不一样，题目、答案 都是一致的
+ 什么原理?
  >
  > + 不考虑其他因素（这里不讲获取答题的题目，无非就是一层一层调用api拿到所有作业ID，再往下就能获取到当前作业的所有题目以及选项）
  > + 例如单选题，题目有一个ID，其四个选项A B C D，各自对应一个值（0，1，2，3），这个时候就需要拿到一个正常题目ID，以及答案，然后匹配到题目ID，再通过模拟浏览器行为发起请求选上填入值，就完成了一个题目的作答
  > + 以此循环所有的题目，包括单选多选简答等，依次发请求就可以，而且你作答一个题目其实后台都会保存你的选项，这时候就不用担心有些突发情况导致填入答案的失效了
  > + 但是，前提是你需要一份正确答案的题库，于是需要另一个账号（小号）
  >
+ 为什么要小号？
  >
  > + 使用小号主要是由于你提交完作业就会立刻出现答案，虽然用大号也可以，但是你用大号就导致有些作业就只能作答一次，答案倒是有了，但是用不上了
  > + __小号的功能__: 添加大号的课程，去做题目，直接提交（0分），查看作答记录，提取题目ID以及答案，生成题库
  > + 反复循环，把所有大号的相同课程的题全部做完，获取到适配你大号的题库，就生成了完整的题库
  >
+ __之后的流程__: 大号开始工作，拿到题目ID去题库遍历出答案，并填入答案，提交，就成功作答完全部的作业
+ 不好的地方
  > 这种方式会明显可以知道，你所填的答案将会和标准答案一致，就像简答题会和标准答案一模一样，__标点符号都一致__

## 使用方法

1. 填入 __大号 小号__ 账号密码 不填写小号信息为仅刷课模式
2. ~~里面配置暂时无法自定义，熟悉 `python` 可自行修改，有时间再更新，目前默认 __刷课+测验+考试+测验~~
3. 运行 `StartWork.py`

## 部分问题
>
> 1. __运行全部成功，但部分作业考试测验显示未做 ？__
>
>> 由于部分作业数量显示不正常，导致无法完全正确提交，需要前往官网，手动点进去提交，但是答题答案已经选上去了，注意：__官网提交请注意时长停留久一点，不然提交完一个作业就几秒就做完了
>
> 2. __简答题 ？__
>
>> 部分简答题需要上传附件，导致该类型的作业是不会自动提交的，但是其他选项会填入，请运行结束后去网站上自行检查一遍

## BUG 提交

+ 请详细提供 __错误信息__ 以及错误出现的 __代码行__
+ [![Email](http://rescdn.qqmail.com/zh_CN/htmledition/images/function/qm_open/ico_mailme_11.png "QQ Email")](http://mail.qq.com/cgi-bin/qm_share?t=qm_mailme&email=KBkbHhsQEBgZHxpoWVkGS0dF)

## 免责声明

⚠️本项目仅限于学习交流使用，项目中使用的代码及功能如有侵权或违规请联系作者删除

⚠️本项目接口数据均来自于mooc，请勿用于其它商业目的

⚠️如使用本项目代码造成侵权与作者无关

## 效果展示

![1](./images/1.jpg)

---

![2](./images/2.jpg)

---

![3](./images/3.jpg)

---

![4](./images/4.jpg)

---

![5](./images/5.jpg)

---

![6](./images/6.jpg)

---
## 投币支持

<center>
  <img src="./images/wxpay.jpg" width="40%">
  <img src="./images/alipay.jpg" width="40%">
</center>
