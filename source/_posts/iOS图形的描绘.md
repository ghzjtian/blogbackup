---
title: iOS图形的描绘
tags: [iOS]
categories: [iOS]
date: 2018-10-10 17:46:19
---

# [1.简单线条的描绘](#simple_line)
# [2.简单的形状绘制](#simple_shape)
# [3.自定义 View](#custom_view)


<!-- more -->

***
***
***

# 1.简单线条的描绘<a name="simple_line"/>

### 1.画一条直线(从点 (10,10) 到 (100,100) 的蓝色的线)
* 1.先把笔头移到指定的位置.
* 2.再画一条线到指定的点.
* 3.设置线的宽度和颜色.
```
- (void)drawRect:(CGRect)rect {
    UIBezierPath *linePath = [UIBezierPath bezierPath];
    [linePath moveToPoint:CGPointMake(10.0, 10.0)];
    [linePath addLineToPoint:CGPointMake(100.0, 100.0)];
    linePath.lineWidth = 3;
    [[UIColor blueColor] setStroke];
    [linePath stroke];
    }
```
<img src="/assets/imgs/iOS/ScreenShot_2018-10-10_13.55.10.png">

### 2.画一条 sin 曲线.
* 0.[iOS_三角函数](https://www.jianshu.com/p/dbcf833eeaa2)
* 1.先理解 `弧长等于半径的圆弧所对应的圆心角为1弧度`,即 `角度 = 弧度 * (180/PI)`,`弧度 = 角度 * (PI/180)`
* 2.iOS  中的 `double sin(double);` 函数中的参数是弧度,即 `sin(30*(M_PI/180))=0.5` `sin(90*(M_PI/180))=1`
* 3.sin 曲线的形状
<img src="/assets/imgs/iOS/sin_svg.png">
* 4.[sin 函数中的 `振幅 amplitude、周期 cycle、相移 hori_move 和频率 frequency` 概念的理解](https://www.shuxuele.com/algebra/amplitude-period-frequency-phase-shift.html)
* 5.了解了以上的那些，就可以开始描绘出我们的 `sin` 曲线了:
	
```
- (void)drawRect:(CGRect)rect {
    
    UIBezierPath *linePath = [UIBezierPath bezierPath];
    [linePath moveToPoint:CGPointMake(0, 100.0)];
    
    float amplitude = 20;    //振幅
//    float cycle = 2*M_PI/self.frame.size.width;//self.frame.size.width/(2*M_PI);        // 一个周期为控件的宽度,弧度计算
    float cycle = 360 / self.frame.size.width * 2;  // 一个周期为 控件的宽度/2 ,用角度计算
    float hori_move = 0;    //相移为 0
    float ver_move = 100;   //垂直位移为 100
    
    for (float x=0.0; x<self.frame.size.width; x++) {
        //         float y = amplitude * sin(cycle*x+hori_move) + ver_move; //弧度计算
        float y = amplitude * sin(cycle * (x*(M_PI/180))+hori_move) + ver_move; //角度计算
        [linePath addLineToPoint:CGPointMake(x, y)];
    }
    linePath.lineWidth = 1;
    [[UIColor blueColor] setStroke];
    [linePath stroke];
}
```
* 6.效果图
	* 1.至于为什么波峰先向下，是因为手机屏幕的 Y 轴是倒转的. 
<img src="/assets/imgs/iOS/ScreenShot_2018-10-10_15.26.20.png">


* 7.如果是需要动图，如示波器那种，就只需一直增加 相移和定时更新 View 即可,如:
<img src="/assets/imgs/iOS/wave7.gif">

	* 1.相关的代码:
	
```
	//
//  LineView.m
//  TestWaveView
//
//  Created by tian zeng on 2018/10/10.
//  Copyright © 2018年 GLB. All rights reserved.
//

#import "LineView.h"
@interface LineView()
@property (nonatomic, strong) CADisplayLink *displayLink;
@property (nonatomic,assign) CGFloat offsetX;
@end

@implementation LineView

-(void)awakeFromNib{
    [super awakeFromNib];
    self.backgroundColor = [UIColor yellowColor];
    [self addDisplayLinkAction];
}

- (instancetype)initWithFrame:(CGRect)frame{
    if(self=[super initWithFrame:frame]){
        self.backgroundColor = [UIColor purpleColor];
    }
    return self;
}


- (void)addDisplayLinkAction
{
    //添加到刷新的 LOOP 中，刷新的频率与屏幕刷新的频率一样
    _displayLink = [CADisplayLink displayLinkWithTarget:self selector:@selector(displayLinkAction)];
    [_displayLink addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSRunLoopCommonModes];
}

- (void)displayLinkAction
{
    
    _offsetX += 0.1;
    
    //完成,移除刷新 LOOP
    if (_offsetX >= 1000)  [self removeDisplayLinkAction];
    
    // 通知更新 VIEW
    [self setNeedsDisplay];
}
- (void)removeDisplayLinkAction
{
    [_displayLink invalidate];
    _displayLink = nil;
}

// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect {
    
    UIBezierPath *linePath = [UIBezierPath bezierPath];
    [linePath moveToPoint:CGPointMake(0, 100.0)];
    
    float amplitude = 20;    //振幅
//    float cycle = 2*M_PI/self.frame.size.width;//self.frame.size.width/(2*M_PI);        // 一个周期为控件的宽度,弧度计算
    float cycle = 360 / self.frame.size.width * 2;  // 一个周期为 控件的宽度/2 ,用角度计算
    float hori_move = 0;    //相移为 0
    float ver_move = 100;   //垂直位移为 100
    
    for (float x=0.0; x<self.frame.size.width; x++) {
        //         float y = amplitude * sin(cycle*x+hori_move+_offsetX) + ver_move; //弧度计算
        float y = amplitude * sin(cycle * (x*(M_PI/180))+hori_move+_offsetX) + ver_move; //角度计算
        [linePath addLineToPoint:CGPointMake(x, y)];
    }
    linePath.lineWidth = 1;
    [[UIColor blueColor] setStroke];
    [linePath stroke];
}


@end
```


***

# 2.简单的形状绘制<a name="simple_shape"/>
* 1.简单的三角形
	* 1.与线的不同主要为最后调用 `fill` 方法,会自动连接终点与起点，然后围成一个图形.
	
```
- (void)drawRect:(CGRect)rect {
    UIBezierPath *linePath = [UIBezierPath bezierPath];
    [linePath moveToPoint:CGPointMake(10, 10.0)];
    [linePath addLineToPoint:CGPointMake(50, 10)];
    [linePath addLineToPoint:CGPointMake(60, 112)];
    [[UIColor blueColor] set];
    [linePath fill];
```
	* 2.效果图
<img src="/assets/imgs/iOS/ScreenShot_2018-10-10_16.28.50.png">

* 2.动态波浪图
	* 1.根据上一步的波浪线条，改装一下即可

<img src="/assets/imgs/iOS/wave8.gif">
	* 2.相关的代码
```
//
//  LineView.m
//  TestWaveView
//
//  Created by tian zeng on 2018/10/10.
//  Copyright © 2018年 GLB. All rights reserved.
//

#import "ShapeView.h"
@interface ShapeView()
@property (nonatomic, strong) CADisplayLink *displayLink;
@property (nonatomic,assign) CGFloat offsetX;
@property (nonatomic,assign) CGFloat offsetY;
@end

@implementation ShapeView

-(void)awakeFromNib{
    [super awakeFromNib];
    self.backgroundColor = [UIColor yellowColor];
    _offsetY = 100;
    [self addDisplayLinkAction];
}

- (instancetype)initWithFrame:(CGRect)frame{
    if(self=[super initWithFrame:frame]){
        self.backgroundColor = [UIColor purpleColor];
    }
    return self;
}


- (void)addDisplayLinkAction
{
    //添加到刷新的 LOOP 中，刷新的频率与屏幕刷新的频率一样
    _displayLink = [CADisplayLink displayLinkWithTarget:self selector:@selector(displayLinkAction)];
    [_displayLink addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSRunLoopCommonModes];
}

- (void)displayLinkAction
{
    
    _offsetX += 0.1;
    _offsetY -= 0.1;
    
    //完成,移除刷新 LOOP
    if (_offsetX >= 1000||_offsetY<0){
        [self removeDisplayLinkAction];
    }
    
    // 通知更新 VIEW
    [self setNeedsDisplay];
}
- (void)removeDisplayLinkAction
{
    [_displayLink invalidate];
    _displayLink = nil;
}

// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect {
    
    UIBezierPath *linePath = [UIBezierPath bezierPath];
    [linePath moveToPoint:CGPointMake(0, _offsetY)];
    
    float amplitude = 10;    //振幅
    //    float cycle = 2*M_PI/self.frame.size.width;//self.frame.size.width/(2*M_PI);        // 一个周期为控件的宽度,弧度计算
    float cycle = 360 / self.frame.size.width * 5;  // 一个周期为 控件的宽度/2 ,用角度计算
    float hori_move = 0;    //相移为 0
//    float ver_move = 100;   //垂直位移为 100

    for (float x=0.0; x<self.frame.size.width; x++) {
        //         float y = amplitude * sin(cycle*x+hori_move+_offsetX) + ver_move; //弧度计算
        float y = amplitude * sin(cycle * (x*(M_PI/180))+hori_move+_offsetX) + _offsetY; //角度计算
        [linePath addLineToPoint:CGPointMake(x, y)];
    }
    [linePath addLineToPoint:CGPointMake(self.frame.size.width, self.frame.size.height)];
    [linePath addLineToPoint:CGPointMake(0, self.bounds.size.height)];
    
    
    [[UIColor blueColor] set];
    [linePath fill];
    
}


@end

```
	
***

# 3.自定义 View<a name="custom_view"/>
* 1.让自定义 View 可以在 `Interface Builder` 中查看及实时编辑.
	* [1.IB_DESIGNABLE Custom Views in Interface Builder](https://useyourloaf.com/blog/ib-designable-custom-views-in-interface-builder/)
		* [1.Source Code](https://github.com/kharrison/CodeExamples/tree/master/Designable)
	





	
	
	
