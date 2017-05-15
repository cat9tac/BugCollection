# Android支持RTL(从右向左)语言

[Android支持RTL(从右向左)语言介绍](http://droidyue.com/blog/2014/07/07/support-rtl-in-android/)

通过上面的博客可以基本的了解这个概念。
在布局方面常遇到的问题主要有：
  - 在布局文件里，设置位置时使用了类似于“margingLeft”之类的参数，都要改成"margingStart"。注意到将“Left"替换为 "Start"， "Right"替换为"End”这一点基本上就没啥问题了
  - 英文和从右到左语言混排的字符串不能够被显示为从右到左布局。
  - ViewPager不支持从右到左布局。

# 解决
#### 1.英文和从右到左文字混排的字符串（[参考介绍](https://segmentfault.com/a/1190000003781294)）
###### Support BiDi(双向字符集)显示
双向字符集(BiDi)通常是指文字可以从左到有(LTR)和从右到左(RTL)双向书写的文字.
比如本国文字是从右向左(乌尔都语, 阿拉伯语)与英语混排. 

###### 双向字符集语言处理算法
在 BiDi 中，所有的非标点符号被称为强字符。而标点符号既可以是从左向右 LTR 也可以是从右向左 RTL。因为不含任何的方向信息，所以被称为弱字符。
 - 标点符号放在两段有相同方向文字的中间，标点符号将继承相同的方向
 - 标点符号放在两段有不同方向的文字中间，标点符号将继承全局方向
 - 一个弱字符紧挨着一个弱字符，BiDi算法将根据最近相邻的“强”字符来决定字符方向。在某些情况下，这会导致非预期的情况>出现。为了纠正错误，需要使用伪强字符RLM(\u200E)或者LRM(\u200F)插入到文字中间来调整字符的显示。
显然RLM, 表示从右到左, LRM从左到右. 使用时, 在要需要的的伪强字符后添加RLM或者LRM
#### 2.ViewPager不支持从右到左布局
即便在从右到左语言下ViewPager内部内容的布局发生了从右到左的改变，但是由于ViewPager不支持，以至于滑动的时候，方向不改变，会造成与从左到右布局相反的效果。所以先通过下面的代码检测当前是否是从右至左的布局，如果是的话则将ViewPager中的布局进行180度旋转
```sh
if(view!=null&&TextUtils.getLayoutDirectionFromLocale(getResources().getConfiguration().locale)==1) {
            view.setRotationY(180);
        }
```
同时在ViewPager的代码中检测当前是从右至左的布局时
```sh
if(mViewPager!=null&&TextUtils.getLayoutDirectionFromLocale(getResources().getConfiguration().locale)==1){
            mViewPager.setRotationY(180);
        }
```



