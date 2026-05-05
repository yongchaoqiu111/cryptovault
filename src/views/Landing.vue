<template>
  <div class="landing-page">
    <!-- Hero Section -->
    <div class="hero-section">
      <div class="hero-content">
        <div class="badge">UI/UX INTERACTION DEMO</div>
        <h1 class="title">CryptoVault</h1>
        <p class="subtitle">
          Swipe or tap the navigation to explore three core screens of the fintech experience
        </p>
      </div>
    </div>

    <!-- Phone Mockup -->
    <div class="phone-container">
      <div class="phone-frame">
        <!-- Status Bar -->
        <div class="status-bar">
          <span class="time">9:41</span>
          <div class="status-icons">
            <span class="network">5G</span>
          </div>
        </div>

        <!-- Screen Content -->
        <div class="screen-content" :style="{ transform: `translateX(-${currentScreen * 100}%)` }">
          
          <!-- Screen 1: Dashboard -->
          <div class="screen screen-dashboard">
            <div class="header">
              <div class="user-info">
                <div class="avatar">A</div>
                <div>
                  <div class="greeting">Good Morning</div>
                  <div class="username">Alex Turner</div>
                </div>
              </div>
              <div class="notification-icon"></div>
            </div>

            <div class="balance-card">
              <div class="card-header">
                <span class="menu-icon">☰</span>
                <span class="card-title">Ethereum Balance</span>
              </div>
              <div class="balance-amount">12.4853 <span class="currency">ETH</span></div>
              <div class="balance-usd">≈ $35,542.18 USD</div>
              <div class="change-badge positive">↗ +8.34% this month</div>
              <div class="action-buttons">
                <button class="btn btn-deposit">↓ Deposit</button>
                <button class="btn btn-withdraw">↑ Withdraw</button>
              </div>
            </div>

            <div class="watchlist">
              <div class="watchlist-header">
                <h3>Watchlist</h3>
                <span class="see-all">See All</span>
              </div>
              
              <div class="coin-list">
                <div class="coin-item" v-for="coin in coins" :key="coin.symbol">
                  <div class="coin-icon" :style="{ background: coin.color }">{{ coin.symbol.charAt(0) }}</div>
                  <div class="coin-info">
                    <div class="coin-name">{{ coin.name }}</div>
                    <div class="coin-symbol">{{ coin.symbol }}</div>
                  </div>
                  <div class="coin-chart">
                    <svg :viewBox="coin.chartViewBox" class="mini-chart">
                      <polyline :points="coin.chartPoints" :stroke="coin.change > 0 ? '#4CAF50' : '#F44336'" 
                                fill="none" stroke-width="2"/>
                    </svg>
                  </div>
                  <div class="coin-price-info">
                    <div class="coin-price">${{ coin.price }}</div>
                    <div :class="['coin-change', coin.change > 0 ? 'positive' : 'negative']">
                      {{ coin.change > 0 ? '↗' : '↘' }}{{ coin.change > 0 ? '+' : '' }}{{ coin.change }}%
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Screen 2: Asset Chart -->
          <div class="screen screen-chart">
            <div class="chart-header">
              <h2 class="chart-title">Total Assets</h2>
              <div class="total-amount">$84,321.47</div>
              <div class="change-info positive">
                <span class="arrow">↗</span> +12.48% vs last month
              </div>
            </div>

            <div class="time-filters">
              <button 
                v-for="tf in timeFilters" 
                :key="tf"
                :class="['tf-btn', selectedTimeFilter === tf ? 'active' : '']"
                @click="selectedTimeFilter = tf"
              >{{ tf }}</button>
            </div>

            <div class="chart-area">
              <svg viewBox="0 0 300 150" class="performance-chart">
                <defs>
                  <linearGradient id="chartGradient" x1="0" y1="0" x2="0" y2="1">
                    <stop offset="0%" stop-color="#6366F1" stop-opacity="0.4"/>
                    <stop offset="100%" stop-color="#6366F1" stop-opacity="0"/>
                  </linearGradient>
                </defs>
                <!-- Grid lines -->
                <line x1="0" y1="37.5" x2="300" y2="37.5" stroke="rgba(255,255,255,0.05)" stroke-width="1"/>
                <line x1="0" y1="75" x2="300" y2="75" stroke="rgba(255,255,255,0.05)" stroke-width="1"/>
                <line x1="0" y1="112.5" x2="300" y2="112.5" stroke="rgba(255,255,255,0.05)" stroke-width="1"/>
                
                <!-- Chart area fill -->
                <path d="M0,120 L30,110 L60,115 L90,95 L120,100 L150,85 L180,90 L210,70 L240,75 L270,50 L300,40 L300,150 L0,150 Z" 
                      fill="url(#chartGradient)"/>
                <!-- Chart line -->
                <path d="M0,120 L30,110 L60,115 L90,95 L120,100 L150,85 L180,90 L210,70 L240,75 L270,50 L300,40" 
                      fill="none" stroke="#6366F1" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
                
                <!-- Data points -->
                <circle cx="300" cy="40" r="4" fill="#6366F1" stroke="#fff" stroke-width="2"/>
              </svg>
              
              <div class="chart-labels">
                <span>$100k</span>
                <span>$75k</span>
                <span>$50k</span>
              </div>
              
              <div class="chart-months">
                <span v-for="month in months" :key="month">{{ month }}</span>
              </div>
            </div>

            <div class="coin-filters">
              <button 
                v-for="coin in ['All', 'BTC', 'ETH', 'USDT']" 
                :key="coin"
                :class="['coin-filter-btn', selectedCoinFilter === coin ? 'active' : '']"
                @click="selectedCoinFilter = coin"
              >{{ coin }}</button>
            </div>

            <div class="transactions">
              <div class="transactions-header">
                <h3>Recent Transactions</h3>
                <span class="view-all">View All</span>
              </div>
              
              <div class="transaction-list">
                <div class="transaction-item" v-for="tx in transactions" :key="tx.id">
                  <div class="tx-icon" :class="tx.type">
                    {{ tx.type === 'received' ? '↓' : '↑' }}
                  </div>
                  <div class="tx-info">
                    <div class="tx-title">{{ tx.title }}</div>
                    <div class="tx-time">{{ tx.time }}</div>
                  </div>
                  <div class="tx-amount" :class="tx.type">
                    <div class="tx-amount-value">{{ tx.amount }}</div>
                    <div class="tx-usd">{{ tx.usd }}</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Bottom Navigation -->
        <div class="bottom-nav">
          <button 
            v-for="nav in navigation" 
            :key="nav.name"
            :class="['nav-item', currentScreen === nav.index ? 'active' : '']"
            @click="currentScreen = nav.index"
          >
            <div class="nav-icon" v-html="nav.icon"></div>
            <div class="nav-label">{{ nav.label }}</div>
          </button>
        </div>
      </div>

      <!-- Screen Indicator Dots -->
      <div class="screen-indicators">
        <div 
          v-for="i in 2" 
          :key="i"
          :class="['dot', currentScreen === i - 1 ? 'active' : '']"
        ></div>
      </div>
      
      <div class="swipe-hint">点击底部导航图标切换页面</div>
    </div>

    <!-- Tech Stack Section -->
    <div class="tech-section">
      <h2>核心技术栈</h2>
      <div class="tech-grid">
        <div class="tech-card" v-for="tech in techStack" :key="tech.name">
          <div class="tech-icon" :style="{ background: tech.color }">{{ tech.icon }}</div>
          <div class="tech-name">{{ tech.name }}</div>
          <div class="tech-version">{{ tech.version }}</div>
        </div>
      </div>
    </div>

    <!-- CTA Section -->
    <div class="cta-section">
      <h2>开始体验</h2>
      <p>探索完整的交易体验，实时数据与专业图表</p>
      <div class="cta-buttons">
        <router-link to="/app" class="btn-primary">进入交易系统</router-link>
        <a href="https://github.com" class="btn-secondary" target="_blank">GitHub</a>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const currentScreen = ref(0)
