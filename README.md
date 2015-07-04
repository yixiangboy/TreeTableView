# TreeTableView

![](https://github.com/yixiangboy/TreeTableView/blob/master/show.gif)


##使用说明
这是一个使用非递归方法，写的高扩展性，高复用性的IOS树形结构。支持多级树形结构。<br/>
代码简单易懂，纯代码编写。<br/>
下面教你如何使用该树形控件。<br/>

###第一步：建立数据模型

```

/**
*  每个节点类型
*/
@interface Node : NSObject

@property (nonatomic , assign) int parentId;//父节点的id，如果为-1表示该节点为根节点

@property (nonatomic , assign) int nodeId;//本节点的id

@property (nonatomic , strong) NSString *name;//本节点的名称

@property (nonatomic , assign) int depth;//该节点的深度

@property (nonatomic , assign) BOOL expand;//该节点是否处于展开状态

/**
*快速实例化该对象模型
*/
- (instancetype)initWithParentId : (int)parentId nodeId : (int)nodeId name : (NSString *)name depth : (int)depth expand : (BOOL)expand;

@end
```




###第二步：按照以上的数据模型，组装数据，下面以 国家-身份-城市 的三级目录进行演示。

```

//----------------------------------中国的省地市关系图3,2,1--------------------------------------------
Node *country1 = [[Node alloc] initWithParentId:-1 nodeId:0 name:@"中国" depth:0 expand:YES];
Node *province1 = [[Node alloc] initWithParentId:0 nodeId:1 name:@"江苏" depth:1 expand:NO];
Node *city1 = [[Node alloc] initWithParentId:1 nodeId:2 name:@"南通" depth:2 expand:NO];
//Node *subCity1 = [[Node alloc] initWithParentId:2 nodeId:100 name:@"通州" depth:3 expand:NO];
//Node *subCity2 = [[Node alloc] initWithParentId:2 nodeId:101 name:@"如东" depth:3 expand:NO];
Node *city2 = [[Node alloc] initWithParentId:1 nodeId:3 name:@"南京" depth:2 expand:NO];
Node *city3 = [[Node alloc] initWithParentId:1 nodeId:4 name:@"苏州" depth:2 expand:NO];
Node *province2 = [[Node alloc] initWithParentId:0 nodeId:5 name:@"广东" depth:1 expand:NO];
Node *city4 = [[Node alloc] initWithParentId:5 nodeId:6 name:@"深圳" depth:2 expand:NO];
Node *city5 = [[Node alloc] initWithParentId:5 nodeId:7 name:@"广州" depth:2 expand:NO];
Node *province3 = [[Node alloc] initWithParentId:0 nodeId:8 name:@"浙江" depth:1 expand:NO];
Node *city6 = [[Node alloc] initWithParentId:8 nodeId:9 name:@"杭州" depth:2 expand:NO];
//----------------------------------美国的省地市关系图0,1,2--------------------------------------------
Node *country2 = [[Node alloc] initWithParentId:-1 nodeId:10 name:@"美国" depth:0 expand:YES];
Node *province4 = [[Node alloc] initWithParentId:10 nodeId:11 name:@"纽约州" depth:1 expand:NO];
Node *province5 = [[Node alloc] initWithParentId:10 nodeId:12 name:@"德州" depth:1 expand:NO];
Node *city7 = [[Node alloc] initWithParentId:12 nodeId:13 name:@"休斯顿" depth:2 expand:NO];
Node *province6 = [[Node alloc] initWithParentId:10 nodeId:14 name:@"加州" depth:1 expand:NO];
Node *city8 = [[Node alloc] initWithParentId:14 nodeId:15 name:@"洛杉矶" depth:2 expand:NO];
Node *city9 = [[Node alloc] initWithParentId:14 nodeId:16 name:@"旧金山" depth:2 expand:NO];

//----------------------------------日本的省地市关系图0,1,2--------------------------------------------
Node *country3 = [[Node alloc] initWithParentId:-1 nodeId:17 name:@"日本" depth:0 expand:YES];
```



###第三步：使用以上数据进行TeeTableView的初始化。

```
NSArray *data = [NSArray arrayWithObjects:country1,province1,city1,city2,city3,province2,city4,city5,province3,city6,country2,province4,province5,city7,province6,city8,city9,country3, nil];


TreeTableView *tableview = [[TreeTableView alloc] initWithFrame:CGRectMake(0, 20, CGRectGetWidth(self.view.frame), CGRectGetHeight(self.view.frame)-20) withData:data];
[self.view addSubview:tableview];
```


###扩展说明：

通过简单以上三步，你就可以把该树形控件集成到你的项目中。<br/>
在演示项目中，每个cell我都使用系统自带的cell，样式比较简单，如果你要展现更加漂亮的样式，可以自定义cell。<br/>
同时，你也可以扩展该数据模型，运动到更加复杂的业务处理中。比如以下场景：<br/>

![](https://github.com/yixiangboy/TreeTableView/blob/master/show2.gif)



