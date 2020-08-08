---
title: 『React Navigation 3x系列教程』createSwitchNavigator开发指南
date: 2019-12-20 12:41:59
tags: React & React Native
categories: React & React Native

---

## [createSwitchNavigator]

SwitchNavigator 的用途是一次只显示一个页面。 默认情况下，它不处理返回操作，并在你切换时将路由重置为默认状态。

## [createSwitchNavigator API]

> createSwitchNavigator(RouteConfigs, SwitchNavigatorConfig):

- `RouteConfigs`(必选，同createStackNavigator的RouteConfigs)：路由配置对象是从路由名称到路由配置的映射，告诉导航器该路由呈现什么。
- `SwitchNavigatorConfig `(可选)：配置导航器的路由;

## [SwitchNavigatorConfig]

几个被传递到底层路由以修改导航逻辑的选项：

- initialRouteName -第一次加载时初始选项卡路由的 routeName。
- resetOnBlur - 切换离开屏幕时，重置所有嵌套导航器的状态。 默认为true。
- paths - 提供 routeName 到 path 配置的映射, 它重写 routeConfigs 中设置的路径。
- backBehavior - 控制 “返回” 按钮是否会导致 Tab 页切换到初始 Tab 页? 如果是, 设置为 initialRoute, 否则 none。 默认为none行为。

## [【案例1】使用createSwitchNavigator进行登录场景的跳转]

多数应用程序都要求用户通过某种方式进行身份验证才能访问与用户或其他私人内容相关的数据。 通常情况下，流程如下所示：

- 用户打开应用。
- 该应用程序从持久存储中加载某个身份验证状态（例如，AsyncStorage）。
- 当状态被加载时，根据是否加载有效的认证状态，向用户呈现认证页面或主页面。
- 当用户注销时，我们清除认证状态并跳转到认证页面。
- 注意：我们说“认证页面”，因为通常有不止一个。 您可能会有一个主页面，其中包含用户名和密码字段，一个用于“忘记密码”的页面，一个用于注册的页面。

![1](H:\上传到git里面的md文件图片\picture\React\RN3.x\1.gif)

## [第一步：创建一个createSwitchNavigator类型的导航器]

```react
const AppStack = createStackNavigator({
    Home: {
        screen: HomePage
    },
    Page1: {
        screen: Page1
    }
});
const AuthStack = createStackNavigator({
    Login: {
        screen: Login
    },
},{
    navigationOptions: {
        // header: null,// 可以通过将header设为null 来禁用StackNavigator的Navigation Bar
    }
});

export default createSwitchNavigator(
    {
        Auth: AuthStack,
        App: AppStack,
    },
    {
        initialRouteName: 'Auth',
    }
);
```

## [第二步：界面跳转]

```react
 const {navigation} = this.props;
 ...
 <Button
    title="Login"
    onPress={() => {
        navigation.navigate('App');
    }}
/>
```