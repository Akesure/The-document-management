# weex里Vuex state使用storage持久化

### 在weex里使用Vuex作为state管理工具，问题来了，如何使得state可以持久化呢？weex官方提供store模块，因此我们可以尝试使用该模块来持久化state。

先看下该模块介绍：

**storage 是一个在前端比较常用的模块，可以对本地数据进行存储、修改、删除，并且该数据是永久保存的，除非手动清除或者代码清除。但是，storage 模块有一个限制就是浏览器端（H5）只能存储小于5M的数据，因为在 H5/Web 端的实现是采用 HTML5 LocalStorage API。而 Android 和 iOS 这块是没什么限制的。**

首先，引入模块：
```
const storage = weex.requireModule('storage')
```
定义state
```
var state = {
    banner:[],
    activeTabIndex:0,
    lists: {
        searchList:[],
        tabColumns: {
            new:[],
            hot:[],
            select:[]
        },
        items:[]
    }
}
```

初始化时，从storage加载state数据
```
// 从storage里加载数据
storage.getItem(STORAGE_KEY, event => {
  if (event.result == "success" && event.data){
      // 这里可以使用extend等方法，这里仅举例说明
      var data = JSON.parse(event.data);
      state.banner = data.banner;
      state.activeTabIndex = data.activeTabIndex;
  }
})
```　　

关键来了，如何存储？Vuex提供了插件机制，我们可以通过插件订阅state的每一次更改，在更改的时候保存我们感兴趣的就OK了
```
// 存储plugin，存储感兴趣的数据，store里数据太多，没必要全持久化
const storagePlugin = store => {
  store.subscribe((mutation, {activeTabIndex,banner}) => {
    storage.setItem(STORAGE_KEY, JSON.stringify({activeTabIndex,banner}),event => {
      console.log('cache success');
    })
  })
}
```

最后，创建Vuex，大功告成
```
const store = new Vuex.Store({
  actions,
  mutations,
  plugins:[storagePlugin],

  state: state,

  getters: {
    // ids of the items that should be currently displayed based on
    // current list type and current pagination
    activeIds (state) {
      const { activeType, lists, counts } = state
      return activeType ? lists[activeType].slice(0, counts[activeType]) : []
    },

    // items that should be currently displayed.
    // this Array may not be fully fetched.
    activeItems (state, getters) {
      return getters.activeIds.map(id => state.items[id]).filter(_ => _)
    }
  }
})
```
