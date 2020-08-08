---
title: 『React Navigation 3x系列教程』createDrawerNavigator开发指南
date: 2019-12-21 20:01:55
tags: React & React Native
categories: React & React Native

---

`createDrawerNavigator`抽屉效果，侧边滑出：

![4](H:\上传到git里面的md文件图片\picture\React\RN3.x\4.jpg)

## [createDrawerNavigator API]

> ```js
> createDrawerNavigator(RouteConfigs, DrawerNavigatorConfig):
> ```

- `RouteConfigs`(必选)：路由配置对象是从路由名称到路由配置的映射，告诉导航器该路由呈现什么。
- `DrawerNavigatorConfig`(可选)：配置导航器的路由(如：默认首屏，navigationOptions，paths等)样式(如，转场模式mode、头部模式等)。

从createDrawerNavigator API上可以看出`createDrawerNavigator`支持通过`RouteConfigs`和 `DrawerNavigatorConfig`两个参数来创建createDrawerNavigator导航器。

## [RouteConfigs]

RouteConfigs支持三个参数`screen`、`path`以及`navigationOptions`；

- `screen`(必选)：指定一个 React 组件作为屏幕的主要显示内容，当这个组件被DrawerNavigator加载时，它会被分配一个`navigation` prop。
- `path`(可选)：用来设置支持schema跳转时使用，具体使用会在下文的有关`Schema`章节中讲到；
- `navigationOptions`(可选)：用以配置全局的屏幕导航选项如：title、headerRight、headerLeft等；

## [DrawerNavigatorConfig]

- drawerWidth: 设置侧边菜单的宽度；
- drawerPosition: 设置侧边菜单的位置，支持’left’、 ‘right’，默认是’left’；
- contentComponent: 用于呈现抽屉导航器内容的组件，例如导航项。接收抽屉导航器的 navigation 属性 。默认为DrawerItems。有关详细信息，请参阅下文；
- contentOptions: 配置抽屉导航器内容，见下文;
- useNativeAnimations: 是否启用Native动画，默认启用；
- drawerBackgroundColor: 侧边菜单的背景；
- initialRouteName: 初始化哪个界面为根界面，如果不配置，默认使用RouteConfigs中的第一个页面当做根界面；
- order: drawer排序，默认使用配置路由的顺序；
- paths: 提供routeName到path config的映射，它覆盖routeConfigs中设置的路径。
- backBehavior: 后退按钮是否会导致标签切换到初始drawer？ 如果是，则设切换到初始drawer，否则什么也不做。 默认为切换到初始drawer。

## [自定义侧边栏(contentComponent)]

DrawerNavigator有个默认的带滚动的侧边栏，你也可以通过重写这个侧边栏组件来自定义侧边栏：

```
contentComponent:(props) => (
    <ScrollView style=>
        <SafeAreaView forceInset=>
            <DrawerItems {...props} />
        </SafeAreaView>
    </ScrollView>
)
```

## [ DrawerItems 的 contentOptions ]

contentOptions主要配置侧滑栏item的属性，只对DrawerItems，例如我们刚才写的例子，就可以通过这个属性来配置颜色，背景色等。其主要属性有：

- items: 路由数组，如果要修改路由可以可以修改或覆盖它；
- activeItemKey: 定义当前选中的页面的key；
- activeTintColor: 选中item状态的文字颜色；
- activeBackgroundColor: 选中item的背景色；
- inactiveTintColor: 未选中item状态的文字颜色；
- inactiveBackgroundColor: 未选中item的背景色；
- onItemPress: 选中item的回调，这个参数属性为函数，会将当前路由回调过去；
- itemsContainerStyle: 定义itemitem容器的样式；
- itemStyle: 定义item的样式；
- labelStyle: 定义item文字的样式；
- iconContainerStyle: 定义item图标容器的样式；
- activeLabelStyle：选中状态下文本样式；
- inactiveLabelStyle：非选中状态下文本样式；
- iconContainerStyle ：用于设置图标容器的样式。

> eg:

```
contentOptions: {
  activeTintColor: '#e91e63',
  itemsContainerStyle: {
    marginVertical: 0,
  },
  iconContainerStyle: {
    opacity: 1
  }
}
```



## [navigationOptions（屏幕导航选项）]

DrawerNavigator支持的屏幕导航选项的参数有：

- title: 可以用作headerTitle和drawerLabel的备选的通用标题。

