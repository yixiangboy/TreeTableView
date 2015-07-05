# TreeTableView
===
##一、简介

树形控件在多列列表、多级菜单中使用比较常见，比如：国家-省份-城市 多级选择、学校-专业-班级 多级选择等等。然而IOS自带控件中并不存在树形控件，我们要在IOS开发中使用树形控件，通常需要自己扩展UITableView列表控件。<br/>现在在这里开源一个自己写的高扩展性，高复用性的IOS树形结构控件。<br/>支持无限极树形结构。<br/>使用的是非递归方式。<br/>代码简单易懂，扩展方便。<br/>图片演示如下：
![项目演示](http://img.my.csdn.net/uploads/201507/05/1436079831_2869.gif)
<br/>
<br/>
##二、使用说明
###第一步：建立数据模型
parentId ： 该节点的父控件id号，如果为-1则表示该节点为根节点<br/>
nodeId ： 每个节点自身的id号，是每个节点的唯一标示<br/>
name ： 节点的名称<br/>
depth ： 该节点所带的树形结构中的深度，根节点的深度为0<br/>
expand ： 该节点是否处于展开状态<br/>
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
NSArray *data = [NSArray arrayWithObjects:country1,province1,city1,city2,city3,province2,city4,city5,province3,city6,country2,province4,province5,city7,province6,city8,city9,country3, nil];
```
###第三步：使用以上数据进行TeeTableView的初始化。

```
TreeTableView *tableview = [[TreeTableView alloc] initWithFrame:CGRectMake(0, 20, CGRectGetWidth(self.view.frame), CGRectGetHeight(self.view.frame)-20) withData:data];
[self.view addSubview:tableview];
```
通过简单以上三步，你就可以把该树形控件集成到你的项目中。
<br/>
<br/>
##三、实现原理
树形结构的列表用的其实就是UITableView控件，但是如何能够让UItableView能够动态的增加和删除指定的行数的cell是实现树形结构的关键所在。
这时候我们需要用到两个UItableView自带的行数：
```
- (void)insertRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
- (void)deleteRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
```
第一个函数用来在指定的位置插入cells，第二个函数用来在指定的位置删除cells，并且这二个函数都自带多种动画效果，让删除和插入的过程不至于太突兀、有种渐变的感觉，具有良好的用户体验。
对于这几个动画做了尝试：
UITableViewRowAnimationFade ： 渐变效果
UITableViewRowAnimationRight ： 右边进入，右边消失
UITableViewRowAnimationLeft ： 左边进入，左边消失
UITableViewRowAnimationTop ： 顶部进入，顶部消失
UITableViewRowAnimationBottom ： 顶部进入，底部消失

###<font color=red>注意点：</font>
在调用insertRowsAtIndexPaths和deleteRowsAtIndexPaths的时候一定要先改变数据源，在调用上述函数，不然会产生crash。

接下来把TreeTableView的主要代码展示出来，因为本来代码量就不大，而且代码中注释也比较全，希望能够帮助大家理解。

```
#import "TreeTableView.h"
#import "Node.h"

@interface TreeTableView ()<UITableViewDataSource,UITableViewDelegate>

@property (nonatomic , strong) NSArray *data;//传递过来已经组织好的数据（全量数据）

@property (nonatomic , strong) NSMutableArray *tempData;//用于存储数据源（部分数据）


@end

@implementation TreeTableView

-(instancetype)initWithFrame:(CGRect)frame withData : (NSArray *)data{
self = [super initWithFrame:frame style:UITableViewStyleGrouped];
if (self) {
self.dataSource = self;
self.delegate = self;
_data = data;
_tempData = [self createTempData:data];
}
return self;
}

/**
* 初始化数据源
*/
-(NSMutableArray *)createTempData : (NSArray *)data{
NSMutableArray *tempArray = [NSMutableArray array];
for (int i=0; i<data.count; i++) {
Node *node = [_data objectAtIndex:i];
if (node.expand) {
[tempArray addObject:node];
}
}
return tempArray;
}


#pragma mark - UITableViewDataSource

#pragma mark - Required

-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
return _tempData.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
static NSString *NODE_CELL_ID = @"node_cell_id";

UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:NODE_CELL_ID];
if (!cell) {
cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:NODE_CELL_ID];
}

