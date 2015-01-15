---
layout: post
title: Tableview 分类显示数据
date: 2015-01-15 12:00:00
---

Mark 记录一下。<br/>
{% highlight Objective-C %}
_favTableView = [[AlinkTableView alloc] initWithFrame: rect withStyle: UITableViewStyleGrouped];
NSString *plistPath = [[NSBundle mainBundle] pathForResource:@"aqicity" ofType:@"plist"];
NSMutableArray *unsortedArray = [[NSMutableArray alloc] initWithContentsOfFile:plistPath];
NSMutableArray *sortedArray = (NSMutableArray *)[unsortedArray sortedArrayUsingFunction: nickNameSort1
                                                                                context: nil];
_citys = [[NSMutableArray alloc] initWithArray: sortedArray];
[unsortedArray release];
_letterGroup = [[NSMutableDictionary alloc] init];
for(int index = 0; index<_citys.count; index ++){
    NSDictionary *dic = [_citys objectAtIndex: index];
    NSString *strname = [dic valueForKey: @"cityName"];
    NSString *header_ = [PinYinForObjc chineseConvertToPinYinHead: strname];
    char firstLetter = toupper([header_ characterAtIndex: 0]);//[ts.obj.name characterAtIndex:0]))
    if(firstLetter < 'A' || firstLetter > 'Z')
        firstLetter = '#';
		NSString* charKey = [NSString stringWithFormat:@"%c", firstLetter];
    [charKey UTF8String];
    NSMutableArray* grp = [_letterGroup objectForKey:charKey];
    if(grp == nil) {
        grp = [NSMutableArray new];
        [_letterGroup setObject:grp forKey:charKey];
    }
    [grp addObject: dic];
}
if(_sectionGroup == nil) {
    _sectionGroup = [NSMutableArray new];
}
[_sectionGroup removeAllObjects];
[_sectionGroup addObjectsFromArray:[_letterGroup allKeys]];
[_sectionGroup sortUsingSelector:@selector(compare:)];
{% endhighlight %}

{% highlight Objective-C %}
- (void)receive_letter_change: (NSNotification *)nt{
    [self _reloadData];
    NSDictionary *dict = nt.userInfo;
    NSString *letter = [dict valueForKey: @"AQI_LETTER_CHANGE"];
    if([_sectionGroup containsObject: letter]){
        //scroll to letter section
        int idn = [_sectionGroup indexOfObject: letter];
        NSIndexPath *idx = [NSIndexPath indexPathForRow: 0 inSection: idn];
        [_favTableView.tableView scrollToRowAtIndexPath: idx
                                       atScrollPosition: UITableViewScrollPositionTop
                                               animated: NO];
    }
}
{% endhighlight %}

{% highlight Objective-C %}
- (void)scrollViewDidScroll:(UIScrollView *)scrollView{
    NSArray *ar = [_favTableView.tableView indexPathsForVisibleRows];
    if(ar.count){
        NSIndexPath *indexPath = [ar objectAtIndex: 0];
        int section = indexPath.section;
        NSString *secName = [_sectionGroup objectAtIndex: section];
        _positionTitle.text = secName;
    }
}
{% endhighlight %}

{% highlight Objective-C %}
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
{
    if(_sectionGroup == nil) return(0);
    return(_sectionGroup.count);
}
{% endhighlight %}

{% highlight Objective-C %}
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
    NSInteger ct = self.citys.count;
    if(ct == 0){
        return 1;
    }
    else{
        //获取分组
        NSString *key = [_sectionGroup objectAtIndex:section];
        //获取分组里面的数组
        NSArray *array =[_letterGroup objectForKey:key];
        return [array count];
    }
    return 0;
}
{% endhighlight %}

{% highlight Objective-C %}
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
    return 55;
}
{% endhighlight %}

{% highlight Objective-C %}
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section {
    UIView *mview = [[UIView alloc] initWithFrame:CGRectMake(0, 0, tableView.frame.size.width, 20)];
    mview.backgroundColor = [UIColor grayColor];
    UILabel *mlabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 0, tableView.frame.size.width-10, 20)];
    mlabel.text = [self tableView:tableView titleForHeaderInSection:section];
    mlabel.textColor = [UIColor whiteColor];
    mlabel.backgroundColor = [UIColor clearColor];
    mlabel.font = [UIFont systemFontOfSize: 12.0];
    mlabel.shadowColor = [UIColor colorWithRed:0 green:0 blue:0 alpha:0.25];
    mlabel.shadowOffset = CGSizeMake(1, 1);
    [mview addSubview:mlabel];
    mlabel = nil;
    return mview;
}
{% endhighlight %}

{% highlight Objective-C %}
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section{
    return [_sectionGroup objectAtIndex: section];
}
{% endhighlight %}

{% highlight Objective-C %}
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section{
    return 20.0;
}
{% endhighlight %}