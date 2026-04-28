---
date: 2026-04-26
tags:
  - nextjs
  - webdev
  - web
  - webapp
  - frontend
  - cs
---

# lec-19 Server-Side Architecture



# lec-20 Cookie, Session, CORS

```tsx
// HTTP-Only cookie, 不可被 JS 读取
httpOnly: true,

// Secure Cookie: 只允许加密网络通过 https 通信, 不允许非加密通讯
secure: true, 

// 默认 "lax" (松), 改成 "strict" 防止 CSRF
sameSite: "lax" / "strict" / "none",

// Persistence Cookie: 7天 (单位ms) 后过期, 存在 Disk 里
// Session Cookie: 不设置 maxAge, 关闭即过期, 存在 RAM 里
maxAge: 7*24*60*60*1000,
```

| 类型 | 存储位置 | 容量 | 生命周期 | 是否跨 tab | 随 HTTP 发给服务器 | 安全性 | 适用场合 |
|------|----------|------|----------|------------|------------------------|------------------|------|
| Cookie | Browser cookie storage | ~4KB–5KB | 可设置 | ✔ | ✔ | ✔ | 记住登录状态 |
| Session Storage | RAM | ~5MB | 关 tab 即清除 | ❌ | ❌ | ❌ | 临时用 |
| Local Storage | Disk | ~5–10MB | 永久 (需手动清除) | ✔ | ❌ | ⚠️ | 持久本地偏好设置 |

**CORS: Cross-Origin Resource Sharing**
