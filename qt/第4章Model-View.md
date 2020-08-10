```
data：数据源
model：模型，与源数据进行通信，并为视图组件提供接口，example：QStringListModel、QSqlTableModel
view：可视化界面，example：QlistView、QTreeView、QTableView
Delegate:代理类，代理负责从数据模型获取相应的数据，然后显示在编辑器中，修改数据后又将数据保存在模型中

```

### 文件系统源码

main.py

```python
import sys

from PyQt5 import QtWidgets
from PyQt5.QtCore import Qt, QDateTime, QTime, QDate, QTimer, QDir, QModelIndex
from PyQt5.QtWidgets import QMainWindow, QFileSystemModel

from untitled2 import Ui_MainWindow


class QMyWidget(QMainWindow):
    def __init__(self,parent = None):
        super().__init__(parent)
        self.__UI = Ui_MainWindow()
        self.__UI.setupUi(self)

        self.model = QFileSystemModel()
        self.model.setRootPath(QDir.currentPath())

        self.__UI.treeView.setModel(self.model)
        self.__UI.listView.setModel(self.model)
        self.__UI.tableView.setModel(self.model)
        self.__UI.treeView.clicked.connect(self.__UI.listView.setRootIndex)
        self.__UI.treeView.clicked.connect(self.__UI.tableView.setRootIndex)

        self.__UI.treeView.clicked.connect(self.click_set)
    def click_set(self,index):
        self.__UI.checkBox.setChecked(self.model.isDir(index))
        self.__UI.label_4.setText(self.model.filePath(index))
        self.__UI.label.setText(self.model.fileName(index))
        self.__UI.label_3.setText(self.model.type(index))
        filesize = self.model.size(index)/1024
        if filesize < 1024 :
            self.__UI.label_2.setText("%d KB"%filesize)
        else :
            self.__UI.label_2.setText("%0.2f MB"%(filesize/1024.0))
if __name__ == '__main__':
    app = QtWidgets.QApplication(sys.argv)
    MainWindow = QMyWidget()

    MainWindow.show()
    sys.exit(app.exec_())
```

untitled.ui

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ui version="4.0">
 <class>MainWindow</class>
 <widget class="QMainWindow" name="MainWindow">
  <property name="geometry">
   <rect>
    <x>0</x>
    <y>0</y>
    <width>653</width>
    <height>485</height>
   </rect>
  </property>
  <property name="windowTitle">
   <string>MainWindow</string>
  </property>
  <property name="layoutDirection">
   <enum>Qt::LeftToRight</enum>
  </property>
  <widget class="QWidget" name="centralwidget">
   <layout class="QHBoxLayout" name="horizontalLayout_3">
    <item>
     <widget class="QSplitter" name="splitter_3">
      <property name="orientation">
       <enum>Qt::Vertical</enum>
      </property>
      <widget class="QSplitter" name="splitter_2">
       <property name="orientation">
        <enum>Qt::Horizontal</enum>
       </property>
       <widget class="QTreeView" name="treeView"/>
       <widget class="QSplitter" name="splitter">
        <property name="orientation">
         <enum>Qt::Vertical</enum>
        </property>
        <widget class="QListView" name="listView"/>
        <widget class="QTableView" name="tableView"/>
       </widget>
      </widget>
      <widget class="QGroupBox" name="groupBox">
       <property name="title">
        <string>GroupBox</string>
       </property>
       <layout class="QHBoxLayout" name="horizontalLayout">
        <item>
         <widget class="QLabel" name="label">
          <property name="text">
           <string>TextLabel</string>
          </property>
         </widget>
        </item>
        <item>
         <widget class="QLabel" name="label_2">
          <property name="text">
           <string>TextLabel</string>
          </property>
         </widget>
        </item>
        <item>
         <widget class="QLabel" name="label_3">
          <property name="text">
           <string>TextLabel</string>
          </property>
         </widget>
        </item>
        <item>
         <widget class="QCheckBox" name="checkBox">
          <property name="text">
           <string>CheckBox</string>
          </property>
         </widget>
        </item>
       </layout>
      </widget>
      <widget class="QGroupBox" name="groupBox_3">
       <property name="title">
        <string>GroupBox</string>
       </property>
       <layout class="QHBoxLayout" name="horizontalLayout_2">
        <item>
         <widget class="QLabel" name="label_4">
          <property name="text">
           <string>TextLabel</string>
          </property>
         </widget>
        </item>
       </layout>
      </widget>
     </widget>
    </item>
   </layout>
  </widget>
 </widget>
 <resources/>
 <connections/>
</ui>
```



##### flag：

1. 对于列表和表格类型的数据模型，顶层节点总是用QModelIndex()表示
2. 

