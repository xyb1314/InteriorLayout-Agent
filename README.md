# InteriorLayout-Agent 🏠

一个基于多智能体（Multi-Agent）系统的室内布局设计工具，使用AutoGen框架实现智能化的室内空间规划和家具布局。

## 🎯 项目概述

InteriorLayout-Agent是一个创新的室内设计系统，通过多个专业智能体的协作，自动生成符合用户偏好的室内布局方案。系统能够理解空间约束、用户需求，并生成最优的家具摆放方案。

## 🤖 核心Agent架构

### 主要设计Agent

#### 1. **Interior Designer (室内设计师)**
- **职责**: 根据用户偏好和房间功能，建议需要添加的家具对象
- **输出**: 包含对象名称、建筑风格、材料、尺寸和数量的详细建议
- **特点**: 专注于美学和功能性，不涉及门窗相关物品

#### 2. **Interior Architect (室内建筑师)**
- **职责**: 分析用户偏好，为每个家具对象找到最佳放置位置
- **功能**: 
  - 空间布局规划
  - 相对位置关系确定
  - 朝向分析（东南西北墙）
  - 邻近关系评估（相邻/不相邻）

#### 3. **Engineer (工程师)**
- **职责**: 整合设计师和建筑师的建议，生成标准化的JSON格式输出
- **特点**: 严格遵循JSON Schema，确保数据格式的一致性

### 验证与纠错Agent

#### 4. **JSON Schema Debugger (JSON模式调试器)**
- **职责**: 验证所有Agent输出的JSON格式是否符合预定义的模式
- **功能**: 提供详细的错误反馈和修正建议
- **终止条件**: 当验证成功时自动终止对话

### 布局优化Agent

#### 5. **Layout Refiner (布局优化器)**
- **职责**: 优化现有布局，改进对象间的相对位置关系
- **功能**: 为子对象找到第二合适的位置，考虑初始定位

#### 6. **Spatial Corrector (空间纠正器)**
- **职责**: 解决空间冲突，调整对象位置以消除布局问题
- **功能**: 修改场景图和面向对象关系

#### 7. **Object Deletion Agent (对象删除代理)**
- **职责**: 当空间不足时，智能选择删除非必要的家具对象
- **决策**: 基于房间功能重要性进行对象优先级排序

## 🔧 技术特性

- **多智能体协作**: 使用AutoGen框架实现智能体间的有效通信
- **JSON Schema验证**: 严格的数据格式控制，确保输出质量
- **空间冲突检测**: 自动识别和解决布局中的空间冲突
- **模块化设计**: 每个Agent职责明确，易于维护和扩展
- **Blender集成**: 支持将设计结果导入Blender进行3D可视化

## 📁 项目结构

```
InteriorLayout-Agent/
├── agents.py              # 核心Agent定义
├── corrector_agents.py    # 纠错和验证Agent
├── refiner_agents.py      # 布局优化Agent
├── IDesign.py             # 主要设计流程控制
├── chats.py               # 聊天管理模块
├── schemas.py             # JSON Schema定义
├── utils.py               # 工具函数
├── place_in_blender.py    # Blender集成
└── test.py                # 测试示例
```

## 🚀 快速开始

### 环境要求
- Python 3.7+
- AutoGen框架
- OpenAI API密钥（配置在`LLM_CONFIG_LIST.json`中）

### 基本使用

```python
from IDesign import IDesign

# 创建设计实例
i_design = IDesign(
    no_of_objects=15,                    # 家具对象数量
    user_input="A creative vibrant livingroom",  # 用户偏好
    room_dimensions=[4.0, 4.0, 2.5]     # 房间尺寸 [长, 宽, 高]
)

# 执行完整设计流程
i_design.create_initial_design()         # 初始设计
i_design.correct_design()                # 纠错优化
i_design.refine_design(verbose=True)     # 布局优化
i_design.create_object_clusters()        # 对象聚类
i_design.backtrack(verbose=True)         # 回溯优化
i_design.to_json()                       # 输出结果
```

## 🔄 工作流程

1. **初始设计阶段**: Interior Designer + Interior Architect协作生成初始方案
2. **验证阶段**: JSON Schema Debugger验证输出格式
3. **纠错阶段**: Spatial Corrector解决空间冲突
4. **优化阶段**: Layout Refiner优化对象位置关系
5. **聚类阶段**: 将相关对象组织成逻辑组
6. **最终输出**: 生成标准化的JSON格式设计文件

## 🎨 设计特点

- **智能空间规划**: 考虑房间尺寸、功能和用户偏好
- **冲突自动解决**: 自动检测和解决空间布局冲突
- **多风格支持**: 支持现代、经典等多种建筑风格
- **材料多样性**: 支持木材、金属等多种材料选择
- **精确尺寸控制**: 使用米制单位，精确到0.1米

## 🔮 未来扩展

- 支持更多房间类型和功能
- 集成更多设计风格和材料选项
- 增强3D可视化能力
- 支持实时协作设计