const selectedTimeFilter = ref('Month')
const selectedCoinFilter = ref('All')

const coins = ref([
  {
    symbol: 'BTC',
    name: 'Bitcoin',
    price: '43,210.50',
    change: 2.84,
    color: 'linear-gradient(135deg, #F7931A, #FFB84D)',
    chartPoints: '0,30 20,25 40,28 60,20 80,22 100,15 120,18',
    chartViewBox: '0 0 120 40'
  },
  {
    symbol: 'ETH',
    name: 'Ethereum',
    price: '2,847.30',
    change: 1.62,
    color: 'linear-gradient(135deg, #627EEA, #8B9FEF)',
    chartPoints: '0,35 20,30 40,32 60,25 80,28 100,20 120,22',
    chartViewBox: '0 0 120 40'
  },
  {
    symbol: 'USDT',
    name: 'Tether',
    price: '1.00',
    change: -0.01,
    color: 'linear-gradient(135deg, #26A17B, #4BC99F)',
    chartPoints: '0,20 20,22 40,18 60,25 80,23 100,28 120,25',
    chartViewBox: '0 0 120 40'
  }
])

const timeFilters = ['Month', 'Year']
const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun']

const transactions = ref([
  {
    id: 1,
    type: 'received',
    title: 'Received BTC',
    time: 'Today, 10:24 AM',
    amount: '+0.0248 BTC',
    usd: '+$1,072.14'
  },
  {
    id: 2,
    type: 'sent',
    title: 'Sent ETH',
    time: 'Yesterday, 6:45 PM',
    amount: '-1.500 ETH',
    usd: '-$4,270.95'
  },
  {
    id: 3,
    type: 'received',
    title: 'Received USDT',
    time: 'Dec 14, 2:30 PM',
    amount: '+5,000 USDT',
    usd: '+$5,000.00'
  }
])

