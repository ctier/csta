# 1. 事件结构文档 v0.0.1

本次修改目标：

- 尽量不删除原有字段
- 尽量在事件上增加客户需要某些字段

# 2. 事件名及其数据结构
## 2.1. 座席状态事件
### 2.1.1. agentLoggedOn：座席登录事件
### 2.1.2. agentLoggedOff：座席登出事件
### 2.1.3. agentReady：座席就绪事件
### 2.1.4. agentWorkingAfterCall：座席话后处理事件
### 2.1.5. agentAllocated：座席预占事件
### 2.1.6. agentNotReady：座席离席事件

## 2.2. 呼叫事件
## 2.3. serviceInitiated：摘机事件
## 2.4. originated：呼出事件
## 2.5. delivered：振铃事件
## 2.6. established：接通事件
## 2.7. connectionCleared：呼叫挂断事件
## 2.8. transferred：转移事件
## 2.9. conferenced：会议事件
## 2.10. retrieved：取回事件
## 2.11. held：保持事件


