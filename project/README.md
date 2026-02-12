# Supabase 邮箱验证码登录

一个使用 Supabase 实现的现代化邮箱验证码登录注册页面。

## ✨ 功能特性

- ✅ 邮箱验证码登录/注册
- ✅ 8位数字验证码
- ✅ 60秒倒计时防重复发送
- ✅ 实时表单验证
- ✅ 响应式设计
- ✅ 现代美观的界面

## 🚀 快速开始

### 1. 打开登录页面

直接在浏览器中打开 `index.html` 文件即可使用。

### 2. 使用说明

1. 输入邮箱地址
2. 点击"获取验证码"
3. 检查邮箱（包括垃圾邮件文件夹）
4. 输入收到的 8 位验证码
5. 点击"登录/注册"完成

## 📁 项目文件

```
.
├── index.html              # 登录页面（主文件）
├── README.md              # 项目说明
├── CONFIGURE-OTP.md       # Supabase 配置指南
└── .gitignore             # Git 忽略文件
```

## ⚙️ 配置说明

项目已经配置好 Supabase 连接，可以直接使用。

如果需要修改配置或遇到问题，请查看 `CONFIGURE-OTP.md` 文件。

## 🔧 自定义配置

如果需要使用自己的 Supabase 项目：

1. 打开 `index.html` 文件
2. 找到 `CONFIG` 对象（约第 200 行）
3. 替换 `url` 和 `key` 为你的 Supabase 项目信息

```javascript
const CONFIG = {
    url: 'YOUR_SUPABASE_URL',
    key: 'YOUR_SUPABASE_ANON_KEY'
};
```

## 📧 邮件配置

确保 Supabase 项目中：
1. Email 认证已启用
2. 邮件模板配置为发送 OTP（验证码）而不是 Magic Link

详细配置步骤请查看 `CONFIGURE-OTP.md`。

## 🎨 技术栈

- HTML5
- CSS3
- JavaScript (ES6+)
- Supabase Auth API

## 📝 注意事项

- 验证码有效期为 60 秒
- 免费版 Supabase 每小时限制 4 封邮件
- 建议在生产环境中使用 HTTPS
- 验证码为 8 位数字

## 🆘 常见问题

### 收不到验证码？

1. 检查垃圾邮件文件夹
2. 确认邮箱地址正确
3. 等待 1-2 分钟（邮件可能延迟）
4. 检查是否超过发送限制（每小时 4 封）

### 验证码验证失败？

- 确保验证码在 60 秒有效期内
- 每个验证码只能使用一次
- 确保邮箱地址完全一致

## 📄 License

MIT