const navigation = [
  {
    index: 0,
    name: 'home',
    label: 'Dashboard',
    icon: '<svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/></svg>'
  },
  {
    index: 1,
    name: 'assets',
    label: 'Assets',
    icon: '<svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M3.5 18.49l6-6.01 4 4L22 6.92l-1.41-1.41-7.09 7.97-4-4L2 16.99z"/></svg>'
  }
]

const techStack = ref([
  { name: 'Vue 3', version: '3.5.x', icon: 'V', color: '#42b883' },
  { name: 'Vite', version: '8.0.x', icon: '⚡', color: '#646cff' },
  { name: 'Pinia', version: '3.0.x', icon: 'P', color: '#ffd859' },
  { name: 'WebSocket', version: 'Real-time', icon: 'WS', color: '#FF9800' },
  { name: 'Charts', version: '5.1.x', icon: '📊', color: '#6366F1' },
  { name: 'Node.js', version: '18+', icon: 'N', color: '#68A063' }
])
</script>

<style scoped>
.landing-page {
  min-height: 100vh;
  background: radial-gradient(circle at center, #1a1a2e 0%, #0a0a0a 100%);
  color: #ffffff;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  overflow-x: hidden;
}

/* Hero Section */
.hero-section {
  text-align: center;
  padding: 60px 20px 40px;
  background: radial-gradient(circle at center, rgba(99, 102, 241, 0.1) 0%, transparent 70%);
}

.badge {
  display: inline-block;
  padding: 8px 16px;
  background: rgba(99, 102, 241, 0.2);
  border: 1px solid rgba(99, 102, 241, 0.3);
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 1px;
  color: #a5b4fc;
  margin-bottom: 20px;
}

.title {
  font-size: 48px;
  font-weight: 800;
  background: linear-gradient(135deg, #6366F1 0%, #a855f7 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin: 0 0 16px;
}

.subtitle {
  font-size: 16px;
  color: #94a3b8;
  max-width: 500px;
  margin: 0 auto;
  line-height: 1.6;
}

/* Phone Container */
.phone-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40px 20px;
}

.phone-frame {
  width: 375px;
  height: 812px;
  background: linear-gradient(180deg, #1a1a2e 0%, #0f0f1e 100%);
  border-radius: 40px;
  border: 3px solid #2a2a3e;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5), 0 0 40px rgba(99, 102, 241, 0.2);
  overflow: hidden;
  position: relative;
}

/* Status Bar */
.status-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 24px;
  background: rgba(0, 0, 0, 0.3);
  font-size: 14px;
  font-weight: 600;
}

/* Screen Content */
.screen-content {
  height: calc(100% - 120px);
  display: flex;
  transition: transform 0.3s ease;
}

.screen {
  min-width: 100%;
  padding: 20px;
  overflow-y: auto;
}

/* Dashboard Screen */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 12px;
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: linear-gradient(135deg, #6366F1, #a855f7);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  font-weight: bold;
}

.greeting {
  font-size: 12px;
  color: #94a3b8;
}

.username {
  font-size: 16px;
  font-weight: 600;
}

.notification-icon {
  font-size: 20px;
  cursor: pointer;
}

/* Balance Card */
.balance-card {
  background: linear-gradient(135deg, rgba(99, 102, 241, 0.3) 0%, rgba(168, 85, 247, 0.3) 100%);
  border: 1px solid rgba(99, 102, 241, 0.4);
  border-radius: 16px;
  padding: 20px;
  margin-bottom: 24px;
}

.card-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 16px;
  font-size: 14px;
  color: #cbd5e1;
}

