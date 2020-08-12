# 根据数据的变化在div中动态插入iframe
- 场景描述：当父节点数据list发生变化的时候，就在指定的子节点div中动态地加入iframe。
- 解决问题的过程：
   - 第一次尝试：使用watch监控list数据的变化，当list变化的时候，往指定的div中加入iframe。（失败，因为当list发生改变时，指定的div还未渲染完成，无法往里面插入iframe。）
   - 第二次尝试：使用MutationObserver API，定位在list的父节点，当父节点的子节点发生变化的时候，触发回调函数。（成功执行)
- MutationObserver的使用方法
```
🌰：
// 选择需要观察变动的节点
const targetNode = document.getElementById('some-id');

// 观察器的配置（需要观察什么变动）
const config = { attributes: true, childList: true, subtree: true };

// 当观察到变动时执行的回调函数
const callback = function(mutationsList, observer) {
    // Use traditional 'for loops' for IE 11
    for(let mutation of mutationsList) {
        if (mutation.type === 'childList') {
            console.log('A child node has been added or removed.');
        }
        else if (mutation.type === 'attributes') {
            console.log('The ' + mutation.attributeName + ' attribute was modified.');
        }
    }
};

// 创建一个观察器实例并传入回调函数
const observer = new MutationObserver(callback);

// 以上述配置开始观察目标节点
observer.observe(targetNode, config);

// 之后，可停止观察
observer.disconnect();
//如果需要在beforeDestroy时清除observer可以使用：        
this.$once('hook:beforeDestroy', () => {
            observer.disconnect();
});
```

- 参考：
[MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)


