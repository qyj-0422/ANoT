# Python implementation of AnoT framework.

## 运行命令
~~~shell
python main.py -g YAGO
~~~

## 代码解读

### **1. `model.py`**
#### **核心功能**：规则图构建与管理
- **`Model` 类**：
  - **`__init__`**：初始化规则图、计数、张量等，对应论文中规则图的初始化逻辑（Section 4.3.3）。
  - **`add_rule`/`remove_rule`**：添加/删除原子规则或规则边，对应论文中规则图的动态更新机制（Section 4.4）。
  - **`build_rule_graph`**：构建规则依赖图，对应论文中规则图的构建算法（Algorithm 1）。
  - **`update`**：处理新事实并更新规则图，对应论文中更新器模块的核心逻辑（Section 4.4）。


### **2. `model_updater.py`**
#### **核心功能**：规则图在线更新
- **`ModelUpdater` 类**：
  - **`update_category`**：处理实体语义变化，对应论文中通过关系组合生成新类别的机制（Section 4.4.2）。
  - **`add_new_rules`**：添加新原子规则和规则边，对应论文中应对图模式变化的策略（Section 4.4.3）。
  - **`handle_new_entity`**：处理新实体的类别生成，对应论文中实体语义更新的逻辑（Section 4.4.2）。


### **3. `searcher.py`**
#### **核心功能**：规则图优化与搜索
- **`build_model` 函数**：
  - 实现规则候选的生成、排名和选择，对应论文中基于MDL原则的规则图优化算法（Algorithm 1，Section 4.3.3）。
  - 通过贪心策略选择最优规则，对应论文中规则选择的排序机制（Section 4.3.3）。


### **4. `evaluator.py`**
#### **核心功能**：模型评估与误差计算
- **`Evaluator` 类**：
  - **`evaluate`/`evaluate_temporal`**：计算描述长度 \(L(M) + L(G|M)\)，对应论文中基于MDL的模型评估指标（Section 4.2）。
  - **`length_rule`/`length_assertion`**：计算规则和断言的编码成本，对应论文中信息论编码长度的定义（Section 4.2.1-4.2.2）。
  - **`evaluate_change`**：评估规则添加后的误差变化，对应论文中规则选择的动态调整逻辑（Section 4.3.3）。


### **5. `temporal_model.py`**
#### **核心功能**：时间误差检测
- **`TemporalModel` 类**：
  - **`compute_temporal_score`**：计算时间分数，对应论文中基于规则图遍历的时间误差检测（Algorithm 2，Section 4.3.4）。
  - **`traverse_rule_graph`**：递归遍历规则图，对应论文中时间误差检测的递归策略（Section 4.3.4）。
  - **`update_timespan`**：更新规则边的时间跨度分布，对应论文中时间模式变化的处理（Section 4.4.3）。


### **6. `anomaly_detector.py`**
#### **核心功能**：异常检测流程
- **`AnomalyDetector` 类**：
  - **`detect`**：整合静态分数和时间分数，对应论文中异常分数的综合计算（Section 4.3.4）。
  - **`correct_prompt`**：生成错误修正提示，对应论文中解释性证据的提供（Section 4.3.4）。
  - **`monitor_error`**：监控负误差并触发规则图重建，对应论文中监视器模块的逻辑（Section 4.5）。


### **7. `rule.py`**
#### **核心功能**：规则定义与操作
- **`AtomicRule`/`RuleEdge` 类**：
  - 定义原子规则和规则边的结构，对应论文中原子规则和链/三元规则边的定义（Section 3.4.1-3.4.2）。
  - 处理规则的映射和关联，对应论文中规则图的构建基础（Section 4.3.2）。


### **8. `graph.py`**
#### **核心功能**：图结构管理
- **`TemporalGraph` 类**：
  - 维护时间知识图的结构，对应论文中TKG的存储与操作（Section 3.1）。
  - 提供图遍历和查询接口，支持规则图的构建与更新（Section 4.3.3）。


### **9. `main.py`**
#### **核心功能**：主程序入口
- 初始化探测器、更新器、监视器模块，调用训练和测试流程，对应论文中完整框架的整合（Section 4.1）。
- 处理数据集加载与参数配置，对应论文中实验设置（Section 5.1-5.2）。


### **10. `interface.ipynb`**
#### **核心功能**：交互式示例
- 提供代码使用示例，展示如何调用模块进行异常检测，对应论文中实验结果的复现指导（Section 5.3-5.7）。


### **11. `eval_function.py`**
#### **核心功能**：评估指标计算
- 实现精度、Fβ分数、AUC等指标，对应论文中实验评估协议（Section 5.2）。


### **12. `correct_assertion.py`**
#### **核心功能**：正确断言生成
- 处理规则的实例化与断言验证，对应论文中正确断言的编码成本计算（Section 4.2.2）。


### **13. `data/` 目录**
#### **功能**：数据集存储
- 包含实验使用的数据集（如ICEWS、YAGO），对应论文中数据集描述（Section 5.1）。


### **总结**
- **探测器模块**：`model.py`、`searcher.py`、`rule.py`（规则构建）。
- **更新器模块**：`model_updater.py`（动态更新）。
- **监视器模块**：`evaluator.py`、`anomaly_detector.py`（误差监控）。
- **时间误差检测**：`temporal_model.py`。
- **实验与评估**：`main.py`、`eval_function.py`、`interface.ipynb`。

每个代码文件紧密对应论文中提出的算法模块和实验流程，共同实现了可解释的在线时间知识图异常检测框架。