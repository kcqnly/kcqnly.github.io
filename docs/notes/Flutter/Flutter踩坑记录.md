# Flutter踩坑记录

持续更新中......

## Container大小问题

我们知道Container组件，在没有子组件的时候，它的大小是占满整个父组件的，但是当它有了子组件的时候，它的大小是和子组件大小一样的。
但是设置`Alignment`对齐方式后，Container将会充满其父控件，相当于Android中match_parent,不在是根据子控件调整大小。

## 线性渐变坐标问题

CSS中的线性渐变的坐标是角度坐标，如下图所示
![css](https://www.runoob.com/wp-content/uploads/2014/07/7B0CC41A-86DC-4E1B-8A69-A410E6764B91.jpg)

而flutter中是x,y坐标，主要是通过`Alignment`来控制。

```dart
Alignment(this.x, this.y)
```

`Alignment`Widget会以**矩形的中心点作为坐标原点**，即`Alignment(0.0, 0.0)`。x、y的值从-1到1分别代表矩形左边到右边的距离和顶部到底边的距离，因此2个水平（或垂直）单位则等于矩形的宽（或高），如`Alignment(-1.0, -1.0)`代表矩形的左侧顶点，而`Alignment(1.0, 1.0)`代表右侧底部终点，而`Alignment(1.0, -1.0)`则正是右侧顶点，即`Alignment.topRight`。为了使用方便，矩形的原点、四个顶点，以及四条边的终点在`Alignment`类中都已经定义为了静态常量。

`Alignment`可以通过其坐标转换公式将其坐标转为子元素的具体偏移坐标：

```dart
(Alignment.x*childWidth/2+childWidth/2, Alignment.y*childHeight/2+childHeight/2)
```

## DropdownButton 下拉菜单的长度

flutter版本是不能设置下拉菜单的长度的，解决方法见
[stackoverflow](https://stackoverflow.com/questions/53983783/set-height-of-dropdownbuttonformfield-list)

```dart
return Container(
      // child: DropdownButton(
      //   underline: Container(height: 0), // 隐藏下划线
      //   hint: Text(
      //     '  第$_weekIndex周',
      //     style: TextStyle(
      //       color: Colors.black,
      //       fontSize: ScreenUtil().setSp(51),
      //     ),
      //   ),
      //   items: _generateItemList(),
      //   onChanged: (value) {
      //     timeProvider.weekIndex = value;
      //   },
      // ),
      child: custom.DropdownButton(
        height: ScreenUtil().setHeight(900),
        underline: Container(height: 0), // 隐藏下划线
        hint: Text(
          '  第$_weekIndex周',
          style: TextStyle(
            color: Colors.black,
            fontSize: ScreenUtil().setSp(51),
          ),
        ),
        items: _generateItemList(),
        onChanged: (value) {
          timeProvider.weekIndex = value;
        },
      ),
    );
```

Item的处理如下

```dart
  _generateItemList() {
    List<custom.DropdownMenuItem<dynamic>> items = new List();
    for (int i = 1; i <= 20; i++) {
      custom.DropdownMenuItem item = custom.DropdownMenuItem(value: i, child: new Text(i.toString()));
      items.add(item);
    }
    return items;
  }
```

## 滑动组件的蓝色波纹效果

在android上，滑动组件默认是有一个蓝色波纹效果，我们可以通过使用`ScrollConfiguration`来包裹滑动组件，并添加`behavior`属性，来消除蓝色波纹。

```dart
/*
 * @Author: kcqnly
 * @Date: 2020-08-28 19:14:51
 * @Last Modified by: kcqnly
 * @Last Modified time: 2020-08-28 19:18:35
 * @message 去除滚动的蓝色回弹
 */

import 'dart:io';

import 'package:flutter/material.dart';

class MyBehavior extends ScrollBehavior {
  @override
  Widget buildViewportChrome(
      BuildContext context, Widget child, AxisDirection axisDirection) {
    if (Platform.isAndroid || Platform.isFuchsia) {
      return child;
    } else {
      return super.buildViewportChrome(context, child, axisDirection);
    }
  }
}
```

```dart
        Container(
          height: ScreenUtil().setHeight(300) * list.length,
          child: ScrollConfiguration(
            behavior: MyBehavior(),
            child: ReorderableListView(
              children: list,
              onReorder: (int oldIndex, int newIndex) {
                setState(() {
                  if (newIndex == list.length) {
                    newIndex = list.length - 1;
                  }
                  var item = list.removeAt(oldIndex);
                  var course = courseModel.removeAt(oldIndex);
                  courseModel.insert(newIndex, course);
                  list.insert(newIndex, item);
                });
                CourseUtil.changeIndex(courseModel);
              },
            ),
          ),
        )
```
