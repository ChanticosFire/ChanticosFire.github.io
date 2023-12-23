---
layout: post
title: 数学期望计算器
category: 技术
tags:
  - Python
keywords: Python
---
2023/12/23
更新，添加清除按钮，百分比，快捷键操作，修改初始窗口大小。

2023/12/22
嫌整包打包**Qt5**太大，换**Tkinter**折腾了半天，调窗口大小和缩放，效果太差，不换了。

2023/12/21
（谁能想到搓个计算器的原因是出于打游戏呢

今天上CS2炼金，找了个许久之前用过的方子炼一炉黑龙Glock，出货率15%，果不其然的炸炉了，想了想当炼金术士这么久也没仔细算过期望值，都是大概估一眼就开了，于是决定搞搞这个，搓个计算器方便以后用。

[关于数学期望：▷ 数学期望（或期望值） (statorials.org)](https://statorials.org/cn/%e6%95%b0%e5%ad%a6%e6%9c%9f%e6%9c%9b%e6%88%96%e6%9c%9f%e6%9c%9b%e5%80%bc/)

放到炼金这里 计算公式就是：
期望值=∑(产出值×产出概率)−成本

PyQt5的简单GUI，这种小打小闹就不往Github上再建个仓库了，直接贴blog。

```python
import sys  
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QHBoxLayout, QLabel, QLineEdit, QPushButton, QGridLayout  
from PyQt5.QtCore import Qt  
  
class CustomLineEdit(QLineEdit):  
def keyPressEvent(self, event):  
if event.key() in [Qt.Key_Plus, Qt.Key_Minus]:  
# 将按键事件传递给主窗口处理  
self.parent().keyPressEvent(event)  
else:  
# 对于其他按键事件，使用默认处理  
super().keyPressEvent(event)  
  
class ExpectedValueCalculator(QWidget):  
def __init__(self):  
super().__init__()  
self.initUI()  
  
def initUI(self):  
self.value_entries = []  
self.setGeometry(300, 300, 520, 412)  
self.setWindowTitle('数学期望计算器')  
  
# 创建布局  
layout = QVBoxLayout()  
  
# 成本输入  
cost_layout = QHBoxLayout()  
cost_layout.addStretch()  
cost_layout.addWidget(QLabel('成本:'))  
self.cost_entry = CustomLineEdit(self)  
cost_layout.addWidget(self.cost_entry)  
cost_layout.addStretch()  
layout.addLayout(cost_layout)  
  
# 几率和产出价值输入  
self.grid_layout = QGridLayout()  
self.grid_layout.addWidget(QLabel('几率%'), 0, 0)  
self.grid_layout.addWidget(QLabel('产出价值'), 0, 1)  
layout.addLayout(self.grid_layout)  
  
# 添加和删除按钮布局  
btn_layout = QHBoxLayout()  
btn_layout.addStretch()  
clear_btn = QPushButton('清除', self)  
clear_btn.clicked.connect(self.clear_entries)  
btn_layout.addWidget(clear_btn)  
add_btn = QPushButton('+', self)  
add_btn.clicked.connect(self.add_value_entry)  
btn_layout.addWidget(add_btn)  
remove_btn = QPushButton('-', self)  
remove_btn.clicked.connect(self.remove_value_entry)  
btn_layout.addWidget(remove_btn)  
btn_layout.addStretch()  
layout.addLayout(btn_layout)  
  
# 计算按钮和结果显示  
calc_layout = QHBoxLayout()  
calc_layout.addStretch()  
calc_btn = QPushButton('计算', self)  
calc_btn.clicked.connect(self.calculate_expected_value)  
calc_layout.addWidget(calc_btn)  
calc_layout.addStretch()  
layout.addLayout(calc_layout)  
  
self.result_label = QLabel('数学期望: 0')  
layout.addWidget(self.result_label)  
  
self.setLayout(layout)  
self.add_value_entry()  
self.add_value_entry()  
self.add_value_entry()  
  
def keyPressEvent(self, event):  
if event.key() == Qt.Key_Plus:  
self.add_value_entry()  
elif event.key() == Qt.Key_Minus:  
self.remove_value_entry()  
elif event.key() == Qt.Key_Return:  
self.calculate_expected_value()  
elif event.key() == Qt.Key_Escape:  
self.clear_entries()  
  
def add_value_entry(self):  
probability_entry = CustomLineEdit(self)  
value_entry = CustomLineEdit(self)  
row = len(self.value_entries) + 2  
self.grid_layout.addWidget(probability_entry, row, 0)  
self.grid_layout.addWidget(value_entry, row, 1)  
self.value_entries.append((probability_entry, value_entry))  
  
def remove_value_entry(self):  
if len(self.value_entries) > 1:  
probability_entry, value_entry = self.value_entries.pop()  
probability_entry.deleteLater()  
value_entry.deleteLater()  
  
def clear_entries(self):  
self.cost_entry.clear()  
for probability_entry, value_entry in self.value_entries:  
probability_entry.clear()  
value_entry.clear()  
self.result_label.setText('数学期望: 0')  
  
def calculate_expected_value(self):  
try:  
cost = float(self.cost_entry.text())  
total_value = sum(float(entry[1].text()) * float(entry[0].text()) / 100 for entry in self.value_entries)  
expected_value = total_value - cost  
percentage_value = (expected_value / cost) * 100 if cost != 0 else 0  
self.result_label.setText(f'数学期望: {expected_value:.2f} ({percentage_value:.2f}%)')  
except ValueError as e:  
self.result_label.setText(f'错误: {e}')  
  
def main():  
app = QApplication(sys.argv)  
ex = ExpectedValueCalculator()  
ex.show()  
sys.exit(app.exec_())  
  
if __name__ == '__main__':  
main()
```