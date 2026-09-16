# 台南市立醫院｜找文獻 ChatGPT App / MCP v1.0

這是由原 Claude Artifact 版重構的 ChatGPT App 後端。**本專案不需要 OpenAI API Key**：GPT 推理在 ChatGPT 端進行，MCP 只提供 PubMed、館藏、全文路由與 OA 工具。

## 已完成
- `search_pubmed`：PubMed/MeSH 搜尋、分頁、摘要、PMID/DOI/PMCID/PII/MeSH。
- `get_pubmed_article`：取得單篇完整資料。
- `check_tmh_holdings`：使用原版 2026-09 ERM 內嵌清單（7,047 筆）判斷本院館藏與年份。
- `resolve_tmh_fulltext`：PMC、ClinicalKey、EBSCO MEDLINE、Ovid、ScienceDirect、ERM、出版社代理入口。
- `find_free_fulltext`：PMC + OpenAlex OA 查找。
- `library_help`：遠端連線、ERM、NDDS 說明。
- ChatGPT App 搜尋結果卡片：題名、書目、摘要、本院館藏、平台、PubMed/全文/DOI。
- 不保存院內共用帳密；帳密與授權仍由本院 ERM/遠端系統處理。

## 本機測試
```bash
npm install
npm run check
npm start
```
健康檢查：`http://localhost:8787/health`；MCP endpoint：`http://localhost:8787/mcp`。

## 發布
MCP endpoint 必須是 ChatGPT 可連線的 HTTPS 網址。可部署至 Render、Railway、Fly.io、Cloud Run 等 Node.js 主機；若要用 Cloudflare Workers，需再改為 Workers runtime（本版為 Node/Express）。部署後在 ChatGPT 的 App/Developer Mode 將 `https://你的網域/mcp` 加入並掃描 tools。

## AI 使用方式
不要在 MCP 伺服器加入 OpenAI API Key。使用者在 ChatGPT 內提出臨床問題，ChatGPT 負責 PICO、MeSH/關鍵字、證據整理，再呼叫本 MCP。這可避免由本專案伺服器自行產生 OpenAI API 費用；實際 ChatGPT App/Plugin 可用性仍依使用者方案、workspace 與 OpenAI rollout 為準。

## 館藏資料
`data/holdings.txt` 是由原 HTML 的 `HOLDINGS_GZ_B64` 解壓所得，版本 2026-09，共 7,047 筆。格式：`刊名|平台代碼|起年|迄年|journal_id`。