.balance-amount {
  font-size: 32px;
  font-weight: 800;
  margin-bottom: 8px;
}

.currency {
  font-size: 20px;
  color: #94a3b8;
}

.balance-usd {
  font-size: 14px;
  color: #94a3b8;
  margin-bottom: 12px;
}

.change-badge {
  display: inline-block;
  padding: 6px 12px;
  border-radius: 8px;
  font-size: 12px;
  font-weight: 600;
  margin-bottom: 16px;
}

.change-badge.positive {
  background: rgba(76, 175, 80, 0.2);
  color: #4CAF50;
}

.action-buttons {
  display: flex;
  gap: 12px;
}

.btn {
  flex: 1;
  padding: 12px;
  border: none;
  border-radius: 12px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.btn-deposit {
  background: rgba(99, 102, 241, 0.3);
  color: #a5b4fc;
  border: 1px solid rgba(99, 102, 241, 0.4);
}

.btn-withdraw {
  background: rgba(168, 85, 247, 0.3);
  color: #c084fc;
  border: 1px solid rgba(168, 85, 247, 0.4);
}

/* Watchlist */
.watchlist {
  margin-top: 24px;
}

.watchlist-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.watchlist-header h3 {
  font-size: 18px;
  font-weight: 700;
  margin: 0;
}

.see-all {
  font-size: 14px;
  color: #6366F1;
  cursor: pointer;
}

.coin-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.coin-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  transition: all 0.3s;
}

.coin-item:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(99, 102, 241, 0.3);
}

.coin-icon {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  font-weight: bold;
}

.coin-info {
  flex: 1;
}

.coin-name {
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 4px;
}

.coin-symbol {
  font-size: 12px;
  color: #94a3b8;
}

.coin-chart {
  width: 80px;
  height: 40px;
}

.mini-chart {
  width: 100%;
  height: 100%;
}

.coin-price-info {
  text-align: right;
}

.coin-price {
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 4px;
}

.coin-change {
  font-size: 12px;
  font-weight: 600;
}

.coin-change.positive {
  color: #4CAF50;
}

.coin-change.negative {
  color: #F44336;
}

/* Chart Screen */
.screen-chart {
  padding: 0;
}

.chart-header {
  padding: 20px;
  text-align: center;
}

.chart-title {
  font-size: 18px;
  font-weight: 600;
  color: #cbd5e1;
  margin: 0 0 8px;
}

.total-amount {
  font-size: 28px;
  font-weight: 800;
  color: #a5b4fc;
  margin-bottom: 8px;
}

.change-info {
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 16px;
}

.change-info.positive {
  color: #4CAF50;
}

.change-info .arrow {
  margin-right: 4px;
}

.time-filters {
  display: flex;
  gap: 8px;
  justify-content: center;
  margin-bottom: 20px;
  padding: 0 20px;
}

.tf-btn {
  padding: 8px 16px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  color: #94a3b8;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.tf-btn.active {
  background: rgba(99, 102, 241, 0.3);
  border-color: rgba(99, 102, 241, 0.5);
  color: #a5b4fc;
}

.chart-area {
  position: relative;
  padding: 0 20px;
  margin-bottom: 20px;
}

.performance-chart {
  width: 100%;
  height: 150px;
}

.chart-labels {
  position: absolute;
  right: 20px;
  top: 0;
  bottom: 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  font-size: 10px;
  color: #64748b;
  padding: 10px 0;
}

.chart-months {
  display: flex;
  justify-content: space-between;
  padding: 0 20px;
  margin-top: 8px;
  font-size: 10px;
  color: #64748b;
}

