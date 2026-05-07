# Ethical Use & Platform ToS

HiveReach is a glue layer over upstream open-source tools that mostly call public web APIs and webpages. Some channel implementations send a browser-style `User-Agent` header so the request isn't rejected by the platform's default UA filter — this is industry-standard for tools in this category, but it means the responsibility for compliant use sits with **you**, the operator.

By using HiveReach you agree to the following ground rules:

1. **Read the target platform's Terms of Service.** Each platform has its own rules about automated access, rate limits, and downstream use of data. HiveReach does not enforce those rules — you do.
2. **Respect rate limits.** When a platform throttles you (HTTP 429, captcha, "too many requests"), back off. Don't loop, swap accounts, or rotate IPs to bypass.
3. **No bulk scraping or republishing.** Casual reading, search, and one-off fetches for personal or agentic use are within the spirit of the tool. Bulk crawls, building competitor datasets, training models on copyrighted content, or republishing scraped content are not.
4. **Use throwaway accounts for cookie-auth platforms.** Twitter, XiaoHongShu, Bilibili, etc. Cookies grant full account access and platforms may flag scripted activity — see the 🍪 cookie warning in the project README.
5. **If you need scale, pay the official API.** HiveReach's design point is "give an agent eyes" — single-user reach, not a scraping pipeline.

If you're a platform owner concerned about HiveReach's behavior, please [open an issue](https://github.com/xavierliang/HiveReach/issues) — we'll engage in good faith.

## 中文摘要

HiveReach 通过上游开源工具调用公开 API 和网页。部分 channel 会带浏览器风格的 `User-Agent` 头（行业惯例，避免被默认 UA 拦截），这意味着合规使用的责任在你这一侧：

- 阅读并遵守目标平台的服务条款（ToS）和 robots.txt
- 被限流时降速或停止，不要绕过
- 不做大规模抓取、转售或用于训练侵权数据集
- 需要 Cookie 的平台用专用小号，不要用主号
- 真要规模化，请走平台官方 API
