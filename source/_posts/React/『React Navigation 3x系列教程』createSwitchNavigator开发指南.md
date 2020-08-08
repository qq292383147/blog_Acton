---
title: 『React Navigation 3x系列教程』 createMaterialTopTabNavigator开发指南
date: 2019-12-20 13:06:23
tags: React & React Native
categories: React & React Native

---

## [createMaterialTopTabNavigator API]

> createMaterialTopTabNavigator(RouteConfigs, TabNavigatorConfig):

- `RouteConfigs`(必选)：路由配置对象是从路由名称到路由配置的映射，告诉导航器该路由呈现什么。
- `TabNavigatorConfig`(可选)：配置导航器的路由(如：默认首屏，navigationOptions，paths等)样式(如，转场模式mode、头部模式等)。

从createMaterialTopTabNavigator API上可以看出`createMaterialTopTabNavigator `支持通过`RouteConfigs`和 `TabNavigatorConfig `两个参数来创建createMaterialTopTabNavigator导航器。

## [RouteConfigs]

RouteConfigs支持三个参数`screen`、`path`以及`navigationOptions`；

- `screen`(必选)：指定一个 React 组件作为屏幕的主要显示内容，当这个组件被TabNavigator加载时，它会被分配一个`navigation` prop。
- `path`(可选)：用来设置支持schema跳转时使用，具体使用会在下文的有关`Schema`章节中讲到；
- `navigationOptions`(可选)：用以配置全局的屏幕导航选项如：title、headerRight、headerLeft等；

## [TabNavigatorConfig]

- tabBarComponent：指定TabNavigator的TabBar组件；
- tabBarPosition: 用于指定TabBar的显示位置，支持’top’ 与 ‘bottom’两种方式；
- swipeEnabled : 是否可以左右滑动切换tab；
- lazy - 默认值是 false。如果是true，Tab 页只会在被选中或滑动到该页时被渲染。当为 false 时，所有的 Tab 页都将直接被渲染；（可以轻松实现多Tab 页面的懒加载）；
- optimizationsEnabled -是否将 Tab 页嵌套在到 中。如果是，一旦该 Tab 页失去焦点，将被移出当前页面, 从而提高内存使用率。
- animationEnabled : 切换页面时是否有动画效果。
- initialLayout : 包含初始高度和宽度的可选对象可以被传递以防止react-native-tab-view呈现中的一个帧延迟；
- tabBarOptions: 配置TaBar下文会详细讲解；
- initialRouteName : 默认页面组件，TabNavigator显示的第一个页面；
- order: 定义tab顺序的routeNames数组。
- paths: 提供routeName到path config的映射，它覆盖routeConfigs中设置的路径。
- backBehavior: 后退按钮是否会导致标签切换到初始tab？ 如果是，则设切换到初始tab，否则什么也不做。 默认为切换到初始tab。

## [tabBarOptions（tab配置）]

- activeTintColor: 设置TabBar选中状态下的标签和图标的颜色；
- inactiveTintColor: 设置TabBar非选中状态下的标签和图标的颜色；
- showIcon: 是否展示图标，默认是false；
- showLabel: 是否展示标签，默认是true；
- upperCaseLabel - 是否使标签大写，默认为true。
- tabStyle: 设置单个tab的样式；
- indicatorStyle: 设置 indicator(tab下面的那条线)的样式；
- labelStyle: 设置TabBar标签的样式；
- iconStyle: 设置图标的样式；
- style: 设置整个TabBar的样式；
- allowFontScaling: 设置TabBar标签是否支持缩放，默认支持；
- pressColor -Color for material ripple（仅支持 Android >= 5.0；
- pressOpacity -按下标签时的不透明度（支持 iOS 和 Android < 5.0）；
- scrollEnabled -是否支持 选项卡滚动

> eg:

```react
tabBarOptions: {
  labelStyle: {
    fontSize: 12,
  },
  tabStyle: {
    width: 100,
  },
  style: {
    backgroundColor: 'blue',
  },
}
```

## [navigationOptions（屏幕导航选项）]

createMaterialTopTabNavigator支持的屏幕导航选项的参数有：

