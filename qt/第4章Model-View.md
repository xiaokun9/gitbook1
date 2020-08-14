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

### QStringListModel
main.py
```
import sys

import PyQt5
from PyQt5.QtGui import QPainter, QColor, QPen, QIcon, QCursor
from PyQt5.QtWidgets import QApplication, QMainWindow, QListWidgetItem, QMenu, QWidget, QFileSystemModel, QDialog, \
  QAbstractItemView, QLabel
from untitled import *
from PyQt5.QtCore import QDateTime, QTimer, Qt, QPoint, QDir, QStringListModel
import time

class show(QMainWindow):
  def __init__(self):
    super().__init__()
    self._ui = Ui_MainWindow();
    self._ui.setupUi(self)

    self.model = QStringListModel()
    self._ui.listView.setModel(self.model)
    str = ["北京","上海","天津","河北","山东","四川","重庆","广东","河南"]
    self.model.setStringList(str)
    self._ui.listView.setEditTriggers(QAbstractItemView.DoubleClicked | QAbstractItemView.SelectedClicked)

    self._ui.pushButton_5.clicked.connect(self.clear)
    self._ui.pushButton_7.clicked.connect(self.show1)
    self._ui.pushButton_6.clicked.connect(self.clear_text)
    self._ui.pushButton_3.clicked.connect(self.inseart)
    self._ui.pushButton_4.clicked.connect(self.clearcur)
    self.label = QLabel()
    self._ui.statusbar.addWidget(self.label)
    self._ui.listView.clicked.connect(self.test)
  def test(self,index):
    print(index.row())
    self.label.setText("%s"%index.data())
  def clearcur(self):
    index = self._ui.listView.currentIndex()
    self.model.removeRow(index.row())
  def inseart(self):
    index = self._ui.listView.currentIndex()
    self.model.insertRow(index.row())
    self.model.setData(index,"inseart",Qt.DisplayRole)
    self.model.setData(index,Qt.AlignRight,Qt.TextAlignmentRole)
    self._ui.listView.setCurrentIndex(index)
  def show1(self):
    self._ui.plainTextEdit.clear()
    index = self.model.rowCount()
    for i in range(index):
      self._ui.plainTextEdit.appendPlainText(self.model.stringList()[i])
  def clear(self):
    self.model.setStringList([])
  def clear_text(self):
    self._ui.plainTextEdit.clear()
if __name__ == '__main__':
  app = QApplication(sys.argv)
  ui = show()

  ui.show()
  sys.exit(app.exec_())
```

untitled.ui
```
<?xml version="1.0" encoding="UTF-8"?>
<ui version="4.0">
 <class>MainWindow</class>
 <widget class="QMainWindow" name="MainWindow">
  <property name="geometry">
   <rect>
    <x>0</x>
    <y>0</y>
    <width>630</width>
    <height>521</height>
   </rect>
  </property>
  <property name="windowTitle">
   <string>MainWindow</string>
  </property>
  <widget class="QWidget" name="centralwidget">
   <layout class="QFormLayout" name="formLayout_2">
    <item row="0" column="0">
     <widget class="QGroupBox" name="groupBox">
      <property name="title">
       <string>QListView</string>
      </property>
      <layout class="QFormLayout" name="formLayout">
       <item row="0" column="0">
        <widget class="QPushButton" name="pushButton">
         <property name="text">
          <string>恢复列表</string>
         </property>
        </widget>
       </item>
       <item row="1" column="0">
        <widget class="QPushButton" name="pushButton_2">
         <property name="text">
          <string>添加项</string>
         </property>
        </widget>
       </item>
       <item row="1" column="1">
        <widget class="QPushButton" name="pushButton_3">
         <property name="text">
          <string>插入项</string>
         </property>
        </widget>
       </item>
       <item row="2" column="0">
        <widget class="QPushButton" name="pushButton_4">
         <property name="text">
          <string>删除当前项</string>
         </property>
        </widget>
       </item>
       <item row="2" column="1">
        <widget class="QPushButton" name="pushButton_5">
         <property name="text">
          <string>清除列表</string>
         </property>
        </widget>
       </item>
       <item row="3" column="0" colspan="2">
        <widget class="QListView" name="listView"/>
       </item>
      </layout>
     </widget>
    </item>
    <item row="0" column="1">
     <widget class="QGroupBox" name="groupBox_2">
      <property name="title">
       <string>QPlainTextEdit</string>
      </property>
      <layout class="QFormLayout" name="formLayout_3">
       <item row="0" column="0">
        <widget class="QPushButton" name="pushButton_6">
         <property name="text">
          <string>清空文本</string>
         </property>
        </widget>
       </item>
       <item row="1" column="0">
        <widget class="QPushButton" name="pushButton_7">
         <property name="text">
          <string>显示数据模型的stringlist</string>
         </property>
        </widget>
       </item>
       <item row="2" column="0">
        <widget class="QPlainTextEdit" name="plainTextEdit"/>
       </item>
      </layout>
     </widget>
    </item>
   </layout>
  </widget>
  <widget class="QStatusBar" name="statusbar"/>
 </widget>
 <resources/>
 <connections/>
</ui>

```


