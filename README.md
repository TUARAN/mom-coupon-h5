# 👶 宝妈省钱神器

一个专为母婴群体设计的优惠券领取H5网页应用，提供精选的母婴用品优惠券信息。

## ✨ 功能特点

- 🎯 **母婴专属**：专注纸尿裤、奶粉、辅食等母婴用品
- 💰 **优惠信息**：展示当前价格、原价、节省金额
- 🚀 **一键跳转**：直接跳转到拼多多等平台购买
- 📱 **移动优先**：H5自适应设计，单手操作友好
- 🎨 **温馨设计**：粉色系配色，圆角卡片，温馨舒适

## 🛠️ 技术栈

- **前端框架**：Vue 3 + Composition API
- **构建工具**：Vite
- **样式框架**：TailwindCSS
- **字体**：Inter (Google Fonts)
- **部署平台**：Cloudflare Pages

## 📁 项目结构

```
mom-coupon-h5/
├── src/
│   ├── components/
│   │   └── CouponCard.vue          # 优惠券卡片组件
│   ├── views/
│   │   └── Home.vue                # 首页组件
│   ├── data/
│   │   └── coupons.json            # 优惠券数据
│   ├── App.vue                     # 根组件
│   ├── main.js                     # 入口文件
│   └── style.css                   # 全局样式
├── public/                         # 静态资源
├── index.html                      # HTML模板
├── tailwind.config.js              # TailwindCSS配置
├── postcss.config.js               # PostCSS配置
├── vite.config.js                  # Vite配置
└── package.json                    # 项目依赖
```

## 🚀 本地开发

```bash
# 安装依赖
npm install

# 启动开发服务器
npm run dev

# 构建生产版本
npm run build

# 预览生产版本
npm run preview
```

## 🌐 部署到 Cloudflare Pages

### 方法一：通过 GitHub 部署

1. 将代码推送到 GitHub 仓库
2. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
3. 进入 **Pages** 页面
4. 点击 **Create a project**
5. 选择 **Connect to Git**
6. 选择你的 GitHub 仓库
7. 配置构建设置：
   - **Framework preset**: Vite
   - **Build command**: `npm run build`
   - **Build output directory**: `dist`
   - **Root directory**: `/` (留空)
8. 点击 **Save and Deploy**

### 方法二：直接上传

1. 本地构建项目：`npm run build`
2. 登录 Cloudflare Dashboard
3. 进入 Pages 页面
4. 点击 **Create a project**
5. 选择 **Direct Upload**
6. 上传 `dist` 文件夹内容

## 📱 移动端优化

- 响应式设计，适配各种屏幕尺寸
- 触摸友好的大按钮设计
- 流畅的滚动和动画效果
- 优化的字体大小和间距

## 🎨 设计特色

- **色彩方案**：温暖的粉色系 + 柔和白色 + 浅蓝色
- **卡片设计**：圆角卡片 + 轻微阴影
- **交互效果**：悬停缩放 + 点击反馈
- **字体选择**：Inter 字体，清晰易读

## 📊 数据管理

当前使用本地 JSON 文件存储优惠券数据，后续可轻松扩展为后端 API：

```json
{
  "id": 1,
  "title": "商品名称",
  "price": "当前价格",
  "detail": "优惠描述",
  "link": "跳转链接",
  "category": "商品分类",
  "originalPrice": "原价",
  "savings": "节省金额"
}
```

## 🔧 自定义配置

### 修改主题色彩

在 `tailwind.config.js` 中修改颜色配置：

```javascript
colors: {
  'pink-warm': '#FFB6C1',
  'pink-light': '#FFE4E1',
  'blue-soft': '#E6F3FF',
  'white-warm': '#FFF8F8',
}
```

### 添加新的优惠券

在 `src/data/coupons.json` 中添加新的优惠券数据即可。

## 📄 许可证

MIT License

---

**宝妈省钱神器** - 让每一位宝妈都能轻松省钱！ 💕