- title: 可以用作headerTitle和tabBarLabel的备选的通用标题。
- swipeEnabled：是否允许tab之间的滑动切换，默认允许；
- tabBarIcon: 设置TabBar的图标；
- tabBarLabel: 设置TabBar的标签；
- tabBarOnPress: Tab被点击的回调函数，它的参数是一保函一下变量的对象：
  - navigation：页面的 navigation props
  - defaultHandler: tab press 的默认 handler
- tabBarAccessibilityLabel：选项卡按钮的辅助功能标签。 当用户点击标签时，屏幕阅读器会读取这些信息。 如果您没有选项卡的标签，建议设置此项；
- tabBarTestID：用于在测试中找到该选项卡按钮的 ID；

## [【案例1】使用createMaterialTopTabNavigator做界面导航、配置navigationOptions]

![2](H:\上传到git里面的md文件图片\picture\React\RN3.x\2.png)

## [第一步：创建一个createMaterialTopTabNavigator类型的导航器]

```react
export const MaterialTopTabNavigator = createMaterialTopTabNavigator({//在这里配置页面的路由
        Page1: {
            screen: Page1,
            navigationOptions: {
                tabBarLabel: 'Page10',
                tabBarIcon: ({tintColor, focused}) => (
                    <Ionicons
                        name={'ios-home'}
                        size={26}
                        style=
                    />
                ),
            }
        },
        Page4: {
            screen: Page4,
            navigationOptions: {
                tabBarLabel: 'Page2',
                tabBarIcon: ({tintColor, focused}) => (
                    <Ionicons
                        name={'ios-people'}
                        size={26}
                        style=
                    />
                ),
            }
        },
        Page3: {
            screen: Page3,
            navigationOptions: {
                tabBarLabel: 'Page3',
                tabBarIcon: ({tintColor, focused}) => (
                    <Ionicons
                        name={'ios-chatboxes'}
                        size={26}
                        style=
                    />
                ),
            }
        },
    },
    {
        tabBarOptions: {
            tabStyle: {
                minWidth: 50
            },
            upperCaseLabel: false,//是否使标签大写，默认为true
            scrollEnabled: true,//是否支持 选项卡滚动，默认false
            // activeTintColor: 'white',//label和icon的前景色 活跃状态下（选中）
            // inactiveTintColor: 'gray',//label和icon的前景色 活跃状态下（未选中）
            style: {
                backgroundColor: '#678',//TabBar 的背景颜色
            },
            indicatorStyle: {
                height: 2,
                backgroundColor: 'white',
            },//标签指示器的样式
            labelStyle: {
                fontSize: 13,
                marginTop: 6,
                marginBottom: 6,
            },//文字的样式
        },
    }
)
```

## [第二步：配置navigationOptions：]

TabNavigator的navigationOptions有两个关键的属性，tabBarLabel标签与tabBarIcon图标：

```react
Page2: {
    screen: Page2,
    navigationOptions: {
        tabBarLabel: 'Page2',
        tabBarIcon: ({tintColor, focused}) => (
            <Ionicons
                name={focused ? 'ios-people' : 'ios-people-outline'}
                size={26}
                style=
            />
        ),
    }
},
```

在上述代码中使用了`react-native-vector-icons`的矢量图标作为Tab的显示图标，tabBarIcon接收一个React 组件，大家可以根据需要进行定制：

- tintColor: 当前状态下Tab的颜色；
- focused: Tab是否被选中；

## [第三步：界面跳转]

```react
 const {navigation} = this.props;
 ...
 <Button
    title="跳转到页面4"
    onPress={() => {
        navigation.navigate("Page4",{ name: 'Devio' })
    }}
/>
```

> 代码解析：

页面跳转可分为两步：

- 1. 获取navigation：

  ```react
    const {navigation} = this.props;
  ```

- 1. 通过`navigate(routeName, params, action)`进行页面跳转：

  ```react
     navigation.navigate('Page2');
     navigation.navigate('Page3',{ name: 'Devio' });
  ```

  这里在跳转到`Page3`的时候传递了参数`{ name: 'Devio' }`；
