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

# lec-20 Cookie, Session, CORS
- 给 senario / eg, 判断属于 cookie / session / CORS 哪个

## Cookies
按 Security 分类: HTTP-Only Cookies vs Secure Cookies
按 Lifetime 分类: Session Cookies vs Persistence Cookies
按 Behavior 分类: Same-Site Cookies (设置了 `sameSite` 规则的 cookie)

|**概念**|**是什么**|**存在哪**|**作用**|**谁负责**|
|---|---|---|---|---|
|Cookie|存储在浏览器的小数据|浏览器|保存用户信息（如 token、登录状态）|浏览器 + 后端|
|Session|服务器端的会话数据|服务器内存/数据库|记录用户登录状态|服务器|
|CORS|跨域访问规则|浏览器安全机制|控制“能不能跨域请求”|浏览器 + 后端响应头|
