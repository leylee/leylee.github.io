# HITSchedule 哈工大第三方课程表 Android App 简要分析

## 写在前面

在学习软件构造课程的过程中, 我一直希望可以把学到的东西应用在项目中实践. 作为 [HITSchedule](https://github.com/HITSchedule/HITSchedule) 软件目前的维护者, 我将利用软件构造课程上学到的知识, 对 HITSchedule 中的某些内容进行简要分析.

## 文件结构

HITSchedule 使用 Java 语言开发, 使用 gradle 构建. 核心源代在 app/src/main/java 目录中. 由于历史遗留原因, 软件的包名为 `com.example.hitschedule`, 因此代码在 app/src/main/java/com/example/hitschedule 中, 以下简称 `SRCROOT`.

`SRCROOT` 中, 包含一个 Application.java 和若干个模块: `ui`, `dialog`, `adapter`, `util`, `view`, `database`.

## Adapter 模式

Adapter 模式是软件开发中常用的模式之一. Adapter 通过适配器的转换功能, 插入在两个类之间, 屏蔽两个类接口的差异, 对调用者使用的接口进行抽象, 以屏蔽底层实现的差异. 比如, 在 `SubjectAdapter` 中, 抽象出了 `getCount`, `getItem`, `getItemId`, `getView` 等方法, 为上层应用提供统一的接口. 内部则是将这些操作与内部的 `scheduleList` 等底层类绑定, 对这些对象的方法进行包装.

采用此种设计模式有很多优点:

- 将目标类和适配者类解耦，通过引入一个适配器类来重用现有的适配者类，而无须修改原有代码。
- 增加了类的透明性和复用性，将具体的实现封装在适配者类中，对于客户端类来说是透明的，而且提高了适配者的复用性。
- 灵活性和扩展性都非常好，通过使用配置文件，可以很方便地更换适配器，也可以在不修改原有代码的基础上增加新的适配器类，完全符合“开闭原则”。

## Repeat myself 问题

在 `WeekChooseAdapter` 中, 用于选择课程周数的列表共有 20 个 button. 对于这 button, 代码中采取复制相关代码 20 次的方式解决, 看起来就像这样:

```java
public View getView(final int i, View view, ViewGroup viewGroup) {
        myView = LayoutInflater.from(mContext).inflate(resourceId,null);
//        initView(myView);

        button1 = myView.findViewById(R.id.button1);
        button2 = myView.findViewById(R.id.button2);
        button3 = myView.findViewById(R.id.button3);
        button4 = myView.findViewById(R.id.button4);
        //....
        button20 = myView.findViewById(R.id.button20);

        buttons.add(button1);
        buttons.add(button2);
        buttons.add(button3);
        buttons.add(button4);
        //....
        buttons.add(button20);
```

这些代码十分丑陋, 且不便于维护, 对于逻辑上的任何一个小的修改, 都要重复修改 20 次, 很容易产生 bugs. 之后, 我将会重构这段代码, 使用 List 存储这 20 个 button, 并进行统一的修改和回调函数注册等操作, 减少代码的复杂度.

## 可以重构的一些 WebViewActivity

`ui` module 中, 有很多继承自 Activity 类, Activity 是 Android 应用程序的基本组成单元之一. 在早期的版本中, 仅有一个 `WebViewActivity` 用于展示校车时刻等内容. 在疫情期间, 我在软件中添加学工系统每日上报的入口. 后来, 由于旧的登录接口失效, 又把课表的登录认证迁移至统一身份认证, 仍然需要一个新的 WebView. 这样一来, 就又添加了 `ReportWebViewActivity` 和 `LoginWebViewActivity`. 之所以新建这两个类, 是因为需要在窗口中注入 javascript, 实现自动跳转和输入输出框的编辑功能. 在这些类中, 大体框架都是相同的, 仅有几个加载动作的回调函数差异较大. 但我之前采用了复制粘贴, 再稍加改动的方式完成其余的两个 WebView Activity, 没有做到 reusable. 接下来, 我会将 WebView 写成一个抽象类, 在需要实现不同功能时, 重写其中的抽象方法, 完成对 WebView 基类的复用.

## 写在最后

以上就是我在学习软件构造后, 在 `HITSchedule` 中获得的发现和找到的问题. 作为一名刚刚入门的开源软件开发者, 我有责任保证代码的质量. 尽管上述问题有的是历史遗留代码, 有的是我写出来的低质量代码, 但在将来, 我会根据软件构造课上学习到的知识, 尽力重构代码, 以实现更好的软件质量.
