# Markdown学习笔记

## 0. 数学公式用

### 0.0 来源

[MarkDown 插入数学公式实验大集合](https://juejin.im/post/5a6721bd518825733201c4a2)

### 0.1 插入方式

1. 行间插入

    行间插入公式，$a+b$ 这样就行

2. 另取一行

    通过使用两个$符号，如:
    $$a+b$$

### 0.2 常用基本类型的插入

1. 上下标,上标用^，下标用_，多于一位用大括号括起来

   - 上标： $30^{hello}$
   - 下标： $H_{2345}O$
   - $$x^{123}_{(456)}$$
   - ${}^{**}x^*$

2. 分式

   - 通过使用\frac{}{}表示分式，第一个大括号写分子$\frac{1}{2}$
   - $$\frac{x+y}{2}$$
   - $$\frac{1}{1+\frac{1}{2}}$$

3. 根式

    - sqrt[]{},[]中代表几次根式，{}中写根号下的表达式，下面例子中，二三的区别在于为了美观微调位置
    > $$\sqrt{2}<\sqrt[3]{3}$$
    > $$\sqrt{1+\sqrt[p]{1+a^2}}$$
    > $$\sqrt{1+\sqrt[^p\!]{1+a^2}}$$

4. 求和、积分

    - 求和函数表达式：sum_{起点}^{终点}表达式
    - 积分函数表达式：int_{下限}^{上限} 被积函数d被积量
    > $$\sum_{k=1}^{n}\frac{1}{k}$$
    > $\sum_{k=1}^n\frac{1}{k}$
    > $$\int_a^b f(x)dx$$
    > $\int_a^b f(x)dx$
    - 一个很有意思的现象，行间公式都被压缩了


5. 空格

    - 紧贴 $a\!b$
    - 没有空格 $ab$
    - 小空格 $a\,b$
    - 中等空格 $a\;b$
    - 大空格 $a\ b$
    - quad空格 $a\quad b$
    - 两个quad空格 $a\qquad b$

主要作用是通过微调距离，使得公式更漂亮，如：

$$\int_a^b f(x)dx$$
$$\int_a^b f(x)\mathrm{d}x$$
$$\int_a^b f(x)\,\mathrm{d}x$$

6. 等更新



## 1. 文本部分

### 1.1 斜体、粗体、删除线

- 斜体：*斜体*
- 粗体：**粗体**
- 删除线： ~~删除线~~

### 1.2 分级标题

    会，省略不写

### 1.3 常用表情

    后补

## 2. 代码

### 2.1 代码块

```python

    # tab上方，esc下方那个符号
    print('hello world')

```

```Java
public class PaintCanvas extends View {
    private Paint mPaint;
    // 省略构造方法
    
    private void init() {       
        mPaint = new Paint();
        mPaint.setAntiAlias(true);        
    }
    @Override
    protected void onDraw(Canvas canvas) {
        canvas.drawCircle(500, 550, 500, mPaint);
    }
}
```

### 2.2 正文中的代码

正文中有 `print('hello world')` 一段代码。

## 3.有序列表，无序列表

### 3.1 有序列表

1. 第一
2. 第二
   1. 第二点的第一点
      1. 又是第一
         1. 还是第一


### 3.2 无序列表

- 这样
+ 这样
  + 这样
    + 还可以这样
      + 还有吗

## 4.引用

### 4.1 单引用

> 我的微信公众号：还没有
> 
> 哈哈哈，
> 略略略

### 4.2 引用中的引用

> 第一级引用
> > 第二级引用
> > > 第三级
> > > > 有没有第四级
> > > > > 第五级
> > > > 
> > > > 就这样吧，哈哈哈，

## 5.链接

### 5.1 网页链接

[百度](http://baidu.com/)

### 5.2 图片

1. 网页图片

![图片描述](https://open.weixin.qq.com/qr/code?username=MrWuXiaolong)

1. 本地图片

![本地图片](本地链接)