Node *node = [_tempData objectAtIndex:indexPath.row];

NSMutableString *name = [NSMutableString string];
for (int i=0; i<node.depth; i++) {
[name appendString:@"     "];
}
[name appendString:node.name];

cell.textLabel.text = name;

return cell;
}


#pragma mark - Optional
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section{
return 0.01;
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
return 40;
}

- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section{
return 0.01;
}

#pragma mark - UITableViewDelegate

#pragma mark - Optional
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath{
//先修改数据源
Node *parentNode = [_tempData objectAtIndex:indexPath.row];
NSUInteger startPosition = indexPath.row+1;
NSUInteger endPosition = startPosition;
BOOL expand = NO;
for (int i=0; i<_data.count; i++) {
Node *node = [_data objectAtIndex:i];
if (node.parentId == parentNode.nodeId) {
node.expand = !node.expand;
if (node.expand) {
[_tempData insertObject:node atIndex:endPosition];
expand = YES;
}else{
expand = NO;
endPosition = [self removeAllNodesAtParentNode:parentNode];
break;
}
endPosition++;
}
}

//获得需要修正的indexPath
NSMutableArray *indexPathArray = [NSMutableArray array];
for (NSUInteger i=startPosition; i<endPosition; i++) {
NSIndexPath *tempIndexPath = [NSIndexPath indexPathForRow:i inSection:0];
[indexPathArray addObject:tempIndexPath];
}

//插入或者删除相关节点
if (expand) {
[self insertRowsAtIndexPaths:indexPathArray withRowAnimation:UITableViewRowAnimationNone];
}else{
[self deleteRowsAtIndexPaths:indexPathArray withRowAnimation:UITableViewRowAnimationNone];
}
}

/**
*  删除该父节点下的所有子节点（包括孙子节点）
*
*  @param parentNode 父节点
*
*  @return 邻接父节点的位置距离该父节点的长度，也就是该父节点下面所有的子孙节点的数量
*/
-(NSUInteger)removeAllNodesAtParentNode : (Node *)parentNode{
NSUInteger startPosition = [_tempData indexOfObject:parentNode];
NSUInteger endPosition = startPosition;
for (NSUInteger i=startPosition+1; i<_tempData.count; i++) {
Node *node = [_tempData objectAtIndex:i];
endPosition++;
if (node.depth == parentNode.depth) {
break;
}
node.expand = NO;
}
if (endPosition>startPosition) {
[_tempData removeObjectsInRange:NSMakeRange(startPosition+1, endPosition-startPosition-1)];
}
return endPosition;
}
```

<br/>
##四、总结
在演示项目中，每个cell我都使用系统自带的cell，样式比较简单，如果你要展现更加漂亮的样式，可以自定义cell。<br/>
同时，你也可以扩展该数据模型，运动到更加复杂的业务处理中。比如以下场景：
![这里写图片描述](http://img.my.csdn.net/uploads/201507/05/1436079894_7992.gif)
<br/>
##五、下载地址
github下载地址：[https://github.com/yixiangboy/TreeTableView](https://github.com/yixiangboy/TreeTableView)<br/>
如果觉得对你还有些用，给一颗star吧。你的支持是我继续的动力。
<br/>
##五、博主的话
以前看过很多别人的博客，学到不少东西。现在准备自己也开始谢谢博客，希望能够帮到一些人。如果工作不是特别忙的话，准备每周写一篇。<br/>
我的联系方式：[新浪微博](http://weibo.com/p/1005055612984599/home?from=page_100505&mod=TAB#place)
