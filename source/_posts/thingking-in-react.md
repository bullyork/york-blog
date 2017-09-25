---
title: thingking-in-react
date: 2017-08-30 10:54:28
tags: react
---
### 引子

所谓齐其家在修其身者，人之其所亲爱而辟焉，之其所贱恶而辟焉，之其所畏敬而辟焉，之其所哀矜而辟焉。故好而知其恶，恶而知其美者，天下鲜矣。故谚有之曰：“人莫知其子之恶，莫知其苗之硕。”此谓身不修不可以齐其家。

--《大学》

### 思想

单一功能原则：此规则规定每个类都应该有一个单一的功能，并且该功能应该由这个类完全封装起来。所有它的服务都应该严密的和该功能平行（功能平行，意味着没有依赖）

相应react组件：理论上一个组件应该只做一件事，如果要扩展，应该被拆分到更小的组件。

根据ui拆分组件是没有必要的，根据json数据拆分数据是合理的

DRY：Don't Repeat Yourself

辨别什么是一个state：

- 是否从父组件props传递，如果是，那不是state
- 是否一直不变，如果是，那不是state
- 是否能从其他state计算得到，如果是，那不是state

单向数据流：优势是数据流很清晰，缺点是交互的动态实现需要更多一点代码

使用ref的场景

- 处理焦点，区域选择，媒体播放
- 触发动画
- 配合第三方库

react diff 算法

> [普通树变动为什么是o(n3)的算法复杂度](http://grfia.dlsi.ua.es/ml/algorithms/references/editsurvey_bille.pdf)

react算法的两个设想

- 两个不同类型的tree会生成不同的树
- 通过不同的key可以得知在不同的渲染过程中哪个元素是不变的

diff 算法

**不同类型的元素**

根元素的不同类型会导致整体重绘

**相同类型的dom元素**

react会检查它们的属性是否相同，如果不相同，则仅仅更新它的属性，之后递归的对子元素进行操作

**子代的递归操作**
react按顺序比较
```
  <ul>
    <li>first</li>
    <li>second</li>
  </ul>

  <ul>
    <li>first</li>
    <li>second</li>
    <li>third</li>
  </ul>
  这种情况，react只会在末尾插入<li>third</li>
```
```
  <ul>
    <li>Duke</li>
    <li>Villanova</li>
  </ul>

  <ul>
    <li>Connecticut</li>
    <li>Duke</li>
    <li>Villanova</li>
  </ul>
  这种情况，react会更新所有元素
```

**keys**

keys可以解决上述的低效操作
```
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
这种情况，react会认为带有'2014'的key的元素是新的，其他元素仅仅是移动了
```

所以可以给数组元素加上等于下标顺序的key，但如果重排顺序的话，将会很慢

假定情形不使用的状况，算法就很吃力了

- 算法会忽略掉不同类型的组件，即使他们有相似的输出。不过在实践中，没有相关issue
- keys应该稳定，可预知，并且是独特的，不稳定的key会导致dom节点没有必要的重绘，并且容易丢失子组件的状态。


**高阶组件**

定义：高阶组件就是一个函数，接收一个组件，返回一个新组件
```
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```
高阶组件普遍存在于react第三方库中，例如：redux的connect和relay的createcontainer

```
class CommentList extends React.Component {
  constructor() {
    super();
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      // "DataSource" is some global data source
      comments: DataSource.getComments()
    };
  }

  componentDidMount() {
    // Subscribe to changes
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    // Clean up listener
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    // Update component state whenever the data source changes
    this.setState({
      comments: DataSource.getComments()
    });
  }

  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}

class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id)
    };
  }

  componentDidMount() {
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id)
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}

const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);

// This function takes a component...
function withSubscription(WrappedComponent, selectData) {
  // ...and returns another component...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... that takes care of the subscription...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```
> 不要改变原始组件，使用合成手段，不要在render方法中使用高阶组件

**合成事件**

合成事件的属性
```javascript
boolean bubbles
boolean cancelable
DOMEventTarget currentTarget
boolean defaultPrevented
number eventPhase
boolean isTrusted
DOMEvent nativeEvent
void preventDefault()
boolean isDefaultPrevented()
void stopPropagation()
boolean isPropagationStopped()
DOMEventTarget target
number timeStamp
string type
```