.coin-filters {
  display: flex;
  gap: 8px;
  padding: 0 20px;
  margin-bottom: 20px;
}

.coin-filter-btn {
  padding: 6px 12px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  color: #94a3b8;
  font-size: 11px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.coin-filter-btn.active {
  background: rgba(99, 102, 241, 0.3);
  border-color: rgba(99, 102, 241, 0.5);
  color: #a5b4fc;
}

.transactions {
  padding: 0 20px 20px;
}

.transactions-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.transactions-header h3 {
  font-size: 16px;
  font-weight: 700;
  margin: 0;
}

.view-all {
  font-size: 13px;
  color: #6366F1;
  cursor: pointer;
}

.transaction-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.transaction-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  transition: all 0.3s;
}

.transaction-item:hover {
  background: rgba(255, 255, 255, 0.08);
}

.tx-icon {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  font-weight: bold;
}

.tx-icon.received {
  background: rgba(76, 175, 80, 0.2);
  color: #4CAF50;
}

.tx-icon.sent {
  background: rgba(244, 67, 54, 0.2);
  color: #F44336;
}

.tx-info {
  flex: 1;
}

.tx-title {
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 4px;
}

.tx-time {
  font-size: 11px;
  color: #64748b;
}

.tx-amount {
  text-align: right;
}

.tx-amount-value {
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 4px;
}

.tx-amount.received .tx-amount-value {
  color: #4CAF50;
}

.tx-amount.sent .tx-amount-value {
  color: #F44336;
}

.tx-usd {
  font-size: 12px;
  color: #94a3b8;
}

/* Bottom Navigation */
.bottom-nav {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  background: rgba(0, 0, 0, 0.8);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding: 12px 0 24px;
}

.nav-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  background: transparent;
  border: none;
  color: #64748b;
  cursor: pointer;
  transition: all 0.3s;
  padding: 8px;
}

.nav-item.active {
  color: #6366F1;
}

.nav-icon {
  font-size: 24px;
}

.nav-label {
  font-size: 11px;
  font-weight: 600;
}

/* Screen Indicators */
.screen-indicators {
  display: flex;
  gap: 8px;
  margin-top: 24px;
  justify-content: center;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  transition: all 0.3s;
}

.dot.active {
  background: #6366F1;
  width: 24px;
  border-radius: 4px;
}

.swipe-hint {
  margin-top: 16px;
  font-size: 14px;
  color: #64748b;
  text-align: center;
}

/* Tech Section */
.tech-section {
  padding: 60px 20px;
  text-align: center;
}

.tech-section h2 {
  font-size: 32px;
  font-weight: 700;
  margin-bottom: 40px;
}

.tech-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 20px;
  max-width: 900px;
  margin: 0 auto;
}

.tech-card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  padding: 24px;
  transition: all 0.3s;
}

.tech-card:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(99, 102, 241, 0.3);
  transform: translateY(-4px);
}

.tech-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  font-weight: bold;
  margin: 0 auto 12px;
}

.tech-name {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 4px;
}

.tech-version {
  font-size: 12px;
  color: #94a3b8;
}

/* CTA Section */
.cta-section {
  padding: 60px 20px 80px;
  text-align: center;
  background: radial-gradient(circle at center, rgba(99, 102, 241, 0.1) 0%, transparent 70%);
}

.cta-section h2 {
  font-size: 32px;
  font-weight: 700;
  margin-bottom: 16px;
}

.cta-section p {
  font-size: 16px;
  color: #94a3b8;
  margin-bottom: 32px;
}

.cta-buttons {
  display: flex;
  gap: 16px;
  justify-content: center;
}

.btn-primary, .btn-secondary {
  padding: 16px 32px;
  border-radius: 12px;
  font-size: 16px;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s;
}

.btn-primary {
  background: linear-gradient(135deg, #6366F1, #a855f7);
  color: white;
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(99, 102, 241, 0.4);
}

.btn-secondary {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.btn-secondary:hover {
  background: rgba(255, 255, 255, 0.15);
}

/* Responsive */
@media (max-width: 768px) {
  .title {
    font-size: 36px;
  }
  
  .phone-frame {
    width: 100%;
    max-width: 375px;
    height: 700px;
  }
  
  .tech-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  
  .cta-buttons {
    flex-direction: column;
  }
}
</style>
