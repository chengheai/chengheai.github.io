---
title: react+antd+mongodb+express实现增删查改
date: 2018-12-10 13:17:07
tags: [antd, mongodb, dvajs, nodejs, react]
---
## 前言：
之前有一段时间做过react + antd的项目，还挺熟，但是现在在公司一直用的是vue，随着时间的推移，记忆也慢慢消失，所有趁工作之余的休息时间来写了这个小例子，实现本地起服务，调本地mongo数据，当然，你也可以放线上或者VPS上预览。\[demo](http://144.34.148.126:10000/) ;
另外也有一个vue+ element 的项目 \[GitHub](https://github.com/chengheai/mongodb-vue)
<font color=#EC6E3A size=4 face=" 华文行楷">
\[完整代码](https://github.com/chengheai/mongo-react) ;欢迎👏 star follow
</font>
<font color=#3421B3 size=4 face=" 华文行楷">
如有问题请在GitHub上留言，谢谢！
</font>
### 效果图
![](https://github.com/chengheai/review-demo-image/blob/master/2018-12-10%2014-30-31.2018-12-10%2014_31_55.gif?raw=true)
## 登录
* 使用sessionStorage在登录的时候setItem用户名，然后在components下Header.jsx去getItem用户名
## 主要代码
``` javascript
  handleSubmit = (e) => {
    const { dispatch } = this.props;
    e.preventDefault();
    this.props.form.validateFields((err, values) => {
      if (!err) {
        dispatch(routerRedux.replace('/list'));
        console.log('Received values of form: ', values);
        sessionStorage.setItem('guest', values.userName)
      }
    });
  }
```
## 添加与编辑，添加图片
![](https://github.com/chengheai/review-demo-image/blob/master/2018-12-10%2014-41-32.2018-12-10%2014_49_11.gif?raw=true)
![](https://github.com/chengheai/review-demo-image/blob/master/2018-12-10%2014-51-07.2018-12-10%2014_59_38.gif?raw=true)

* 编辑与添加共用一个抽屉，加一个types用来区别是编辑还是添加
## 主要代码
``` javascript
  handleSubmit = (e) => {
    let that = this;
    const { dispatch } = this.props;
    const { types, editForm } = this.state;
    e.preventDefault();
    if(types === 2){
      this.props.form.validateFields((err, values) => {
        values = Object.assign(editForm, values);
      if (!err) {
        // message.loading('正在添加...');
        dispatch({
          type: 'heroModel/put_heros',
          payload: values,
          callback: (res) => {
            message.success('修改成功');
            this.setState({
              visible: false,
              editId: ''
            });
            this.props.form.resetFields();
            that.getData();
          }
        })
      }
    });
    } else {
      this.props.form.validateFields((err, values) => {
        if (!err) {
          // message.loading('正在添加...');
          dispatch({
            type: 'heroModel/post_hero',
            payload: values,
            callback: (res) => {
              message.success('添加成功');
              this.setState({
                visible: false,
              });
              this.props.form.resetFields();
              that.getData();
            }
          })
        }
      });
    }
  }
```
## 分页与删除
![](https://github.com/chengheai/review-demo-image/blob/master/2018-12-10%2015-08-45.2018-12-10%2015_14_15.gif?raw=true)
## 主要代码
server.js
``` javascript
  router.get("/hero", (req, res) => {
  // console.log('========',req.query.pageSize)
  // console.log('+++++++++++++',Hero.count())
  var total = 0;
  Hero.count({}, function(err, count){
    if(err) return;
    total = count;
    res.set('x-header', total)
  })

  Hero.find({})
    .limit(Math.min(parseInt(req.query.pageSize) || 10, 100))
    .skip(parseInt(req.query.currentPage -1) * req.query.pageSize || 0)
    .sort({ updatedAt: -1 })
    .then(heros => {
      res.json(heros);
    })
    .catch(err => {
      res.json(err);
    })

});
```
list.js
``` javascript
  handleTableChange = (pagination, filters, sorter) => {
    console.log(pagination);
    // let that = this;
    const { pageSize, current } = pagination;
    const { dispatch } = this.props;
    const { payload } = this.state;
    this.setState({
      pagination: {
        currentPage: current,
        pageSize,
      },
    });
    const query = {
      ...payload,
      pageSize,
      currentPage: current
    };
    dispatch({
      type: 'heroModel/get_heros',
      payload: query,
      callback: res => {
        this.setState({
          tableData: res.data,
          // eslint-disable-next-line
          total: parseInt(res.headers['x-header'])
        })
      }
    });
  }
```