- drawerLabel：侧滑标题；

- drawerIcon：侧滑的标题图标，这里会回传两个参数：

  - {focused: boolean, tintColor: string}：
    - focused: 表示是否是选中状态；
    - tintColor: 表示选中的颜色；

- drawerLockMode：指定抽屉的锁定模式。 这也可以通过在顶级路由器上使用screenProps.drawerLockMode 动态更新。

## [侧边栏操作(打开、关闭、切换)]

侧边栏支持以下几种操作方式：

```js
navigation.openDrawer();
navigation.closeDrawer();
navigation.toggleDrawer();
//或
navigation.dispatch(DrawerActions.openDrawer());
navigation.dispatch(DrawerActions.closeDrawer());
navigation.dispatch(DrawerActions.toggleDrawer());
```



其中路由名`openDrawer`对应这打开侧边栏的操作，`DrawerClose`对应关闭侧边栏的操作，`toggleDrawer`对应切换侧边栏操作，要进行这些操作我么还需要一个`navigation`，`navigation`可以从props中获取；

- 打开侧边栏：`navigation.openDrawer();`；
- 关闭侧边栏：`navigation.closeDrawer();`；
- 切换侧边栏：`navigation.toggleDrawer();`；

#### [其他API](https://coding.imooc.com/class/304.html?mc_marking=911b955aff9c2cfc95f12d0f15e1b2e7&mc_channel=shouji)

### [【案例1】使用DrawerNavigator做界面导航、配置navigationOptions、自定义侧边栏]

![5](H:\上传到git里面的md文件图片\picture\React\RN3.x\5.gif)

## [第一步：创建一个createDrawerNavigator类型的导航器]

```react
export const DrawerNav = createDrawerNavigator({
        Page4: {
            screen: Page4,
            navigationOptions: {
                drawerLabel: 'Page4',
                drawerIcon: ({tintColor}) => (
                    <MaterialIcons name="drafts" size={24} style=/>
                ),
            }
        },
        Page5: {
            screen: Page5,
            navigationOptions: {
                drawerLabel: 'Page5',
                drawerIcon: ({tintColor}) => (
                    <MaterialIcons
                        name="move-to-inbox"
                        size={24}
                        style=
                    />
                ),
            }
        },
    },
    {
        initialRouteName: 'Page4',
        contentOptions: {
            activeTintColor: '#e91e63',
        },
        contentComponent:(props) => (
            <ScrollView style=>
                <SafeAreaView forceInset=>
                    <DrawerItems {...props} />
                </SafeAreaView>
            </ScrollView>
        )
    }
);
```



## [第二步：配置navigationOptions：]

DrawerNavigator的navigationOptions有两个关键的属性，tabBarLabel标签与tabBarIcon图标：

```react
Page4: {
    screen: Page4,
    navigationOptions: {
        drawerLabel: 'Page4',
        drawerIcon: ({tintColor}) => (
            <MaterialIcons name="drafts" size={24} style=/>
        ),
    }
},
```



在上述代码中使用了`react-native-vector-icons`的矢量图标作为Tab的显示图标，drawerIcon接收一个React 组件，大家可以根据需要进行定制：

- tintColor: 当前状态下Item的颜色；
- focused: Item是否被选中；

## [第三步：界面跳转]

```react
render() {
    const {navigation} = this.props;
    return <View style=>
        <Text style={styles.text}>欢迎来到Page5</Text>
        <Button
            onPress={() => navigation.openDrawer()}
            title="Open drawer"
        />
        <Button
            onPress={() => navigation.toggleDrawer()}
            title="Toggle drawer"
        />
        <Button
            onPress={() => navigation.navigate('Page4')}
            title="Go to Page4"
        />
    </View>
}
```

> 代码解析：

页面跳转可分为两步：

- 1. 获取navigation：

  ```react
    const {navigation} = this.props;
  ```

  

- 1. 通过`navigate(routeName, params, action)`进行页面跳转：

```react
	 navigation.navigate('Page5');
 });
```

## [自定义侧边栏]

如果DrawerNavigator的侧边栏的效果无法满足我们的需求，我们可以通过`contentComponent`属性来自定义侧边栏：

```react
contentComponent:(props) => (
    <ScrollView style=>
        <SafeAreaView forceInset=>
            <DrawerItems {...props} />
        </SafeAreaView>
    </ScrollView>
)
```