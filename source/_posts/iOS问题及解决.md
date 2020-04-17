---
title: iOS 问题及解决
tags: [iOS]
categories: [iOS]
date: 2018-11-12 17:46:19
---

## 1.遇到的问题



<!-- more -->

## 1.遇到的问题

#### 1.[动态设置 ScrollView 滚动的高度](https://www.jianshu.com/p/400fa268c2c8)
* 1.遇到了在 `Storyboard` 中添加了 `ScrollView` 和相应的 `contentView`,并设置了 `contentView` 的约束的高度,但这样是固定的高度，不会动态改变高度.
	* 1.在代码中设置下面这样，就解决了动态适配高度的问题:

```

-(void)viewDidAppear:(BOOL)animated{
    [super viewDidAppear:animated];
     //[动态设置 ScrollView 滚动的高度]( https://www.jianshu.com/p/400fa268c2c8 )
     // _next_bt 为最底下的一个控件
    CGFloat height = _next_bt.frame.origin.y+_next_bt.frame.size.height+44;
    _contentViewHC.constant = height;
}

```

#### 2.[导航栏显示和隐藏的坑] ( https://www.jianshu.com/p/60e2369bbe0e )

* 1.在没有 `Navigation bar` 的页面 push 到有 `Navigation bar` 的页面，然后按返回,会发生突然不见 `Navigation bar` 的现象,解决如下.

```
-(void)viewWillAppear:(BOOL)animated{
    [super viewWillAppear:animated];
    //显示导航条,
    //[导航栏显示和隐藏的坑] ( https://www.jianshu.com/p/60e2369bbe0e )
    [self.navigationController setNavigationBarHidden:NO animated:animated];
}
```
