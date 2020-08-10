```
data：数据源
model：模型，与源数据进行通信，并为视图组件提供接口，example：QStringListModel、QSqlTableModel
view：可视化界面，example：QlistView、QTreeView、QTableView
Delegate:代理类，代理负责从数据模型获取相应的数据，然后显示在编辑器中，修改数据后又将数据保存在模型中

```

##### flag：

1. 对于列表和表格类型的数据模型，顶层节点总是用QModelIndex()表示
2. 

