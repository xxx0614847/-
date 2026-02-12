# 🔧 配置 Supabase 发送 OTP（验证码）而不是 Magic Link

## 问题现象

收到的邮件包含"登录链接"而不是"6位验证码"。

---

## ✅ 解决方案

### 方法一：修改邮件模板（推荐）

1. **登录 Supabase 控制台**
   - 访问：https://supabase.com/dashboard
   - 选择项目：`zgfyqhhvotaunytjtcxi`

2. **进入邮件模板设置**
   - 点击左侧 **Authentication**
   - 选择 **Email Templates**
   - 找到 **Magic Link** 模板

3. **修改模板内容**

将模板改为显示验证码：

```html
<h2>您的登录验证码</h2>

<p>您的验证码是：</p>

<h1 style="font-size: 32px; letter-spacing: 8px; color: #667eea; font-weight: bold;">
  {{ .Token }}
</h1>

<p style="color: #666;">验证码有效期为 60 秒</p>

<p style="color: #999; font-size: 12px;">
  如果这不是您的操作，请忽略此邮件。
</p>
```

4. **保存模板**
   - 点击 **Save** 按钮

---

### 方法二：检查 Auth 设置

1. **进入 Auth 设置**
   - **Settings** > **Auth**

2. **检查以下选项**
   - **Enable email confirmations**: 关闭（测试时）
   - **Secure email change**: 关闭（测试时）
   - **Mailer autoconfirm**: 开启（测试时）

3. **保存设置**

---

### 方法三：使用 Phone Auth（备选方案）

如果邮件 OTP 一直有问题，可以改用手机号验证码：

1. **Authentication** > **Providers**
2. 启用 **Phone** 提供商
3. 配置 Twilio 或其他短信服务

---

## 🧪 测试步骤

配置完成后：

1. **等待 2-3 分钟**（让配置生效）
2. 打开 `index.html`
3. 输入邮箱地址
4. 点击"获取验证码"
5. 检查邮箱，应该收到 **6 位数字验证码**

---

## 📧 正确的邮件格式

配置成功后，邮件应该是这样的：

```
主题：您的登录验证码

您的验证码是：

123456

验证码有效期为 60 秒
```

---

## ⚠️ 注意事项

1. **Token 变量**
   - 在邮件模板中，`{{ .Token }}` 会自动替换为 6 位验证码
   - 不要删除这个变量

2. **不要使用 .ConfirmationURL**
   - `{{ .ConfirmationURL }}` 是 Magic Link（登录链接）
   - 我们需要的是 `{{ .Token }}`（验证码）

3. **模板类型**
   - 确保修改的是 **Magic Link** 模板
   - 不是 **Confirm Signup** 或其他模板

---

## 🔍 验证配置是否正确

### 检查邮件模板

在 **Email Templates** > **Magic Link** 中，应该看到：

```html
{{ .Token }}
```

而不是：

```html
{{ .ConfirmationURL }}
```

### 检查收到的邮件

- ✅ 正确：邮件包含 6 位数字（如 123456）
- ❌ 错误：邮件包含登录链接

---

## 💡 快速模板（复制粘贴）

### 简洁版模板

```html
<h2>验证码</h2>
<h1 style="font-size: 36px; letter-spacing: 10px;">{{ .Token }}</h1>
<p>有效期：60秒</p>
```

### 完整版模板

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
</head>
<body style="font-family: Arial, sans-serif; padding: 20px; background: #f5f5f5;">
  <div style="max-width: 600px; margin: 0 auto; background: white; padding: 40px; border-radius: 10px;">
    <h2 style="color: #333; text-align: center;">您的登录验证码</h2>
    
    <div style="text-align: center; margin: 30px 0;">
      <div style="font-size: 48px; letter-spacing: 15px; color: #667eea; font-weight: bold;">
        {{ .Token }}
      </div>
    </div>
    
    <p style="color: #666; text-align: center;">验证码有效期为 60 秒</p>
    
    <hr style="border: none; border-top: 1px solid #eee; margin: 30px 0;">
    
    <p style="color: #999; font-size: 12px; text-align: center;">
      如果这不是您的操作，请忽略此邮件。<br>
      此邮件由系统自动发送，请勿回复。
    </p>
  </div>
</body>
</html>
```

---

## 🆘 如果还是收到链接

### 清除缓存

1. 在 Supabase 控制台修改模板后
2. 等待 2-3 分钟
3. 清除浏览器缓存
4. 重新测试

### 检查代码

确保 `app.js` 中使用的是：

```javascript
await supabase.auth.signInWithOtp({
    email: email,
    options: {
        shouldCreateUser: true
        // 不要设置 emailRedirectTo
    }
});
```

而不是：

```javascript
await supabase.auth.signInWithOtp({
    email: email,
    options: {
        emailRedirectTo: 'http://...'  // ❌ 这会发送链接
    }
});
```

---

配置完成后，重新测试 `index.html`！
