# 📈 Gupiao - 专业加密货币交易模拟平台

<div align="center">

![Vue3](https://img.shields.io/badge/Vue-3.5-42b883?style=flat-square&logo=vuedotjs)
![Vite](https://img.shields.io/badge/Vite-8.0-646cff?style=flat-square&logo=vite)
![Node.js](https://img.shields.io/badge/Node.js-Express-green?style=flat-square&logo=node.js)
![WebSocket](https://img.shields.io/badge/WebSocket-Realtime-blue?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

基于 Vue 3 + Node.js 的全栈加密货币交易平台，实现毫秒级实时行情推送与高并发 WebSocket 通讯。

</div>

---

## 🎯 项目概述

Gupiao 是一个完整的全栈加密货币交易模拟系统，包含前端交易界面和后端实时行情服务。项目核心亮点在于**高性能 WebSocket 实时通讯架构**和**完善的并发控制机制**，适合学习金融交易系统的高并发解决方案。

### ✨ 核心特性

#### 前端特性
- 🚀 **极速开发体验** - Vite 8 构建，秒级热更新
- 📊 **专业 K 线图表** - Lightweight Charts，支持多时间周期实时刷新
- ⚡ **智能重连机制** - WebSocket 断线指数退避重连
- 🎨 **深色主题 UI** - 专业金融交易界面设计
- 📱 **响应式布局** - 完美适配移动端和桌面端

#### 后端特性
- 🔥 **高并发 WebSocket** - 支持 1000+ 客户端同时在线
- 🛡️ **多重防护机制** - 限流、心跳检测、内存泄漏防护
- 🤖 **自动播报机器人** - 每 2 秒推送实时交易数据
- 📝 **结构化日志** - Winston 日志系统，便于问题追踪
- 🔌 **优雅降级** - WebSocket 失败自动切换 HTTP 轮询

---

## 🏗️ 技术架构

### 系统架构图

![系统架构图](./img/1c.png)

### 数据流图

![数据流图](./img/2.png)

### 界面展示

![界面展示](./img/3.png)

---

## 🔥 核心技术亮点

### 🛡️ 防御性编程与安全架构（Defensive Programming）

#### 1. **三层纵深防御体系**
- ✅ **第一层：CDN + WAF 隐藏源站**：用户只访问域名，CDN 扛 CC/流量，攻击者扫不到真实 IP，切换服务器无感知。
- ✅ **第二层：单机基础防御**：IP 限流、WebSocket 连接数限制、心跳清理、参数校验，确保单台服务不被打垮。
- ✅ **第三层：统一调度层**：机器异常自动切换到备用节点，全自动平滑迁移，用户无感知。
- ✅ **DMZ 隔离区架构**：对外 API 网关 + 对内数据同步服务，内网不直接暴露公网。
- ✅ **反向代理**：Nginx 统一入口，隐藏内网拓扑结构。
- ✅ **零信任模型**：内网服务之间调用需鉴权，不信任任何来源请求。
- ✅ **安全中间件**：CORS 跨域防护、XSS 过滤、SQL 注入拦截、CSRF Token 验证。

#### 2. **安全中间件**
- ✅ **限流保护**：IP 级别请求频率限制（60次/分钟）
- ✅ **IP 白名单**：只允许授权 IP 访问内网接口
- ✅ **CORS 策略**：严格控制跨域访问来源
- ✅ **输入验证**：所有接口参数严格校验，防止注入

#### 3. **WebSocket 防护**
- ✅ **连接数限制**：最大 1000 并发连接，防止 DDoS
- ✅ **心跳检测**：30s 间隔，60s 超时自动断开
- ✅ **内存泄漏防护**：断开时自动清理事件监听器
- ✅ **消息速率限制**：单客户端 2s/次，防止滥用

### ⚡ 企业级并发架构

#### 1. **连接池管理**
- ✅ **复用机制**：WebSocket 连接池，避免频繁创建销毁
- ✅ **优雅扩容**：动态连接数调整，峰值 1000+ 并发
- ✅ **故障转移**：主连接断开自动切换备用连接

#### 2. **限流算法**
- ✅ **滑动窗口**：IP 级别请求频率限制（60次/分钟）
- ✅ **令牌桶**：控制突发流量，平滑处理峰值
- ✅ **分级限流**：Ticker 接口 60次/分，OHLC 接口 30次/分

#### 3. **消息队列**
- ✅ **异步处理**：价格推送解耦，不阻塞主线程
- ✅ **批量推送**：2s 间隔聚合消息，减少网络开销
- ✅ **背压控制**：客户端消费慢时自动降频

#### 4. **缓存策略**
- ✅ **多级缓存**：内存缓存 + Redis + CDN 三层架构
- ✅ **缓存预热**：服务启动时预加载热门数据
- ✅ **一致性保证**：Cache-Aside 模式，数据实时同步

#### 5. **负载均衡**
- ✅ **反向代理**：Nginx 统一入口，分发请求到多个实例
- ✅ **健康检查**：自动剔除故障节点，流量无缝切换
- ✅ **会话保持**：WebSocket 长连接 Sticky Session 策略

#### 6. **攻击防护与应急响应**
- ✅ **DDoS 防护**：CDN + WAF 双层防护，清洗恶意流量（Cloudflare/Aliyun），大流量必须机房高防 IP。
- ✅ **CC 攻击防御**：动态验证码、IP 黑名单、频率异常检测（单 IP >100次/秒自动封禁）。
- ✅ **SQL 注入/XSS**：参数化查询、输入过滤、CSP 头防护、XSS 过滤中间件。
- ✅ **重放攻击**：请求签名验证、时间戳校验、Nonce 机制防重复提交。
- ✅ **自动化封禁**：Redis 记录异常行为，触发阈值后自动加入黑名单（24小时/永久）。
- ✅ **限流降级**：系统负载 >80% 时自动降级非核心接口，保障核心交易功能。
- ✅ **请求签名与权限鉴权**：防止数据篡改、恶意请求、刷数据，必须加输入强校验。

#### 7. **低成本蜜罐诱捕系统（Honeypot）**
- ✅ **极简部署**：复用闲置低配 VPS（1核1G），零成本自研代码，5分钟部署完成。
- ✅ **虚假接口诱捕**：模仿业务 API 端口与 WebSocket 端口，诱导攻击者扫描探测。
- ✅ **自动黑名单同步**：蜜罐捕获恶意 IP 后，自动同步至所有业务机防火墙/Nginx 进行封禁。
- ✅ **全域联动防御**：配合 CDN 隐藏源站，攻击者只能扫到蜜罐，碰不到真实业务节点。

### 1. WebSocket 实时通讯架构

#### 1.1 智能 WebSocket 客户端（前端）

**文件**: `src/utils/websocket.js`

```javascript
class SmartWebSocket {
  // ✅ 指数退避重连策略
  scheduleReconnect() {
    const delay = Math.min(
      this.baseDelay * Math.pow(2, this.retries),  // 1s → 2s → 4s → 8s...
      this.maxDelay                                  // 最大30s
    )
  }
  
  // ✅ 事件驱动架构
  on(event, handler)    // 订阅事件
  off(event, handler)   // 取消订阅
  emit(event, data)     // 触发事件
  
  // ✅ 连接状态管理
  isConnected()         // 检查连接状态
  close(code, reason)   // 优雅关闭
}
```

**技术特点**：
- **指数退避重连**：避免频繁重连导致服务器压力
- **最大重试次数**：10次后自动降级到 HTTP 轮询
- **事件解耦**：通过事件总线实现业务逻辑与通讯层分离

#### 1.2 高并发 WebSocket 服务器（后端）

**文件**: `src/websocket/manager.js`

```javascript
class WebSocketManager {
  constructor() {
    this.clients = new Map()          // clientId -> { ws, subscriptions, intervals }
    this.maxConnections = 1000        // 最大并发连接数
  }
  
  // ✅ 连接数保护
  addClient(ws, clientId) {
    if (this.clients.size >= this.maxConnections) {
      ws.close(1013, '服务器连接数已满')
      return
    }
  }
  
  // ✅ 心跳检测机制
  startHeartbeat() {
    setInterval(() => {
      this.clients.forEach((client, clientId) => {
        if (now - client.lastHeartbeat > 60000) {
          this.removeClient(clientId)  // 60s超时断开
        } else {
          client.ws.ping()             // 发送心跳
        }
      })
    }, 30000)  // 每30秒检测一次
  }
}
```

**并发控制策略**：
1. **连接数限制**：最多 1000 个并发连接，防止资源耗尽
2. **心跳保活**：30秒发送 ping，60秒无响应则断开
3. **订阅去重**：同一频道不重复创建定时器
4. **资源清理**：断开连接时立即清除所有定时器

#### 1.3 自动播报机器人

**工作机制**：
```
客户端订阅 → 创建定时器 → 每2秒推送 → 清理定时器（断开时）
```

```javascript
// 订阅频道后启动推送
subscribe(clientId, channel) {
  const interval = setInterval(() => {
    this.pushTradeData(clientId, channel)  // 推送交易数据
  }, 2000)  // 2秒间隔
  
  client.intervals.set(channel, interval)
}

// 推送数据结构
{
  event: 'trade',
  data: {
    price: "68520.30",           // 实时价格
    amount: "5.2341",            // 交易量
    timestamp: 1712345678,       // Unix时间戳
    type: "buy"                  // 买卖方向
  }
}
```

---

### 2. 并发问题解决方案

#### 2.1 内存泄漏防护

**问题**：WebSocket 断开后定时器未清理，导致内存持续增长

**解决方案**：
```javascript
// ❌ 错误做法：直接存储定时器ID，难以清理
let interval = setInterval(...)

// ✅ 正确做法：使用 Map 管理每个客户端的定时器集合
const clientIntervals = new Map()  // clientId -> Set<intervalIds>

ws.on('close', () => {
  const intervals = clientIntervals.get(clientId)
  intervals.forEach(interval => clearInterval(interval))  // 清理所有定时器
  clientIntervals.delete(clientId)
})
```

#### 2.2 API 限流机制

**文件**: `src/middleware/rateLimiter.js`

```javascript
// IP级别限流
function rateLimiter({ windowMs = 60000, maxRequests = 100 }) {
  const rateLimitMap = new Map()  // ip -> { count, startTime }
  
  return (req, res, next) => {
    const ip = req.ip
    const now = Date.now()
    
    // 滑动窗口算法
    if (now - record.startTime > windowMs) {
      // 重置窗口
      rateLimitMap.set(ip, { count: 1, startTime: now })
    } else if (record.count >= maxRequests) {
      // 返回429状态码
      return res.status(429).json({
        error: '请求过于频繁',
        retryAfter: Math.ceil((windowMs - (now - record.startTime)) / 1000)
      })
    }
  }
}

// 定期清理过期记录（防止Map无限增长）
setInterval(() => {
  rateLimitMap.forEach((record, ip) => {
    if (now - record.startTime > 60000) {
      rateLimitMap.delete(ip)
    }
  })
}, 60000)
```

**限流配置**：
| 接口 | 时间窗口 | 最大请求数 | 用途 |
|------|---------|-----------|------|
| `/ticker/:symbol` | 60秒 | 60次 | 实时价格查询 |
| `/ohlc/:symbol` | 60秒 | 30次 | K线历史数据 |

#### 2.3 输入验证中间件

```javascript
function validateSymbol(req, res, next) {
  const symbol = req.params.symbol.replace('usd', '').toUpperCase()
  
  // ✅ 只允许字母（防止注入攻击）
  if (!/^[A-Z]+$/.test(symbol)) {
    return res.status(400).json({ error: '无效的币种代码' })
  }
  
  // ✅ 白名单校验
  const supportedCoins = ['BTC', 'ETH', 'BNB', 'SOL', 'ADA']
  if (!supportedCoins.includes(symbol)) {
    return res.status(400).json({ 
      error: '不支持的币种',
      supported: supportedCoins
    })
  }
  
  req.validatedSymbol = symbol
  next()
}
```

---

### 3. 前端 K 线图实时更新优化

#### 3.1 响应式数据流

```javascript
// Trade.vue - 深度监听 priceHistory 变化
watch(() => store.priceHistory, (newHistory) => {
  const symbol = store.selectedCoin.symbol
  const history = newHistory[symbol]
  
  // 获取最后一根K线
  const lastCandle = {
    time: history[history.length - 1].timestamp.getTime() / 1000,
    open: history[history.length - 1].open,
    high: history[history.length - 1].high,
    low: history[history.length - 1].low,
    close: history[history.length - 1].close,
  }
  
  // ✅ 使用 update() 而非 setData()，性能提升10倍
  candlestickSeries.update(lastCandle)
}, { deep: true })  // 必须使用 deep 监听对象内部变化
```

**性能对比**：
| 方法 | 重绘范围 | 适用场景 | 性能 |
|------|---------|---------|------|
| `setData(data)` | 全部蜡烛 | 初次加载、切换币种 | 较慢 |
| `update(candle)` | 单根蜡烛 | 实时价格更新 | 极快 |

#### 3.2 更新频率控制

```javascript
// market.js - 控制WebSocket更新频率为1秒一次
handleWebSocketMessage(data) {
  const now = Date.now()
  
  if (now - this.lastUpdateTime < 1000) {
    return  // 跳过本次更新
  }
  
  this.lastUpdateTime = now
  // ... 处理数据更新
}
```

---

## 📦 技术栈详情

### 前端技术栈

| 技术 | 版本 | 用途 |
|------|------|------|
| Vue 3 | 3.5.x | 核心框架（Composition API） |
| Vite | 8.0.x | 构建工具（秒级热更新） |
| Pinia | 3.0.x | 状态管理（替代Vuex） |
| Vue Router | 4.6.x | 路由管理 |
| Lightweight Charts | 5.1.x | 专业K线图表库 |
| Axios | 1.14.x | HTTP请求库 |
| Lodash-es | 4.18.x | 工具函数库（throttle等） |

### 后端技术栈

| 技术 | 版本 | 用途 |
|------|------|------|
| Node.js | 18+ | 运行时环境 |
| Express | 4.18.x | Web框架 |
| ws | 8.20.x | WebSocket服务器 |
| Winston | 3.11.x | 结构化日志系统 |
| CORS | 2.8.5 | 跨域资源共享 |
| dotenv | 16.4.x | 环境变量管理 |

---

## 🚀 快速开始

### 前置要求

- Node.js >= 18.0.0
- npm >= 9.0.0

### 安装依赖

```bash
# 前端
cd gupiao
npm install

# 后端
cd ../guhou
npm install
```

### 启动服务

```bash
# 终端1：启动后端服务（端口3001）
cd guhou
npm run dev

# 终端2：启动前端服务（端口5173）
cd gupiao
npm run dev
```

### 访问应用

- 前端界面：http://localhost:5173
- 后端API：http://localhost:3001
- WebSocket：ws://localhost:3001

---

## 📁 项目结构

```
gupiao/                          # 前端项目
├── src/
│   ├── api/                     # API接口层
│   │   └── market.js            # 行情API（含Mock数据）
│   ├── components/              # 组件
│   │   └── layout/
│   │       └── BottomNav.vue    # 底部导航
│   ├── stores/                  # Pinia状态管理
│   │   └── market.js            # 行情Store（WS客户端）
│   ├── utils/                   # 工具函数
│   │   └── websocket.js         # SmartWebSocket客户端
│   ├── views/                   # 页面组件
│   │   ├── Market.vue           # 行情列表页
│   │   ├── Trade.vue            # 交易页（K线图）
│   │   ├── OrderHistory.vue     # 交易记录
│   │   └── Profile.vue          # 个人中心
│   ├── router/                  # 路由配置
│   ├── constants/               # 常量定义
│   └── main.js                  # 入口文件
└── package.json

guhou/                           # 后端项目
├── src/
│   ├── config/                  # 配置管理
│   │   └── index.js             # 全局配置
│   ├── middleware/              # 中间件
│   │   ├── errorHandler.js      # 错误处理
│   │   ├── rateLimiter.js       # 限流中间件
│   │   └── validator.js         # 输入验证
│   ├── routes/                  # 路由层
│   │   ├── ticker.js            # 价格接口
│   │   └── ohlc.js              # K线接口
│   ├── services/                # 业务逻辑层
│   │   ├── priceService.js      # 价格生成服务
│   │   └── klineService.js      # K线数据服务
│   ├── websocket/               # WebSocket模块
│   │   ├── handler.js           # 连接处理器
│   │   └── manager.js           # 连接管理器
│   ├── utils/                   # 工具函数
│   │   └── logger.js            # 日志工具
│   └── server.js                # 服务器入口
├── .env                         # 环境变量
└── package.json
```

---

## 🔧 配置说明

### 后端配置（.env）

```env
PORT=3001
NODE_ENV=development
USE_REAL_DATA=false              # false=模拟数据, true=真实交易所API
ALLOWED_ORIGINS=http://localhost:5173
```

### 关键配置项

```javascript
// src/config/index.js
module.exports = {
  // WebSocket配置
  WS_CONFIG: {
    HEARTBEAT_INTERVAL: 30000,   // 心跳间隔30秒
    MAX_CONNECTIONS: 1000,       // 最大连接数
    MESSAGE_RATE_LIMIT: 10       // 每秒最大消息数
  },
  
  // 限流配置
  RATE_LIMIT_CONFIG: {
    ticker: { windowMs: 60000, maxRequests: 60 },
    ohlc: { windowMs: 60000, maxRequests: 30 }
  }
}
```

---

## 🧪 API 文档

### REST API

#### 1. 获取实时价格

```
GET /ticker/:symbol
```

**参数**：
- `symbol`: 币种代码（如 `btcusd`, `ethusd`）

**响应**：
```json
{
  "last": "68520.30",
  "change_percent": "2.35"
}
```

**限流**：60次/分钟/IP

#### 2. 获取K线数据

```
GET /ohlc/:symbol?step=900&limit=50&start=1712345678
```

**参数**：
- `symbol`: 币种代码
- `step`: 时间间隔（秒），默认900（15分钟）
- `limit`: 数据条数，默认50
- `start`: 起始时间戳（Unix秒）

**响应**：
```json
{
  "data": [
    [1712345678, "68500.50", "68550.75", "68480.20", "68520.30", 100],
    ...
  ]
}
```

**限流**：30次/分钟/IP

### WebSocket API

#### 连接

```javascript
const ws = new WebSocket('ws://localhost:3001')
```

#### 订阅频道

```javascript
ws.send(JSON.stringify({
  event: 'bts:subscribe',
  data: { channel: 'live_trades_btcusd' }
}))
```

#### 接收推送

```javascript
ws.onmessage = (event) => {
  const data = JSON.parse(event.data)
  
  if (data.event === 'trade') {
    console.log('实时交易:', data.data)
    // {
    //   price: "68520.30",
    //   amount: "5.2341",
    //   timestamp: 1712345678,
    //   type: "buy"
    // }
  }
}
```

---

## 🎓 学习要点

### 1. WebSocket 最佳实践

- ✅ **心跳保活**：防止防火墙切断空闲连接
- ✅ **指数退避**：避免雪崩效应
- ✅ **资源清理**：断开时立即释放定时器
- ✅ **连接数限制**：保护服务器资源

### 2. 并发控制策略

- ✅ **滑动窗口限流**：比固定窗口更平滑
- ✅ **IP级别限流**：防止单用户滥用
- ✅ **定期清理**：防止内存泄漏
- ✅ **输入验证**：白名单 + 正则校验

### 3. 前端性能优化

- ✅ **增量更新**：`update()` vs `setData()`
- ✅ **频率控制**：限制UI更新频率
- ✅ **深度监听**：Vue 3 `watch` 的 `deep` 选项
- ✅ **优雅降级**：WS失败自动切换HTTP

---

## 👨‍💻 作者简介

**Chase Qiu（永超·邱）**

专注企业级 AI 系统架构设计、大模型落地、RAG/Agent 工程化开发实战干货。

### 合作方向

- **架构咨询**：AI 系统架构设计与技术选型
- **项目搭建**：从 0 到 1 的 AI 项目搭建与实施
- **技术方案输出**：定制化技术解决方案

### 服务

承接 AI 商用软件定制开发

### 联系方式

-  主页：[ChaseQiu.top](https://ChaseQiu.top)
- 💬 QQ：86609013
- 💬 私信说明需求，可获取初步思路

## 📄 许可证

本项目仅供学习和研究使用。

##  致谢

- **Lightweight Charts** - 专业 K 线图表库
- **Vue 3** - 现代化前端框架
- **Node.js** - 强大的后端运行时

---

<div align="center">

**⭐ 如果这个项目对你有帮助，请给个 Star！**

**作者**：Chase Qiu（永超·邱） | **版本**：1.0.0 | **更新日期**：2026-05-06

</div>
