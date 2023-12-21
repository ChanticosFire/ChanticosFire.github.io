---
layout: post
title: 数学期望计算器
category: 技术
tags:
  - Python
keywords: Python
---
（谁能想到搓个计算器的原因是出于打游戏呢

今天上CS2炼金，找了个许久之前用过的方子炼一炉黑龙Glock，出货率15%，果不其然的炸炉了，想了想当炼金术士这么久也没仔细算过期望值，都是大概估一眼就开了，于是决定搞搞这个，搓个计算器方便以后用。

数学期望值的计算公式为：
期望值=∑(产出值×产出概率)−成本

PyQt5的简单GUI，这种小打小闹就不往Github上再建个仓库了，直接贴blog。

```
#数学期望计算器
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QHBoxLayout, QLabel, QLineEdit, QPushButton, QGridLayout, QSpacerItem, QSizePolicy

class ExpectedValueCalculator(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.value_entries = []
        self.setGeometry(300, 300, 180, 210)  # 设置初始窗口大小和位置
        self.setWindowTitle('数学期望计算器')

        # 创建布局
        layout = QVBoxLayout()

        # 成本输入
        cost_layout = QHBoxLayout()
        cost_layout.addStretch()
        cost_layout.addWidget(QLabel('成本:'))
        self.cost_entry = QLineEdit(self)
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
        # 初始添加三组输入框

    def add_value_entry(self):
        probability_entry = QLineEdit(self)
        value_entry = QLineEdit(self)
        row = len(self.value_entries) + 2
        self.grid_layout.addWidget(probability_entry, row, 0)
        self.grid_layout.addWidget(value_entry, row, 1)
        self.value_entries.append((probability_entry, value_entry))

    def remove_value_entry(self):
        if len(self.value_entries) > 1:  # 确保至少保留一组输入框
            probability_entry, value_entry = self.value_entries.pop()
            probability_entry.deleteLater()
            value_entry.deleteLater()

    def calculate_expected_value(self):
        try:
            cost = float(self.cost_entry.text())
            total_probability = sum(float(entry[0].text()) for entry in self.value_entries)
            if total_probability != 0:
                adjustment_factor = 1.0 / total_probability
            else:
                adjustment_factor = 0
            total_value = sum(float(entry[1].text()) * float(entry[0].text()) * adjustment_factor for entry in self.value_entries)
            expected_value = total_value - cost
            self.result_label.setText(f'数学期望: {expected_value:.2f}')
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