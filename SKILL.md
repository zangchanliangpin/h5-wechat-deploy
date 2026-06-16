---
name: h5-wechat-deploy
description: >
  H5单页部署到GitHub Pages并分享到微信朋友圈的完整工作流。
  This skill should be used when the user wants to create a single-page H5 landing page,
  deploy it for WeChat Moments (朋友圈) sharing, or troubleshoot WeChat/H5 deployment issues.
  Triggers: H5页面, 部署, GitHub Pages, 微信朋友圈, 链接打不开, 恢复访问, landing page deployment.
agent_created: true
---

# H5 微信朋友圈部署

## 概述

创建电商H5推广单页并部署到GitHub Pages，用于微信朋友圈分享。涵盖页面创建、GitHub仓库搭建、Pages部署、微信兼容性处理的全流程。

## 使用场景

- 用户需要做一个H5单页推广链接发朋友圈
- 需要部署静态页面到GitHub Pages
- 微信打开链接报错需要排查
- 需要修改已有H5页面内容并重新部署

## 工作流程

### 第一步：确认需求

向用户确认以下信息：
1. 页面用途（淘宝店铺推广/微信私域引流/品牌展示等）
2. 需要展示的核心内容（CTA按钮跳转链接、微信联系方式、产品卖点等）
3. 设计风格偏好（暗黑奢华/简约清新/品牌色等）

### 第二步：创建H5单页

使用 `assets/template.html` 作为基础模板，包含以下核心模块：

- **CTA按钮**: 醒目的大按钮引导用户点击跳转（淘宝店铺/微信等）
- **社交证明**: 展示购买记录、用户评价等信任背书元素
- **联系方式**: 微信名片卡片，支持一键复制微信号
- **产品卖点卡片**: 3-4个核心卖点展示
- **Toast提示**: 操作反馈（复制成功等）

关键CSS注意事项：
- 中文字符换行需设置 `word-break: keep-all; overflow-wrap: anywhere;` 防止单字断行
- 按钮必须使用 `<button>` 标签而非 `<a href="#">`，确保微信内置浏览器可点击
- 使用 `-webkit-appearance: none;` 重置微信浏览器默认样式

### 第三步：部署到GitHub Pages

#### 3.1 准备工作

如果用户没有GitHub账号，需协助注册：
- 访问 github.com 注册
- 验证邮箱（QQ邮箱可能需要用外部浏览器打开验证链接）

#### 3.2 创建仓库并推送

```bash
# 在页面目录初始化git
cd <页面目录>
git init
git add index.html
git commit -m "Initial commit"

# 创建GitHub仓库（通过gh CLI或手动）
gh repo create <repo-name> --public --push --source .
```

#### 3.3 启用GitHub Pages

在GitHub仓库页面：
1. Settings → Pages
2. Source: Deploy from a branch
3. Branch: main → / (root) → Save
4. 等待部署完成（页面顶部会显示部署状态）

### 第四步：微信兼容性处理 ⚠️ 关键

#### DNS传播延迟

GitHub Pages首次部署后，DNS生效需要 **5-30分钟**。部署完成后不要立即测试，等待一段时间再打开。

部署完成≠立即可访问！

#### 微信安全提示

微信内置浏览器对 `github.io` 等非备案域名会弹出安全警告页面。

**解决方法**: 用户点击页面上的「**恢复访问**」或「**继续访问**」按钮即可正常浏览。

这不是部署问题，是微信的正常安全机制。

#### 不同设备/账号的差异

不同微信账号的DNS缓存刷新时间不同，可能出现：
- 自己手机能打开，转发给别人打不开 → 等待几分钟让对方重试
- 同一个WiFi能打开，移动网络打不开 → DNS同步问题，等待即可

### 第五步：验证部署

```bash
# 验证页面可访问（返回HTTP 200）
curl -I https://<username>.github.io/<repo-name>/

# 在手机浏览器先测试，确认没问题再用微信打开
```

### 第六步：分享到朋友圈

1. 复制 GitHub Pages URL：`https://<username>.github.io/<repo-name>/`
2. 微信中粘贴链接发送
3. 点击链接→出现安全提示→点击「恢复访问」
4. 确认页面正常显示后即可分享到朋友圈

## 常见问题

| 问题 | 原因 | 解决方案 |
|------|------|----------|
| 部署后立即打不开 | DNS未生效 | 等待5-30分钟 |
| 微信显示"无法打开" | 安全拦截 | 点击「恢复访问」 |
| 转发给朋友打不开 | DNS缓存不同步 | 等待几分钟重试 |
| 按钮点不了 | 用了`<a href="#">` | 改用`<button>`标签 |
| 中文断行奇怪 | 未设置word-break | 添加`word-break: keep-all` |

## 模板资源

- `assets/template.html` — 完整的H5单页模板（暗黑奢华风格，含CTA按钮、社交证明、微信名片、产品卖点卡片、Toast提示系统）
