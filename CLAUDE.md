## 部落格資訊

- 曹安的 Toky Stories 部落格。**目錄結構刻意拆兩層**：
  - `影片\podcast\blog\*.md`（**repo 根目錄**）＝ 每篇文章的原始檔，曹安/Claude 平常寫文章只碰這一層
  - `影片\podcast\blog\code\`＝ Astro 專案本體（package.json、astro.config.mjs、src/ 等），不要把文章寫進 `code/src/content/blog/`——那個資料夾已經清空不用了
- Repo：https://github.com/TokyStories/blog（**公開**，免費 GitHub Pages 只能掛在公開 repo）
- 內容讀取設定在 `code/src/content.config.ts`：`glob({ base: '../', pattern: ['*.{md,mdx}', '!CLAUDE.md'] })`。**`base` 是相對 Astro 專案根目錄（`code/`）算，不是相對 `content.config.ts` 自己的資料夾**——這裡踩過一次坑（多算一層跑到 `podcast/` 去），改這個路徑前留意。`pattern` 用單層 `*`（不是 `**`）只抓 repo 根目錄直屬的 `.md`/`.mdx`，不會遞迴進 `code/`（不然會把 `code/node_modules` 裡幾千個套件 README 也當文章讀進來）；`!CLAUDE.md` 是排除這份文件自己
- 部署：push 到 `main` 觸發 `.github/workflows/deploy.yml`（`withastro/action` build → `actions/deploy-pages` 發布），repo 的 Settings → Pages → Source 要設成「GitHub Actions」（一次性手動設定，沒有 `gh` CLI 沒法用指令設）。**曹安已授權：這個 repo 改完（文章、頁面、設定都算）直接 commit + push main，不用每次先問**——push 就等於發布，跟一般 repo 要先確認不同
- `deploy.yml` 裡 `withastro/action@v3` 兩個必填 with：**`path: code`**（專案不在 repo 根目錄，要告訴 action 去哪找 package.json）、**`node-version: "22"`**（它預設 Node 20，但這個 Astro 版本要求 ≥22.12，不指定會 build 失敗，本機 `npm run build` 卻會過，容易誤判是 Pages 設定問題）
- 目前網址 `https://tokystories.github.io/blog`（`code/astro.config.mjs` 的 `site`+`base`）；之後要接自訂網域：GitHub Settings → Pages 填網域＋DNS 那邊加 CNAME，`site` 改成新網域、拿掉 `base`
- 寫新文章：`.md`/`.mdx` 直接建在 repo 根目錄（跟 `first-post.md` 那五篇範例同一層），frontmatter 至少要有 `title`/`description`/`pubDate`；圖片放 `code/src/assets/` 底下，文章裡引用要寫 `./code/src/assets/xxx.jpg`（範例文章的 `heroImage` 已經改成這個寫法，照抄就好）
- **文章要放影片一律嵌 YouTube，不要把 mp4 檔案本體放進 repo**（GitHub Pages 發布網站硬限 1GB、單檔 >100MB 會被 push 擋下，幾支片就爆）。上傳 YouTube 後直接在 `.md` 貼原生 `<iframe>`（Astro 預設支援，不用轉 `.mdx`），用 `youtube-nocookie.com` 網域＋16:9 aspect-ratio 包一層 div 避免手機版被壓扁。
- **`npm run build`／`npm install`／`astro` 等指令輸出一律收掉**（例 `... 2>&1 | tail -20`），只在真的要除錯才留全文——這些指令的 log 很長，全部留在對話裡會讓後面每一輪重讀上下文的成本一直疊高

## Development

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)
