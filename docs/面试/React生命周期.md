# React生命周期

componentWillMount：在渲染前调用，在客户端也在服务端。

componentDidMount：在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异部操作阻塞UI)。

componentWillReceiveProps：在组件接收到一个新的prop(更新后)时被调用。***这个方法在初始化render时不会被调用。***

shouldComponentUpdate：返回一个布尔值。在组件接收到新的props或者state时被调用。***在初始化时或者使用forceUpdate时不被调用。*** 
可以在你确认不需要更新组件时使用。

componentWillUpdate：在组件接收到新的props或者state但还没有render时被调用。***在初始化时不会被调用。***

componentDidUpdate：在组件完成更新后立即调用。***在初始化时不会被调用。***

componentWillUnmount：在组件从DOM中移除的时候立刻被调用。


## Reference

- [React 组件生命周期](http://www.runoob.com/react/react-component-life-cycle.html)
