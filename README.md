[万物笔记-交互演示-单文件.html](https://github.com/user-attachments/files/34173195/-.-.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover" />
<title>万物笔记 · 交互演示</title>
<style>
/* ============================================================
   动感圆笔记 · Web 高保真交互演示
   ------------------------------------------------------------
   【色卡集中区 · 唯一颜色真源】
   全站所有颜色都定义在下面的 :root 变量里，app.js 顶部只做「读取 + 兜底」。
   换色卡时只改这一处（--c1..--c16 / --l1..--l5 / --ink / --muted）即可，
   style.css 其余部分与 app.js 均无需改动。
     --c1..--c16: 圆 / 板块填充（用户色卡 16 色，逐像素取样）
     --l1..--l5 : 浅色色卡（文字颜色 / 浅色块用）
     --ink      : 主文字 / 深线条
     --muted    : 次级文字 / 图标线条
   ============================================================ */
:root {
  /* ---- 主色卡：圆、竖条、板块填充（16 色，取自用户提供的色卡） ----
     圆内文字由 app.js 的 textColorFor() 按相对亮度自动决定：
     亮度 < 0.20 用白字（#556FB5 / #5F52A0 / #8957A1 / #AD5DA1），其余用 #1A1A1A。 */
  --c1:        #EC6841;
  --c2:        #F19149;
  --c3:        #F7B551;
  --c4:        #FFF45C;
  --c5:        #B3D465;
  --c6:        #7FC269;
  --c7:        #31B16C;
  --c8:        #12B4B1;
  --c9:        #00B7EE;
  --c10:       #448AC9;
  --c11:       #556FB5;
  --c12:       #5F52A0;
  --c13:       #8957A1;
  --c14:       #AD5DA1;
  --c15:       #EA68A2;
  --c16:       #EB6877;

  /* ---- 浅色色卡：文字颜色 / 浅色块（主色卡提亮版，不用深色） ---- */
  --l1:        #BFC7C9;
  --l2:        #C7C7CD;
  --l3:        #CDD1CF;
  --l4:        #CDC7C4;
  --l5:        #D7D2C1;

  --bg:        #F2F2F0;   /* 页面背景：浅灰 */
  --surface:   #FFFFFF;   /* 底色：纯白 */
  --ink:       #2F2F35;   /* 主文字 / 深线条 */
  --ink-rgb:   47, 47, 53;/* 主文字的 RGB 分量（供 rgba() 复用，跟随 --ink） */
  --muted:     #5E616D;   /* 次级文字 / 图标线条 */
  --faint:     #B9B9B4;   /* 空态 ＋ / 占位灰字 / 新建圆悬浮按钮 */
  --theme:     #7B5EA7;   /* 主题紫（新建圆按钮 / 万图标，与 App 启动图标同色） */
  --accent:    #C9A0DC;   /* 粉紫（未选中态的「万」图标） */
  --danger:    #C0453C;   /* 危险红（回收站「删除」等不可恢复操作，同 AppPalette.DANGER） */
  --line:      #E7E6E2;   /* 分隔线 */
  --gray-1:    #ECEBE7;   /* 浅灰占位 1（仅小面积占位用） */
  --gray-2:    #E7E6E1;   /* 浅灰占位 2 */
  --gray-3:    #F0EFEA;   /* 浅灰占位 3 */
  --gray-4:    #EAE9E4;   /* 浅灰占位 4 */
  --tabbar-h:  80px;   /* 与 App Material3 NavigationBar 对齐（80dp） */
}

* { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

html, body {
  margin: 0;
  padding: 0;
  background: var(--bg);
  color: var(--ink);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
               "Hiragino Sans GB", "Microsoft YaHei", "Source Han Sans SC", sans-serif;
  -webkit-font-smoothing: antialiased;
}

/* ---------------- 页面外壳（手机框之外） ---------------- */
.page {
  min-height: 100vh;
  padding: 28px 16px 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.page-head { width: 100%; max-width: 640px; text-align: center; margin-bottom: 20px; }

.page-head h1 {
  margin: 0 0 6px;
  font-size: 22px;
  font-weight: 700;
  letter-spacing: .5px;
  color: var(--ink);
}
.page-head .sub { margin: 0 0 8px; font-size: 13px; color: var(--muted); }
/* 大标题下方的操作说明（放在页面 chrome 上，不在手机演示框内） */
.page-head .page-hint { margin: 0; font-size: 12.5px; line-height: 1.7; color: var(--muted); }
.page-head .page-hint kbd {
  display: inline-block;
  min-width: 16px;
  padding: 0 5px;
  margin: 0 1px;
  border: 1px solid var(--line);
  border-bottom-width: 2px;
  border-radius: 4px;
  background: #FFFFFF;
  font-family: inherit;
  font-size: 11.5px;
  color: var(--ink);
}

.note {
  text-align: left;
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: 14px;
  padding: 14px 16px;
  font-size: 12.5px;
  line-height: 1.75;
  color: var(--muted);
}
.note p { margin: 0 0 6px; }
.note p:last-child { margin-bottom: 0; }
.note b { color: var(--ink); font-weight: 700; }
.note-tip { color: var(--muted); }

.link-btn {
  border: none;
  background: none;
  padding: 0;
  font: inherit;
  color: var(--muted);
  text-decoration: underline;
  cursor: pointer;
}

/* ---------------- 手机外框 ---------------- */
.phone-wrap {
  margin: 0 0 18px;
  transform-origin: top center;
}

.phone {
  width: 450px;            /* 逻辑宽 450：万物结果区内宽≈430，满足 3 列阈值（W-36≥376），与手机端一致 */
  height: 875px;            /* 竖屏比例 1.94:1（=2800/1440），对应 1440×2800（已扣除顶部状态栏占位的完整 3168 高） */
  background: var(--surface);
  border: 1px solid #DFDEDA;
  border-radius: 42px;
  box-shadow: 0 18px 46px rgba(var(--ink-rgb), .10), 0 2px 6px rgba(var(--ink-rgb), .05);
  padding: 10px;
  overflow: hidden;
  position: relative;
}

.screen {
  position: relative;
  width: 100%;
  height: 100%;
  background: var(--surface);
  border-radius: 34px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.screen-body { position: relative; flex: 1 1 auto; overflow: hidden; }

/* Tab 页容器 */
.view {
  position: absolute;
  inset: 0;
  display: none;
  flex-direction: column;
  background: var(--surface);
}
.view.active { display: flex; }

/* ---------------- 通用小组件 ---------------- */
/* 页内小节标题：App 13.sp / TEXT_PRIMARY(#5E616D)；「最近访问」上方 14dp 下方 8dp，
   「全部笔记」上方 8dp 下方 6dp（见 CatalogScreen.kt:123-153） */
.sec-title {
  font-size: 13px;
  font-weight: 500;
  color: var(--muted);
  padding: 14px 16px 8px;
}
.sec-all { padding: 8px 16px 6px; }

/* 目录页大标题：App 22.sp / Bold / TEXT_STRONG（CatalogScreen.kt:116-121） */
.cat-title {
  font-size: 22px;
  font-weight: 700;
  color: var(--ink);
  padding: 10px 16px 4px;
  flex: 0 0 auto;
}

.btn-ghost {
  border: 1px solid var(--line);
  background: var(--surface);
  border-radius: 999px;
  font-size: 11.5px;
  color: var(--muted);
  padding: 4px 11px;
  cursor: pointer;
  font-family: inherit;
}
.btn-ghost:active { background: var(--gray-3); }

/* ---------------- 横向「书页进度条」/ 竖向滚动指示器 ----------------
   两者样式一致：细圆角轨道 + 深色滑块，只是一个横放一个竖放 */
.h-progress {
  flex: 0 0 auto;
  height: 4px;
  margin: 6px 18px 4px;
  background: var(--line);
  border-radius: 999px;
  position: relative;
  overflow: hidden;
}
.h-progress .hp-thumb {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  min-width: 18px;
  border-radius: 999px;
  background: var(--muted);
  transition: left .08s linear, width .08s linear;
}
.v-progress {
  position: absolute;
  top: 0;
  right: 3px;
  width: 4px;
  height: 100%;
  background: transparent;
  border-radius: 999px;
  pointer-events: none;
}
.v-progress .vp-thumb {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  min-height: 24px;
  border-radius: 999px;
  background: var(--muted);
  opacity: .55;
  transition: top .08s linear, height .08s linear;
}

/* ---------------- Tab1 目录页 ---------------- */
.catalog { display: flex; flex-direction: column; height: 100%; }

/* 最近访问：横向排列、左右滑动、显示全部；卡片完整显示不裁切 */
.recent-row {
  display: flex;
  gap: 10px;
  overflow-x: auto;
  padding: 6px 16px 4px;
  scrollbar-width: none;
  flex: 0 0 auto;
}
.recent-row::-webkit-scrollbar { display: none; }

.recent-card {
  flex: 0 0 auto;
  width: 128px;
  height: 78px;
  border-radius: 14px;
  padding: 11px 12px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.recent-card .rc-title {
  font-size: 13.5px; font-weight: 700;
  overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
}
.recent-card .rc-meta { font-size: 10px; opacity: .82; }

.cat-wrap { flex: 1 1 auto; position: relative; overflow: hidden; display: flex; }
.cat-list { flex: 1 1 auto; overflow-y: auto; padding: 2px 8px 8px; scrollbar-width: none; }
.cat-list::-webkit-scrollbar { width: 0; }

.cat-row {
  display: flex;
  align-items: center;
  gap: 11px;
  padding: 11px 10px;
  border-radius: 12px;
  cursor: pointer;
  /* 长按 600ms 弹删除确认：屏蔽系统长按菜单 / 文本选择，避免和浏览器默认行为打架 */
  -webkit-user-select: none;
  user-select: none;
  -webkit-touch-callout: none;
}
.cat-row:active { background: var(--gray-3); }
.cat-dot { width: 26px; height: 26px; border-radius: 50%; flex: 0 0 auto; }
.cat-name { font-size: 14.5px; font-weight: 700; color: var(--ink); }
/* 大标题下方灰色小字：子圆显示所属父圆标题（路径），根圆留空 */
.cat-preview {
  font-size: 11px; color: var(--muted); margin-top: 2px;
  overflow: hidden; text-overflow: ellipsis; white-space: nowrap; max-width: 240px;
  min-height: 0;
}
.cat-row .cat-arrow { margin-left: auto; color: var(--faint); font-size: 15px; }

.cat-bottom {
  flex: 0 0 auto;
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 16px 14px;
  border-top: 1px solid var(--line);
  background: var(--surface);
}
/* 用户 / 搜索 / 设置图标：点线组合、无实心色块 */
.avatar,
.icon-btn {
  width: 30px; height: 30px; border-radius: 50%;
  background: var(--surface);
  border: 1px solid var(--line);
  flex: 0 0 auto;
  display: flex; align-items: center; justify-content: center;
  cursor: pointer;
}
.icon-btn svg, .avatar svg { width: 15px; height: 15px; stroke: var(--muted); fill: none; stroke-width: 1.6; stroke-linecap: round; stroke-linejoin: round; }
.icon-btn:active, .avatar:active { background: var(--gray-3); }
.cat-time {
  margin-left: auto;
  font-size: 11.5px;
  color: var(--muted);
  text-align: right;
}
.cat-time b { color: var(--ink); font-weight: 700; }

/* ---------------- 搜索面板 ---------------- */
.search-panel {
  position: absolute;
  inset: 0;
  background: var(--surface);
  display: none;
  flex-direction: column;
  z-index: 55;
  border-radius: 34px;
  overflow: hidden;
}
.search-panel.open { display: flex; }
.search-head {
  flex: 0 0 auto;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 12px 10px;
  border-bottom: 1px solid var(--line);
}
.search-input {
  flex: 1 1 auto;
  border: none;
  outline: none;
  background: var(--gray-3);
  border-radius: 999px;
  padding: 8px 14px;
  font-size: 13px;
  font-family: inherit;
  color: var(--ink);
}
.search-results { flex: 1 1 auto; overflow-y: auto; padding: 6px 8px 12px; scrollbar-width: none; }
.search-results::-webkit-scrollbar { display: none; }
.search-empty { padding: 22px 18px; font-size: 12px; color: var(--faint); }

/* ---------------- 回收站覆盖层（对应 App TrashScreen） ----------------
   绝对定位铺满 .screen、白底，默认不显示；z-index 必须高于 .tabbar(50)：
   App 里回收站是独立覆盖层、不经过 Scaffold，底部 Tab 栏看不到。 */
.trash-layer {
  position: absolute;
  inset: 0;
  background: var(--surface);
  display: none;
  z-index: 60;
  border-radius: 34px;
  overflow: hidden;
}
/* 顶部操作条：固定 56px，选中 / 未选中两态**原地切换**（不把网格顶下去） */
.trash-topbar {
  height: 56px;
  display: flex;
  align-items: center;
  padding: 0 12px;
}
/* 文字按钮：padding 8px 放在文字之外，得到约 34px 见方的点击热区（同 App TrashBarText） */
.trash-bar-btn {
  flex: 0 0 auto;
  border: none;
  background: none;
  font-family: inherit;
  font-size: 13px;
  font-weight: 500;
  color: var(--ink);
  padding: 8px;
  border-radius: 999px;
  cursor: pointer;
}
.trash-bar-btn:active { background: var(--gray-3); }
.trash-bar-btn.danger { color: var(--danger); margin-left: 10px; }
.trash-bar-title {
  flex: 1 1 auto;
  padding-left: 8px;
  font-size: 18px;
  font-weight: 700;
  color: var(--ink);
}
.trash-bar-title.sel { font-size: 16px; }

.trash-body { height: calc(100% - 56px); overflow-y: auto; scrollbar-width: none; }
.trash-body::-webkit-scrollbar { display: none; }
.trash-empty {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 13px;
  color: var(--faint);
}

/* 网格：3 列，容器左右 18px、上下 12px，行列 gap 均 14px（同 App LazyVerticalGrid） */
.trash-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 14px;
  padding: 12px 18px;
}
/* 回收站里的圆：正方形占满格子、圆灰底、内 padding 10px、14px/700 居中两行截断 */
.trash-circle {
  position: relative;
  aspect-ratio: 1 / 1;
  border-radius: 50%;
  background: var(--faint);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
  cursor: pointer;
  -webkit-touch-callout: none;
  user-select: none;
}
/* 选中态 = 2px 主题紫描边（inset 画在圆内，不撑大格子）+ 右上角 16px 紫色 ✓ */
.trash-circle.sel { box-shadow: inset 0 0 0 2px var(--theme); }
.tc-title {
  font-size: 14px;
  font-weight: 700;
  line-height: 1.25;
  color: var(--ink);
  text-align: center;
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  word-break: break-word;
}
.tc-check {
  position: absolute;
  top: 0;
  right: 0;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: var(--theme);
  color: #FFFFFF;
  font-size: 10px;
  line-height: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* ---------------- Tab2 圆主页 ---------------- */
/* 顶部搜索栏（对齐 App HomeScreen 的 OutlinedTextField）：
   左右各 32 留白、上 14 下 6、圆角 24、13sp、未聚焦边框 = 主文字色、聚焦边框 = 主题紫 */
.ring-search {
  display: block;
  flex: 0 0 auto;
  width: calc(100% - 64px);
  margin: 14px 32px 6px;
  padding: 8px 14px;
  font-size: 13px;
  font-family: inherit;
  color: var(--ink);
  border: 1px solid var(--ink);
  border-radius: 24px;
  outline: none;
  box-sizing: border-box;
  background: var(--surface);
}
.ring-search::placeholder { color: var(--muted); }
.ring-search:focus { border-color: var(--theme); }

/* 搜索结果层：浮在画布上方（App 是 Box 顶部对齐的浮层，左右各 12、圆角 14、阴影 5dp） */
.ring-search-results {
  position: absolute;
  left: 12px;
  right: 12px;
  top: 56px;
  z-index: 40;
  display: none;
  background: var(--surface);
  border-radius: 14px;
  box-shadow: 0 5px 16px rgba(var(--ink-rgb), .14), 0 1px 3px rgba(var(--ink-rgb), .06);
  max-height: 260px;
  overflow-y: auto;
  overscroll-behavior: contain;
}
.ring-search-results.open { display: block; }
.ring-search-results::-webkit-scrollbar { width: 0; }

/* 结果行：24 色点 + 标题 14/700 + 首行正文 11 + 右侧 ›（字号结构照 App HomeSearchResults） */
.rs-row {
  display: flex;
  align-items: center;
  padding: 10px 14px;
  cursor: pointer;
}
.rs-row:active { background: var(--gray-3); }
.rs-dot {
  flex: 0 0 auto;
  width: 24px;
  height: 24px;
  border-radius: 50%;
}
.rs-main {
  flex: 1 1 auto;
  min-width: 0;
  padding-left: 10px;
}
.rs-title {
  font-size: 14px;
  font-weight: 700;
  color: var(--ink);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.rs-sub {
  font-size: 11px;
  color: var(--muted);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.rs-arrow {
  flex: 0 0 auto;
  font-size: 16px;
  color: var(--faint);
}
.rs-empty {
  padding: 16px;
  font-size: 12px;
  color: var(--faint);
}
/* 圆主页视口：整张画布比屏幕大，这里是「局部视口」，
   左右 + 上下都能滑动（原生滚动，带惯性），用于挪动画布看全任意圆 */
.ring-scroll {
  position: relative;
  flex: 1 1 auto;
  overflow: auto;               /* 双轴滑动 */
  scrollbar-width: none;
  -webkit-overflow-scrolling: touch;
  overscroll-behavior: contain;
}
.ring-scroll::-webkit-scrollbar { display: none; }

/* 右下角「新建圆」快捷灰圆：悬浮、灰色，明显区别于内容圆 */
.ring-fab {
  position: absolute;
  right: 84px;              /* 与「？」互换位置后：新建紫圆在左 */
  bottom: 16px;
  width: 58px;
  height: 58px;
  border-radius: 50%;
  border: none;
  background: var(--theme);
  color: #FFFFFF;
  font-size: 28px;
  font-weight: 300;
  line-height: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 30;
  box-shadow: 0 6px 16px rgba(var(--ink-rgb), .16);
  padding: 0;
  font-family: inherit;
}
.ring-fab:active { transform: scale(.94); }

/* 「？」深蓝小圆：紧挨在「新建」紫圆左侧，点击直达「目录 → 产品介绍」（仅 Web 有，手机端不同步）
   问号字重/字号与右侧「＋」（28px / 300）保持一致的纤细感，并用 flex + line-height:1 居中 */
.ring-help {
  position: absolute;
  right: 16px;              /* 与「＋」互换位置后：蓝「？」在最右 */
  bottom: 16px;
  width: 58px;
  height: 58px;
  border-radius: 50%;
  border: none;
  background: #3567C9;      /* 蓝色（比深蓝浅一档） */
  color: #FFFFFF;
  font-size: 30px;
  font-weight: 300;
  line-height: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 30;
  box-shadow: 0 6px 16px rgba(var(--ink-rgb), .16);
  padding: 0;
  font-family: inherit;
}
.ring-help:active { transform: scale(.94); }

/* 圆：无边框、无描边 */
.circle {
  position: absolute;
  border-radius: 50%;
  overflow: hidden;              /* 保证圆内文字不会溢出圆外 */
  cursor: pointer;
  user-select: none;
  -webkit-user-select: none;
  -webkit-user-drag: none;
  transition: box-shadow .25s ease, opacity .18s ease;
  will-change: transform;
  touch-action: pan-y;           /* 保留纵向滚动，自定义拖拽在 JS 中接管 */
}
/* 新圆出现动画：缩放淡入 + 轻微弹入 */
.circle.appear { animation: circleAppear .46s cubic-bezier(.2, .9, .3, 1.35) both; }
@keyframes circleAppear {
  0%   { transform: scale(.35); opacity: 0; }
  55%  { transform: scale(1.09); opacity: 1; }
  78%  { transform: scale(.97); opacity: 1; }
  100% { transform: scale(1); opacity: 1; }
}
.circle .circle-text {
  position: absolute;
  left: 14%;
  right: 14%;
  top: 38%;
  text-align: center;
  pointer-events: none;
}
.circle.kids .circle-text { top: 46%; }

.c-title {
  font-size: 16px;
  font-weight: 700;
  line-height: 1.28;
  letter-spacing: .2px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  word-break: break-word;
}
.c-preview {
  margin-top: 4px;
  font-size: 10.5px;
  line-height: 1.35;
  opacity: .85;
  display: -webkit-box;
  -webkit-line-clamp: 1;      /* 预览小字：可截断，不显全 */
  -webkit-box-orient: vertical;
  overflow: hidden;
  word-break: break-word;
}

/* 空态圆：中央浅灰 ＋ */
.c-plus {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 34px;
  font-weight: 300;
  color: rgba(255, 255, 255, .75);
  pointer-events: none;
}

/* 父圆内的子圆（仅显示标题）；可点按、可长按拎出 */
.child-circle {
  position: absolute;
  border-radius: 50%;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  touch-action: none;
}
.child-circle .cc-title {
  width: 78%;
  text-align: center;
  font-size: 9.5px;
  font-weight: 700;
  line-height: 1.2;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  word-break: break-word;
  pointer-events: none;
}

.more-badge {
  position: absolute;
  left: 50%;
  top: 20%;
  transform: translateX(-50%);
  font-size: 9px;
  font-weight: 700;
  color: var(--muted);
  background: rgba(255, 255, 255, .78);
  border-radius: 999px;
  padding: 1px 5px;
  pointer-events: none;
}

/* 被提起时原位淡出 */
.circle.is-lifted { opacity: .16; }
.child-circle.is-lifted { opacity: .16; }

/* 命中目标 / 刚建立包含后的短暂高亮 */
.circle.is-target { box-shadow: 0 0 0 3px rgba(var(--ink-rgb), .14); }
.circle.flash { animation: flashRing .6s ease; }
@keyframes flashRing {
  0%   { box-shadow: 0 0 0 0 rgba(var(--ink-rgb), .22); }
  40%  { box-shadow: 0 0 0 8px rgba(var(--ink-rgb), .10); }
  100% { box-shadow: 0 0 0 14px rgba(var(--ink-rgb), 0); }
}

/* ---------------- Tab「万物」页（原「无限」页） ----------------
   布局原则：万物大圆居中 → 搜索栏 → 「探索」按钮 → 结果区（细分圆自然排列） */
.insp { height: 100%; display: flex; flex-direction: column; overflow: hidden; }
.insp-head {
  flex: 0 0 auto;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 12px 6px;
}
.insp-path {
  font-size: 11px; color: var(--muted);
  overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
  flex: 1 1 auto; text-align: right;
}

/* 上半区：只放万物大圆（控件已拆到 .insp-bottom，见下） */
.insp-top {
  flex: 0 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 16px 16px 4px;
}
/* 初始态：大圆圆心落在正中心 50%（尺寸与位移由 JS 按容器高度精确计算，此处 150px 仅为无 JS 兜底） */
.insp.is-root .insp-top { padding-top: 150px; }

/* 控件块：搜索栏 + 「探索」按钮 */
.insp-bottom {
  flex: 0 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 16px 4px;
}
/* 初始态：把控件整块推到底部（贴近 Tab 栏），大圆位置保持不变；
   展开态无此规则，控件紧跟小圆下方 —— 与 App InspireScreen 的 weight(1f) 弹性留白等价。
   注意必须同时让根态结果区不抢空间：flexbox 会先把自由空间分给 flex-grow，
   剩下的才轮到 auto margin，否则 .insp-stage 会把整块重新顶回圆下方。 */
.insp.is-root .insp-bottom { margin-top: auto; }
.insp.is-root .insp-stage { flex: 0 0 auto; }

/* 呼吸缩放：整个大圆缓慢放大缩小（与 App 同款，3.2s 往复）。
   根态和展开态都呼吸（App 的 InspireCoreCircle 无条件叠加呼吸动画）；
   纹裂时由更高特异性的 `.insp-core.crack` 覆盖 transform。 */
@keyframes inspBreathe {
  0%, 100% { transform: scale(1); }
  50%      { transform: scale(1.035); }
}
.insp-core {
  animation: inspBreathe 3.2s ease-in-out infinite;
  will-change: transform;
}

/* 「探索」按钮：紧跟搜索栏下方、水平居中（App：高 38dp、左右 28dp、2sp 字间距） */
.insp-explore-wrap { flex: 0 0 auto; display: flex; justify-content: center; margin-top: 10px; }
.insp-explore {
  border: 1px solid var(--line);
  background: var(--surface);
  border-radius: 999px;
  height: 38px;
  font-size: 12.5px;
  color: var(--muted);
  padding: 0 28px;
  cursor: pointer;
  font-family: inherit;
  letter-spacing: 2px;
}
.insp-explore:active { background: var(--gray-2); color: var(--ink); }

/* 「开启超算模式」：万物页**彻底离线**，这个按钮只剩名字，点了只提示「功能正在升级中」，
   不接任何网络、不改任何状态（App InspireNetworkButton / inspire_network_toast） */
.insp-net-wrap { flex: 0 0 auto; display: flex; justify-content: center; margin-top: 8px; }
.insp-net {
  border: 1px solid var(--line);
  background: var(--surface);
  border-radius: 999px;
  height: 34px;
  padding: 0 18px;
  font-size: 12px;
  color: var(--muted);
  letter-spacing: 1.5px;
  cursor: pointer;
  font-family: inherit;
}
.insp-net:active { background: var(--gray-2); color: var(--ink); }

/* 结果区：细分圆自然排列（按行），内容多时纵向滚动 */
.insp-stage {
  flex: 1 1 auto;
  position: relative;
  overflow-y: auto;
  overflow-x: hidden;
  padding: 10px 0 16px;
  scrollbar-width: none;
}
.insp-stage::-webkit-scrollbar { display: none; }
.insp-nodes { position: relative; margin: 0 auto; }

/* 万物圆：时刻水平居中，文字居中，圆下方不写任何注释 */
.insp-core {
  flex: 0 0 auto;
  width: 196px;
  height: 196px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  font-size: 28px;
  font-weight: 700;
  letter-spacing: 4px;
  cursor: pointer;
  user-select: none;
  -webkit-user-select: none;
  transition: width .22s ease, height .22s ease, font-size .22s ease;
}
/* 展开态：大圆收小，把空间让给结果区 */
.insp-core.compact { width: 100px; height: 100px; font-size: 18px; letter-spacing: 2px; }

/* 纹裂动画：抖动 + 放射状裂缝感 + 淡出 */
.insp-core.crack { animation: crackAnim .45s ease forwards; }
/* 纹裂必须盖过「呼吸」的 transform：用更高特异性(.insp.is-root .insp-core.crack)确保生效 */
.insp.is-root .insp-core.crack { animation: crackAnim .45s ease forwards; }
@keyframes crackAnim {
  0%   { transform: scale(1) rotate(0deg); opacity: 1; }
  25%  { transform: scale(1.04) rotate(-1.6deg); }
  45%  { transform: scale(1.07) rotate(1.4deg); }
  65%  { transform: scale(1.12) rotate(-.8deg); opacity: .9; }
  100% { transform: scale(1.35) rotate(0deg); opacity: 0; }
}

/* 自然排列出的圆：由 JS 计算的行列坐标定位在结果区内，弹入动画 */
.insp-node {
  position: absolute;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  cursor: pointer;
  user-select: none;
  -webkit-user-select: none;
  padding: 10px;
  font-weight: 700;
  line-height: 1.25;
  word-break: break-word;
  animation: popIn .38s cubic-bezier(.2, .9, .3, 1.35) both;
}
/* 下下层细分圆：略弱一档。
   必须用 filter 而不是 opacity —— popIn 的 `animation-fill-mode: both` 会把 opacity 定格在 1，
   写在元素上的 opacity 会被动画结果覆盖（等于没生效）。 */
.insp-node.lv2 { filter: opacity(.92); }
@keyframes popIn {
  0%   { transform: scale(0); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}
/* 圆内文字最多 3 行后省略（App：maxLines = 3 + TextOverflow.Ellipsis） */
.insp-node .insp-node-title {
  pointer-events: none;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* 搜索栏：挂在「万物」圆正下方（随上半区一起水平居中）。
   用户 PS 示意图：「解算」按钮嵌进输入框内部右端，输入框更宽 → 改为相对定位容器，按钮绝对定位到右端内侧 */
.insp-search { position: relative; display: flex; align-items: center; }
.insp-search.below-circle {
  flex: 0 0 auto;
  width: 100%;
  max-width: 340px;
  margin-top: 20px;
}
.insp-input {
  flex: 1 1 auto;
  width: 100%;
  min-width: 0;
  /* App 的 OutlinedTextField 把 focused/unfocused 描边色都设成和底色一样 → 视觉上没有描边；
     这里描边色跟着背景走，保持同样的「无边框胶囊」观感 */
  border: 1px solid var(--gray-3);
  outline: none;
  background: var(--gray-3);
  border-radius: 999px;
  height: 56px;          /* App OutlinedTextField 默认最小高度 56dp */
  padding: 0 64px 0 16px;  /* 右侧留出按钮空间，避免文字被「解算」盖住 */
  font-size: 12.5px;
  font-family: inherit;
  color: var(--ink);
}
.insp-input::placeholder { color: var(--faint); }
.insp-go {
  position: absolute;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
  flex: 0 0 auto;
  border: 1px solid var(--line);
  background: var(--surface);
  border-radius: 999px;
  height: 38px;
  font-size: 12px;
  color: #7B5EA7;
  padding: 0 14px;
  letter-spacing: 2px;   /* App：letterSpacing = 2.sp */
  cursor: pointer;
  font-family: inherit;
}
.insp-go:active { background: var(--gray-2); }

/* 长按细分圆弹出的「词条简介」（App WordBriefDialog）：白卡 16 圆角 + 右下紫色胶囊「知道了」。
   万物已彻底离线，正文恒为「离线模式下暂不提供词条简介」，不走任何网络。 */
.insp-brief-mask {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, .32);
  display: none;
  align-items: center;
  justify-content: center;
  padding: 24px;
  z-index: 40;
}
.insp-brief-mask.open { display: flex; }
.insp-brief-card {
  width: 100%;
  max-width: 300px;
  background: var(--surface);
  border-radius: 16px;
  padding: 18px 20px;
}
.insp-brief-title {
  font-size: 15px;
  font-weight: 700;
  color: var(--ink);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.insp-brief-body {
  margin-top: 10px;
  min-height: 72px;
  max-height: 260px;
  overflow-y: auto;
  font-size: 13px;
  line-height: 20px;
  color: var(--ink);
}
.insp-brief-foot { margin-top: 16px; display: flex; justify-content: flex-end; }
.insp-brief-ok {
  border: none;
  border-radius: 999px;
  background: var(--theme);
  color: #FFFFFF;
  font-size: 12px;
  font-family: inherit;
  padding: 8px 18px;
  cursor: pointer;
}

/* ---------------- Tab 新建页 / 详情页（共用同一套编辑面板） ---------------- */
/* 详情页 / 新建页共用的编辑面板：占满父容器剩余高度（头部之外的部分） */
.pane { display: flex; flex-direction: column; flex: 1 1 auto; min-height: 0; }
/* 富文本区自适应填充剩余高度；子圆板块 + 工具条固定在屏幕下方 */
.pane .rich-editor { flex: 1 1 auto; min-height: 60px; }
.pane .strips-wrap { flex: 0 0 auto; }
.pane .rich-toolbar { flex: 0 0 auto; }

.ed-title-input {
  flex: 0 0 auto;
  width: 100%;
  border: none;
  outline: none;
  background: transparent;
  font-family: inherit;
  color: var(--ink);
  resize: none;
  padding: 6px 18px 6px;
  font-size: 19px;
  font-weight: 700;
  letter-spacing: .3px;
}
.ed-title-input::placeholder { color: var(--faint); font-weight: 700; }

/* 竖向小板块（竖条）横滑区：始终固定在屏幕下方 */
.strips-wrap {
  flex: 0 0 auto;
  border-top: 1px solid var(--line);
  padding: 14px 0 12px;
  background: var(--surface);
}
.strips-title {
  font-size: 11px;
  color: var(--muted);
  padding: 0 18px 8px;
  letter-spacing: .5px;
}
.strips {
  display: flex;
  gap: 10px;
  overflow-x: auto;
  padding: 4px 18px 6px;
  scrollbar-width: none;
  -webkit-overflow-scrolling: touch;
}
.strips::-webkit-scrollbar { display: none; }

/* 竖条卡片 96×130，与手机端 editor.css 的 .strip 同值 */
.strip {
  flex: 0 0 auto;
  width: 96px;
  height: 130px;
  border-radius: 14px;
  padding: 11px 11px 12px;
  display: flex;
  flex-direction: column;
  cursor: pointer;
  overflow: hidden;
  position: relative;
}
.strip .s-title {
  font-size: 13px; font-weight: 700; line-height: 1.3;
  display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden;
  word-break: break-word;
}
.strip .s-preview {
  margin-top: 5px; font-size: 10px; line-height: 1.4;
  opacity: .85;
  display: -webkit-box; -webkit-line-clamp: 3; -webkit-box-orient: vertical; overflow: hidden;
  word-break: break-word;
}
.strip .s-idx { margin-top: auto; font-size: 9.5px; opacity: .7; }
/* 「＋」新建竖条：紧跟在已建子圆后边。「＋」用淡紫虚线块（同 THEME_ACCENT 一系），
   与手机端 editor.css 的 .strip-new 逐值一致（#F7F1FC / #DCC7EC / #C9A0DC / 26px）。 */
.strip-new {
  flex: 0 0 auto;
  width: 52px;
  height: 130px;
  border-radius: 14px;
  background: #F7F1FC;
  border: 1px dashed #DCC7EC;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 26px;
  font-weight: 300;
  color: #C9A0DC;
}
.strip-new.disabled { opacity: .45; cursor: not-allowed; }

/* ---------------- 富文本工具条（新建页：位于子圆板块下方） ---------------- */
.rich-toolbar {
  flex: 0 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  overflow-x: auto;
  scrollbar-width: none;
  padding: 4px 14px 6px;
}
.rich-toolbar::-webkit-scrollbar { display: none; }
.rich-toolbar.bottom {
  border-top: 1px solid var(--line);
  /* 底部留 16px + 安全区，保证小屏 / 手势条不吞掉按钮（同手机端 editor.css）；
     2026-10-29：按钮 48→40px、图标 28→22px（与手机端同步缩小一档） */
  padding: 8px 14px calc(16px + env(safe-area-inset-bottom, 0px));
}
.rt-btn {
  flex: 0 0 auto;
  min-width: 40px;
  height: 40px;
  border: 1px solid var(--line);
  background: var(--surface);
  border-radius: 8px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 13px;
  font-weight: 700;
  color: var(--muted);
  font-family: inherit;
  padding: 0 8px;
}
.rt-btn:active, .rt-btn.on { background: var(--gray-2); color: var(--ink); }
.rt-btn svg { width: 22px; height: 22px; stroke: currentColor; fill: none; stroke-width: 1.7; stroke-linecap: round; stroke-linejoin: round; }
.rt-sep { flex: 0 0 auto; width: 1px; height: 24px; background: var(--line); }
/* 文字颜色：单个色块图标（16×16 圆角方块，2026-10-29 同步缩小），点击弹 16 色色卡小窗口 */
.rt-color { padding: 0 10px; }
.rt-color .rt-color-bar { width: 16px; height: 16px; border-radius: 4px; }

.rich-editor {
  flex: 1 1 auto;
  overflow-y: auto;
  outline: none;
  font-size: 13.5px;
  line-height: 1.75;
  padding: 4px 18px 10px;
  min-height: 60px;
  color: var(--ink);
  word-break: break-word;
}
.rich-editor:empty::before {
  content: attr(data-placeholder);
  color: var(--faint);
  pointer-events: none;
}
.rich-editor img.nb-img { max-width: 100%; border-radius: 10px; margin: 6px 0; display: block; }
.nb-table { border-collapse: collapse; margin: 8px 0; width: 100%; }
.nb-table td, .nb-table th {
  border: 1px solid var(--muted);
  padding: 5px 8px;
  font-size: 12px;
  min-width: 34px;
}

/* ---------------- 弹窗（表格行列 / 插图来源） ---------------- */
.modal-mask {
  position: absolute;
  inset: 0;
  background: rgba(var(--ink-rgb), .35);
  display: none;
  align-items: center;
  justify-content: center;
  z-index: 70;
  border-radius: 34px;
}
.modal-mask.open { display: flex; }
.modal {
  background: var(--surface);
  border-radius: 16px;
  padding: 16px;
  width: 264px;
  box-shadow: 0 12px 34px rgba(var(--ink-rgb), .18);
}
.modal-title { font-size: 13.5px; font-weight: 700; color: var(--ink); margin-bottom: 12px; }
.modal-text { font-size: 13px; line-height: 1.5; color: var(--muted); margin-bottom: 14px; }
.modal-row { display: flex; align-items: center; gap: 8px; margin-bottom: 10px; font-size: 12px; color: var(--muted); }
.modal-row input {
  width: 64px;
  border: 1px solid var(--line);
  border-radius: 8px;
  padding: 6px 8px;
  font-size: 13px;
  font-family: inherit;
  outline: none;
  color: var(--ink);
}
.modal-btns { display: flex; justify-content: flex-end; gap: 8px; margin-top: 6px; }
.modal-btn {
  border: 1px solid var(--line);
  background: var(--surface);
  border-radius: 999px;
  font-size: 12px;
  color: var(--muted);
  padding: 6px 14px;
  cursor: pointer;
  font-family: inherit;
}
.modal-btn.primary { background: var(--ink); color: #FFFFFF; border-color: var(--ink); }
.modal-btn.danger { color: var(--danger); border-color: var(--danger); }
/* 主题紫实心胶囊（设置弹窗的「隐私政策」/ 引导页的「下一页」），与 App Button(containerColor=THEME_PRIMARY) 一致 */
.modal-btn.theme { background: var(--theme); color: #FFFFFF; border-color: var(--theme); font-size: 13px; }
/* 纯文字按钮（「关闭」/「跳过」）：App 用 TextButton，只有文字没有边框 */
.modal-btn.text { border-color: transparent; background: transparent; padding: 6px 8px; }
.modal-btn:disabled { opacity: .38; cursor: default; }
.modal-btn.block { display: block; width: 100%; margin-bottom: 8px; padding: 9px 0; }

/* 设置 / 隐私政策 / 产品介绍弹窗（对应 App SettingsDialog / OnboardingScreen） */
.modal-title-lg { font-size: 17px; }
.modal-wide { width: 300px; }
.modal-info { font-size: 12.5px; color: var(--ink); margin-bottom: 14px; }
/* 隐私政策正文：完整可滚动（App: heightIn(min=200,max=420) + verticalScroll） */
.modal-scroll {
  max-height: 420px;
  overflow: auto;
  font-size: 12.5px;
  line-height: 1.7;
  color: var(--ink);
  white-space: pre-wrap;
  overflow-wrap: break-word;
  margin-bottom: 10px;
  -webkit-overflow-scrolling: touch;
}

/* 产品介绍（5 页引导轮播）：标题大粗 + 灰色说明 + 圆点指示 */
.guide-art {
  width: 100%;
  display: flex;
  justify-content: center;
}
/* 逻辑画布 300×260，随弹窗宽度等比缩放（保持与手机端 260dp 插画比例一致） */
.guide-art canvas {
  width: 100%;
  max-width: 268px;
  height: auto;
  aspect-ratio: 300 / 260;
  display: block;
}
.guide-body {
  min-height: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
  text-align: center;
  padding: 2px 2px 0;
}
.guide-core {
  width: 66px; height: 66px;
  border-radius: 50%;
  background: var(--theme);
  color: #FFFFFF;
  font-size: 15px; font-weight: 700;
  display: flex; align-items: center; justify-content: center;
  letter-spacing: 1px;
}
.guide-title { font-size: 19px; font-weight: 700; color: var(--ink); line-height: 1.45; }
.guide-desc { font-size: 13px; color: var(--muted); line-height: 1.7; white-space: pre-line; }
/* 末页（LOCK 插画）展示的「本版完全免费」徽章：与手机端 onboard_unlock_badge 对齐 */
.guide-badge {
  display: none;
  align-items: center;
  justify-content: center;
  margin: 2px auto 0;
  padding: 5px 14px;
  border-radius: 999px;
  background: var(--theme);
  color: #FFFFFF;
  font-size: 12px; font-weight: 700; letter-spacing: 1px;
}
.guide-badge.show { display: flex; }
.guide-foot {
  display: flex; align-items: center; justify-content: space-between;
  gap: 8px; margin-top: 14px;
}
.guide-dots { display: flex; align-items: center; gap: 5px; }
.guide-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--line); transition: width .15s ease; }
.guide-dot.on { width: 14px; border-radius: 999px; background: var(--theme); }
.guide-ctrls { display: flex; gap: 6px; }
.guide-ctrls .modal-btn { padding: 6px 11px; }

/* 产品介绍：全屏白页（对齐手机首启引导，而不是居中弹窗）。
   #guideModal 是全屏遮罩，覆盖整屏（含底部 tab）；这里去掉变暗、让弹窗铺满整屏。 */
#guideModal { background: var(--surface); }
#guideModal .modal.guide-sheet {
  width: 100%;
  height: 100%;
  max-width: none;
  background: var(--surface);
  border-radius: 34px;
  box-shadow: none;
  padding: 30px 24px 22px;
  display: flex;
  flex-direction: column;
}
/* art 与 foot 各带一个 auto 上边距：把「插画+标题」块在剩余空间里垂直居中（近似手机首启页） */
#guideModal .guide-art { margin-top: auto; }
#guideModal .guide-art canvas { max-width: 300px; }
#guideModal .guide-body { margin-top: 20px; }
#guideModal .guide-badge { margin-top: 14px; }
#guideModal .guide-foot { margin-top: auto; }

/* 文字颜色弹窗里的 16 色色卡：4 列 × 4 行网格 */
.color-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  justify-items: center;
}
.color-cell {
  width: 34px;
  height: 34px;
  border-radius: 50%;
  border: 1px solid var(--line);
  padding: 0;
  cursor: pointer;
  font-family: inherit;
}
.color-cell:active { transform: scale(.92); }

/* ---------------- 绘图（手写）覆盖层 ---------------- */
.draw-overlay {
  position: absolute;
  inset: 0;
  background: rgba(255, 255, 255, .96);
  display: none;
  flex-direction: column;
  z-index: 65;
  border-radius: 34px;
}
.draw-overlay.open { display: flex; }
.draw-canvas-wrap { flex: 1 1 auto; position: relative; }
/* canvas 的 CSS 尺寸由 JS 按容器布局尺寸显式写入，坐标换算见 app.js canvasPointOf */
.draw-canvas-wrap canvas {
  position: absolute;
  left: 0;
  top: 0;
  touch-action: none;
  cursor: crosshair;
}
.draw-bar {
  flex: 0 0 auto;
  display: flex;
  gap: 8px;
  justify-content: flex-end;
  padding: 10px 14px 14px;
  border-top: 1px solid var(--line);
}

/* ---------------- 详情页（覆盖全屏） ---------------- */
.detail-layer {
  position: absolute;
  inset: 0;
  background: var(--surface);
  display: none;
  flex-direction: column;
  z-index: 40;
  border-radius: 34px;
  overflow: hidden;
}
.detail-layer.open { display: flex; }
/* 详情页为独立路由，打开时隐藏底部 Tab 栏（与原生 App 的 NavHost 行为一致） */
.detail-layer.open ~ .tabbar { display: none; }

.detail-head {
  flex: 0 0 auto;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 12px 6px;
}
/* 与手机端 editor.css 的 .dh-btn 同值：34×34（2026-10-29 顶栏图标整体放大一档） */
.dh-btn {
  width: 34px; height: 34px; border-radius: 50%;
  border: 1px solid var(--line); background: var(--surface);
  display: flex; align-items: center; justify-content: center;
  cursor: pointer; flex: 0 0 auto;
  font-size: 15px; color: var(--muted); font-family: inherit;
}
/* 顶栏图标改矢量：24 视口、1.8 描边、圆头圆角，与底部工具条同一套语言 */
.dh-btn svg {
  width: 19px; height: 19px;
  stroke: currentColor; fill: none;
  stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round;
}
.dh-btn:active { background: var(--gray-2); }
/* 「新建子」：灰改淡紫。取的是底部「万」Tab 未选中时那个淡紫（THEME_ACCENT #C9A0DC），
   不是进入万物页后加深的主题紫 #7B5EA7 —— 与手机端 editor.css 的 .dh-add 逐值一致。 */
.dh-btn.dh-add { color: #C9A0DC; border-color: #E4D6F2; background: #FBF8FE; }
/* 「保存」是文字按钮，比圆形图标按钮宽，用胶囊形区分；主题紫色描边 + 文字 */
.dh-btn.dh-save {
  width: auto; height: 34px;
  padding: 0 12px; border-radius: 999px;
  font-size: 13px; font-weight: 700; color: #7B5EA7;
  border: 1px solid #7B5EA7;
}
/* 撤回 / 重做：颜色比其它顶栏按钮重一档（这两个是高频操作）。
   图标改为 svg 后字号不再起作用，只保留配色。 */
.dh-btn.dh-hist { color: var(--ink); }
.dh-path {
  font-size: 10.5px; color: var(--muted);
  overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
  flex: 1 1 auto; text-align: right;
  /* 顶部一行有 ← ＋ ↶ ↷ 四个圆按钮 + 路径，flex 子项默认 min-width:auto（=内容宽度），
     中文路径不换行时会把右侧内容挤出屏幕；置 0 才会收缩并让 ellipsis 生效。 */
  min-width: 0;
}

/* ---------------- 底部 Tab 栏（4 个，尺寸对齐 App Material3 NavigationBar：80dp 高） ---------------- */
.tabbar {
  flex: 0 0 auto;
  height: var(--tabbar-h);
  display: flex;
  background: var(--surface);
  position: relative;
  z-index: 50;
}
.tab {
  flex: 1 1 0;
  border: none;
  background: none;
  font-family: inherit;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 5px;
  cursor: pointer;
  color: var(--faint);
  padding: 0;
}
.tab svg { width: 28px; height: 28px; fill: none; stroke: currentColor; stroke-width: 1.6; stroke-linecap: round; stroke-linejoin: round; }
/* 万物 Tab 用汉字字形：字重 900 + 纵向压扁 + 横向微展，与其余 stroke 线条图标的视觉重量对齐；
   颜色继承 .tab（active 时自动变深），且不受 .tab svg 的尺寸规则影响 */
.tab .tab-glyph {
  font-size: 19px;
  font-weight: 900;
  line-height: 1;
  transform: scaleY(.78) scaleX(1.06);
  transform-origin: center;
  color: inherit;
  letter-spacing: 0;
  font-family: inherit;
}
.tab span { font-size: 10.5px; letter-spacing: .5px; }
.tab.active { color: var(--ink); }
.tab.active svg { stroke-width: 2; }
/* 万物「万」图标：强制主题紫（需求 #4/#29，与手机端一致） */
.tab[data-tab="inspire"] { color: var(--accent); }
.tab[data-tab="inspire"].active { color: var(--theme); }

/* ---------------- Toast ---------------- */
.toast {
  position: absolute;
  left: 50%;
  bottom: 96px;   /* = --tabbar-h(80) + 16：toast 必须浮在 Tab 栏之上 */
  transform: translateX(-50%) translateY(10px);
  background: rgba(var(--ink-rgb), .88);
  color: #FFFFFF;
  font-size: 12px;
  padding: 8px 15px;
  border-radius: 999px;
  opacity: 0;
  pointer-events: none;
  transition: opacity .2s ease, transform .2s ease;
  z-index: 80;
  white-space: nowrap;
  max-width: 300px;
  overflow: hidden;
  text-overflow: ellipsis;
}
.toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

/* ---------------- 拖拽层 ---------------- */
.drag-layer {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 9999;
}
.drag-layer .circle { position: fixed; left: 0; top: 0; transform-origin: 50% 50%; }
/* 提起态：App 是 graphicsLayer(scale = 1.03 + wobble × 0.03、rotationZ = wobble × 2°，wobble 400ms 往返)。
   所以 scale 在 **1.00 ↔ 1.06** 之间摆、旋转 ±2°，一个来回 800ms —— 逐帧照抄，别写成固定 1.03。
   这里把缩放任交给关键帧，clone 的位移只用 left / top（见 app.js moveClone） */
.drag-layer .circle.lifting {
  box-shadow: 0 14px 30px rgba(var(--ink-rgb), .18);
  animation: liftWobble .4s ease-in-out infinite alternate;
}
@keyframes liftWobble {
  from { transform: scale(1) rotate(-2deg); }
  to   { transform: scale(1.06) rotate(2deg); }
}
.drag-layer .circle.rebound {
  transition: transform .22s cubic-bezier(.2, .8, .3, 1),
              left .22s cubic-bezier(.2, .8, .3, 1),
              top .22s cubic-bezier(.2, .8, .3, 1);
  box-shadow: none;
}

/* 拖拽删除目标圆（与手机端一致：58px、#5E616D、悬停 #3A3B40 放大 1.12） */
.drag-layer .delete-target {
  position: fixed;
  width: 58px;
  height: 58px;
  border-radius: 50%;
  background: #5E616D;
  color: #FFFFFF;
  font-size: 12px;
  font-weight: 700;
  letter-spacing: .5px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 8px 18px rgba(0, 0, 0, .18);
  transition: transform .12s ease, background .12s ease;
  transform-origin: 50% 50%;
  animation: delPop .15s ease-out;
}
.drag-layer .delete-target.hover { background: #3A3B40; transform: scale(1.12); }
@keyframes delPop {
  from { transform: scale(.6); opacity: 0; }
  to   { transform: scale(1);  opacity: 1; }
}

.page-foot { font-size: 11.5px; color: var(--muted); text-align: center; max-width: 640px; line-height: 1.7; }

/* 窄屏时整体缩放，保证手机框完整可见 */
@media (max-height: 1000px) { .phone-wrap { transform: scale(.9); } }
@media (max-height: 900px)  { .phone-wrap { transform: scale(.8); } }
@media (max-height: 800px)  { .phone-wrap { transform: scale(.72); } }
@media (max-height: 720px)  { .phone-wrap { transform: scale(.64); } }

/* ---------------- 移动端 / 小程序 web-view 全屏适配 ----------------
   在手机或微信 web-view 里，去掉桌面端的「手机外框」与页头，让 .screen 直接铺满视口，
   否则 web-view 里会出现「手机套手机」的双层边框。 */
@media (max-width: 600px) {
  html, body { margin: 0; padding: 0; background: var(--bg); }
  .page { padding: 0; }
  .page-head { display: none; }
  .phone-wrap { margin: 0; transform: none !important; }
  .phone {
    width: 100%;
    height: 100vh;
    height: 100dvh;
    border: none;
    border-radius: 0;
    box-shadow: none;
    padding: 0;
  }
  .screen {
    border-radius: 0;
    padding-top: env(safe-area-inset-top);
  }
}

</style>
</head>
<body>

<div class="page">
  <header class="page-head">
    <h1>万物笔记 · 交互演示</h1>
    <p class="sub">Web 高保真演示，非真实 APK</p>
    <p class="page-hint">圆页面移动：Web 端用键盘 <kbd>←</kbd> / <kbd>→</kbd> 平移；手机 APK 单指拖动移动、双指缩放。</p>
  </header>

  <div class="phone-wrap">
    <div class="phone">
      <div class="screen">

        <div class="screen-body">
          <!-- Tab1 目录 / 菜单 -->
          <section class="view" id="view-catalog"></section>

          <!-- Tab2 圆主页（核心） -->
          <section class="view" id="view-ring">
            <!-- 顶部搜索栏：与 App HomeScreen 的 OutlinedTextField 一致
                 （左右各 32、上 14 下 6、圆角 24、13sp、边框=主文字色 / 聚焦=主题紫）。
                 App 首页没有说明提示行，原先那行 10.5px 的提示已整行删除。 -->
            <input class="ring-search" id="ringSearch" type="text" placeholder="搜索笔记标题或内容…" />
            <div class="ring-search-results" id="ringResults"></div>
            <div class="ring-scroll" id="ringScroll"></div>
            <!-- 右下角紫色「新建圆」快捷圆（悬浮，明显区别于内容圆） -->
            <button class="ring-fab" id="ringFab" type="button" title="新建圆">＋</button>
            <!-- 「？」深蓝小圆：紧挨在新建紫圆左侧，点击直达「目录 → 产品介绍」引导界面（仅 Web 有，手机端不同步） -->
            <button class="ring-help" id="ringHelp" type="button" title="产品介绍">?</button>
          </section>

          <!-- Tab3 万物：万物大圆 + 搜索栏解算 + 探索按钮 + 细分圆自然排列 -->
          <section class="view" id="view-inspire"></section>

          <!-- Tab4 新建快捷 -->
          <section class="view" id="view-new"></section>
        </div>

        <!-- 搜索面板（目录页搜索图标触发，覆盖在屏幕内） -->
        <div class="search-panel" id="searchPanel"></div>

        <!-- 详情页覆盖层 -->
        <div class="detail-layer" id="detailLayer"></div>

        <!-- 回收站覆盖层（目录页图标进入；不做第 5 个 tab，与 App MainActivity 的 showTrash 一致） -->
        <div class="trash-layer" id="trashLayer"></div>

        <!-- 彻底删除二次确认（回收站「删除」/「清空」共用） -->
        <div class="modal-mask" id="trashModal">
          <div class="modal">
            <div class="modal-title">彻底删除</div>
            <div class="modal-text" id="trashModalText"></div>
            <div class="modal-btns">
              <button class="modal-btn" type="button" data-role="cancel">取消</button>
              <button class="modal-btn danger" type="button" data-role="ok">删除</button>
            </div>
          </div>
        </div>

        <!-- 设置弹窗（目录页「设置」入口，对应 App ui/settings/SettingsDialog.kt） -->
        <div class="modal-mask" id="settingsModal">
          <div class="modal">
            <div class="modal-title modal-title-lg">设置</div>
            <div class="modal-info">万物笔记 · 版本 1.0（免费版）</div>
            <button class="modal-btn theme block" type="button" data-role="privacy">隐私政策</button>
            <div class="modal-btns">
              <button class="modal-btn text" type="button" data-role="close">关闭</button>
            </div>
          </div>
        </div>

        <!-- 隐私政策（设置的二级弹窗；正文由 app.js 的 PRIVACY_POLICY_TEXT 全文写入，非「详见 App」） -->
        <div class="modal-mask" id="privacyModal">
          <div class="modal modal-wide">
            <div class="modal-title modal-title-lg">隐私政策</div>
            <div class="modal-scroll" id="privacyBody"></div>
            <div class="modal-btns">
              <button class="modal-btn theme" type="button" data-role="close">关闭</button>
            </div>
          </div>
        </div>

        <!-- 产品介绍：5 页引导轮播（对应 App ui/onboarding/OnboardingScreen.kt） -->
        <div class="modal-mask" id="guideModal">
          <div class="modal modal-wide guide-sheet">
            <!-- 5 页动态插画（对齐 App OnboardingIllustrations.kt） -->
            <div class="guide-art"><canvas id="guideArt"></canvas></div>
            <div class="guide-body" id="guideBody"></div>
            <div class="guide-badge" id="guideBadge">本版完全免费</div>
            <div class="guide-foot">
              <div class="guide-dots" id="guideDots"></div>
              <div class="guide-ctrls">
                <button class="modal-btn" type="button" id="guidePrev" data-role="prev">上一页</button>
                <button class="modal-btn theme" type="button" id="guideNext" data-role="next">下一页</button>
              </div>
            </div>
            <div class="modal-btns">
              <button class="modal-btn text" type="button" data-role="skip">跳过</button>
            </div>
          </div>
        </div>

        <!-- 目录页长按「全部笔记」行 → 删除二次确认（对应 App CatalogScreen 的 AlertDialog） -->
        <div class="modal-mask" id="catDeleteModal">
          <div class="modal">
            <div class="modal-title">删除笔记</div>
            <div class="modal-text" id="catDeleteText"></div>
            <div class="modal-btns">
              <button class="modal-btn" type="button" data-role="cancel">取消</button>
              <button class="modal-btn danger" type="button" data-role="ok">删除</button>
            </div>
          </div>
        </div>

        <!-- 插入表格弹窗 -->
        <div class="modal-mask" id="tableModal">
          <div class="modal">
            <div class="modal-title">插入表格</div>
            <div class="modal-row">行数 <input type="number" data-role="rows" min="1" max="12" value="2" /></div>
            <div class="modal-row">列数 <input type="number" data-role="cols" min="1" max="6" value="2" /></div>
            <div class="modal-btns">
              <button class="modal-btn" type="button" data-role="cancel">取消</button>
              <button class="modal-btn primary" type="button" data-role="ok">插入</button>
            </div>
          </div>
        </div>

        <!-- 插入图片弹窗（拍照 / 相册） -->
        <div class="modal-mask" id="imageModal">
          <div class="modal">
            <div class="modal-title">插入图片</div>
            <button class="modal-btn block" type="button" data-role="camera">拍照</button>
            <button class="modal-btn block" type="button" data-role="album">从相册选择图片</button>
            <div class="modal-btns">
              <button class="modal-btn" type="button" data-role="cancel">取消</button>
            </div>
          </div>
        </div>

        <!-- 文字颜色弹窗（工具条「颜色」图标触发，16 色色卡） -->
        <div class="modal-mask" id="colorModal">
          <div class="modal">
            <div class="modal-title">文字颜色</div>
            <div class="color-grid" id="colorGrid"></div>
            <div class="modal-btns">
              <button class="modal-btn" type="button" data-role="cancel">取消</button>
            </div>
          </div>
        </div>

        <!-- 绘图（手写）覆盖层 -->
        <div class="draw-overlay" id="drawOverlay">
          <div class="draw-canvas-wrap"><canvas></canvas></div>
          <div class="draw-bar">
            <button class="modal-btn" type="button" data-role="clear">清空</button>
            <button class="modal-btn" type="button" data-role="cancel">取消</button>
            <button class="modal-btn primary" type="button" data-role="done">完成</button>
          </div>
        </div>

        <div class="toast" id="toastEl"></div>

        <nav class="tabbar" id="tabbar">
          <button class="tab" type="button" data-tab="catalog">
            <svg viewBox="0 0 24 24"><path d="M4 6h16M4 12h16M4 18h16"/></svg>
            <span>目录</span>
          </button>
          <button class="tab active" type="button" data-tab="ring">
            <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="8.2"/><circle cx="12" cy="12" r="3.2"/></svg>
            <span>圆</span>
          </button>
          <button class="tab" type="button" data-tab="inspire">
            <!-- 万物：黑体「万」字稍微压扁，与其余 stroke 线条图标的视觉重量对齐 -->
            <span class="tab-glyph">万</span>
            <span>万物</span>
          </button>
          <button class="tab" type="button" data-tab="new">
            <svg viewBox="0 0 24 24"><path d="M12 5v14M5 12h14"/></svg>
            <span>新建</span>
          </button>
        </nav>

      </div>
    </div>
  </div>

</div>

<div class="drag-layer" id="dragLayer"></div>

<!-- 隐藏的文件选择器：拍照 / 相册（插入图片用） -->
<input type="file" id="imgCameraInput" accept="image/*" capture="environment" style="display:none" />
<input type="file" id="imgAlbumInput" accept="image/*" style="display:none" />

<script>
/* ============================================================
   灵感页 · 离线内置分类树数据（混合方案之「离线为主」）
   结构：{ id, title, children: [...] }
   层级：万物 → 大类（宇宙/人文/经济/…）→ 子类 → 孙类（≥3 层）
   说明：将来若接入 LLM 在线展开，无需改动本文件，
        只需在 app.js 中打开 INSPIRATION_USE_REMOTE 开关并实现
        fetchChildrenRemote() 即可（详见 app.js 注释）。
   ============================================================ */
window.INSPIRATION_TREE = {
  id: 'wanwu',
  title: '万物',
  children: [
    {
      id: 'yuzhou', title: '宇宙',
      children: [
        { id: 'yuzhou-xingxi', title: '星系', children: [
          { id: 'yuzhou-xingxi-yinhe', title: '银河系', children: [] },
          { id: 'yuzhou-xingxi-hewan', title: '河外星系', children: [] },
          { id: 'yuzhou-xingxi-tuan', title: '星系团', children: [] }
        ]},
        { id: 'yuzhou-hengxing', title: '恒星', children: [
          { id: 'yuzhou-hengxing-mainseq', title: '主序星', children: [] },
          { id: 'yuzhou-hengxing-juxing', title: '红巨星', children: [] },
          { id: 'yuzhou-hengxing-zhongzi', title: '中子星', children: [] }
        ]},
        { id: 'yuzhou-hexing', title: '黑洞', children: [
          { id: 'yuzhou-hexing-shijian', title: '事件视界', children: [] },
          { id: 'yuzhou-hexing-xiji', title: '吸积盘', children: [] }
        ]},
        { id: 'yuzhou-xingxing', title: '行星', children: [
          { id: 'yuzhou-xingxing-leidi', title: '类地行星', children: [] },
          { id: 'yuzhou-xingxing-jumu', title: '气态巨行星', children: [] }
        ]},
        { id: 'yuzhou-yuzhouxue', title: '宇宙学', children: [
          { id: 'yuzhou-yuzhouxue-dabaozha', title: '大爆炸', children: [] },
          { id: 'yuzhou-yuzhouxue-anwuzhi', title: '暗物质', children: [] },
          { id: 'yuzhou-yuzhouxue-anengliang', title: '暗能量', children: [] }
        ]}
      ]
    },
    {
      id: 'renwen', title: '人文',
      children: [
        { id: 'renwen-zhexue', title: '哲学', children: [
          { id: 'renwen-zhexue-lunli', title: '伦理学', children: [] },
          { id: 'renwen-zhexue-cunzai', title: '存在主义', children: [] },
          { id: 'renwen-zhexue-bianzheng', title: '辩证法', children: [] }
        ]},
        { id: 'renwen-wenxue', title: '文学', children: [
          { id: 'renwen-wenxue-shige', title: '诗歌', children: [] },
          { id: 'renwen-wenxue-xiaoshuo', title: '小说', children: [] },
          { id: 'renwen-wenxue-sanwen', title: '散文', children: [] }
        ]},
        { id: 'renwen-shehui', title: '社会学', children: [
          { id: 'renwen-shehui-jieceng', title: '社会分层', children: [] },
          { id: 'renwen-shehui-qunti', title: '群体行为', children: [] }
        ]},
        { id: 'renwen-xinli', title: '心理学', children: [
          { id: 'renwen-xinli-renzhi', title: '认知心理', children: [] },
          { id: 'renwen-xinli-qingxu', title: '情绪机制', children: [] }
        ]}
      ]
    },
    {
      id: 'jingji', title: '经济',
      children: [
        { id: 'jingji-hongguan', title: '宏观经济', children: [
          { id: 'jingji-hongguan-huobi', title: '货币政策', children: [] },
          { id: 'jingji-hongguan-caizheng', title: '财政政策', children: [] },
          { id: 'jingji-hongguan-zhouqi', title: '经济周期', children: [] }
        ]},
        { id: 'jingji-weiguan', title: '微观经济', children: [
          { id: 'jingji-weiguan-gongxu', title: '供需关系', children: [] },
          { id: 'jingji-weiguan-boya', title: '博弈论', children: [] }
        ]},
        { id: 'jingji-jinrong', title: '金融', children: [
          { id: 'jingji-jinrong-gushi', title: '股市', children: [] },
          { id: 'jingji-jinrong-zhaiquan', title: '债券', children: [] },
          { id: 'jingji-jinrong-yansheng', title: '衍生品', children: [] }
        ]},
        { id: 'jingji-hangye', title: '产业', children: [
          { id: 'jingji-hangye-hulianwang', title: '互联网', children: [] },
          { id: 'jingji-hangye-zhizao', title: '制造业', children: [] }
        ]}
      ]
    },
    {
      id: 'huanjing', title: '环境',
      children: [
        { id: 'huanjing-qihou', title: '气候', children: [
          { id: 'huanjing-qihou-biannuan', title: '全球变暖', children: [] },
          { id: 'huanjing-qihou-tan', title: '碳排放', children: [] }
        ]},
        { id: 'huanjing-shengtai', title: '生态', children: [
          { id: 'huanjing-shengtai-duoyang', title: '生物多样性', children: [] },
          { id: 'huanjing-shengtai-xitong', title: '生态系统', children: [] }
        ]},
        { id: 'huanjing-nengyuan', title: '能源', children: [
          { id: 'huanjing-nengyuan-taiyang', title: '太阳能', children: [] },
          { id: 'huanjing-nengyuan-feng', title: '风能', children: [] },
          { id: 'huanjing-nengyuan-he', title: '核能', children: [] }
        ]},
        { id: 'huanjing-wuran', title: '污染治理', children: [
          { id: 'huanjing-wuran-haiyang', title: '海洋塑料', children: [] },
          { id: 'huanjing-wuran-kongqi', title: '空气质量', children: [] }
        ]}
      ]
    },
    {
      id: 'shuxue', title: '数学',
      children: [
        { id: 'shuxue-daishu', title: '代数', children: [
          { id: 'shuxue-daishu-xianxing', title: '线性代数', children: [] },
          { id: 'shuxue-daishu-chouxiang', title: '抽象代数', children: [] },
          { id: 'shuxue-daishu-fangcheng', title: '方程与不等式', children: [] }
        ]},
        { id: 'shuxue-jihe', title: '几何', children: [
          { id: 'shuxue-jihe-ouji', title: '欧氏几何', children: [] },
          { id: 'shuxue-jihe-jiexi', title: '解析几何', children: [] },
          { id: 'shuxue-jihe-topo', title: '拓扑学', children: [] }
        ]},
        { id: 'shuxue-gongshi', title: '公式', children: [
          { id: 'shuxue-gongshi-oula', title: '欧拉公式', children: [] },
          { id: 'shuxue-gongshi-gouding', title: '勾股定理', children: [] }
        ]},
        { id: 'shuxue-weijifen', title: '微积分', children: [
          { id: 'shuxue-weijifen-daoshu', title: '导数', children: [] },
          { id: 'shuxue-weijifen-jifen', title: '积分', children: [] },
          { id: 'shuxue-weijifen-jixian', title: '极限', children: [] }
        ]},
        { id: 'shuxue-gailv', title: '概率统计', children: [
          { id: 'shuxue-gailv-beiyesi', title: '贝叶斯', children: [] },
          { id: 'shuxue-gailv-fenbu', title: '概率分布', children: [] },
          { id: 'shuxue-gailv-tuiduan', title: '统计推断', children: [] }
        ]}
      ]
    },
    {
      id: 'yuyan', title: '语言',
      children: [
        { id: 'yuyan-yuyinxue', title: '语音', children: [
          { id: 'yuyan-yuyinxue-yinwei', title: '音位', children: [] },
          { id: 'yuyan-yuyinxue-shengdiao', title: '声调', children: [] }
        ]},
        { id: 'yuyan-yufa', title: '语法', children: [
          { id: 'yuyan-yufa-jufa', title: '句法', children: [] },
          { id: 'yuyan-yufa-cifa', title: '词法', children: [] }
        ]},
        { id: 'yuyan-wenzi', title: '文字', children: [
          { id: 'yuyan-wenzi-hanzi', title: '汉字演变', children: [] },
          { id: 'yuyan-wenzi-pinyin', title: '拼音文字', children: [] }
        ]},
        { id: 'yuyan-xide', title: '语言习得', children: [
          { id: 'yuyan-xide-eryu', title: '第二语言', children: [] },
          { id: 'yuyan-xide-muyu', title: '母语习得', children: [] }
        ]}
      ]
    },
    {
      id: 'yishu', title: '艺术',
      children: [
        { id: 'yishu-huihua', title: '绘画', children: [
          { id: 'yishu-huihua-youhua', title: '油画', children: [] },
          { id: 'yishu-huihua-shuimo', title: '水墨', children: [] },
          { id: 'yishu-huihua-ban', title: '版画', children: [] }
        ]},
        { id: 'yishu-yinyue', title: '音乐', children: [
          { id: 'yishu-yinyue-gudian', title: '古典', children: [] },
          { id: 'yishu-yinyue-jueshi', title: '爵士', children: [] },
          { id: 'yishu-yinyue-minyao', title: '民谣', children: [] }
        ]},
        { id: 'yishu-jianzhu', title: '建筑', children: [
          { id: 'yishu-jianzhu-gudian', title: '古典建筑', children: [] },
          { id: 'yishu-jianzhu-xiandai', title: '现代主义', children: [] }
        ]},
        { id: 'yishu-dianying', title: '电影', children: [
          { id: 'yishu-dianying-mengtaiqi', title: '蒙太奇', children: [] },
          { id: 'yishu-dianying-jilu', title: '纪录片', children: [] }
        ]}
      ]
    },
    {
      id: 'keji', title: '科技',
      children: [
        { id: 'keji-ai', title: '人工智能', children: [
          { id: 'keji-ai-jiqixuexi', title: '机器学习', children: [] },
          { id: 'keji-ai-damoxing', title: '大模型', children: [] },
          { id: 'keji-ai-shijue', title: '计算机视觉', children: [] }
        ]},
        { id: 'keji-jisuanji', title: '计算机', children: [
          { id: 'keji-jisuanji-suanfa', title: '算法', children: [] },
          { id: 'keji-jisuanji-xitong', title: '操作系统', children: [] }
        ]},
        { id: 'keji-tongxin', title: '通信', children: [
          { id: 'keji-tongxin-5g', title: '5G/6G', children: [] },
          { id: 'keji-tongxin-weixing', title: '卫星通信', children: [] }
        ]},
        { id: 'keji-hangtian', title: '航天', children: [
          { id: 'keji-hangtian-huojian', title: '火箭', children: [] },
          { id: 'keji-hangtian-tance', title: '深空探测', children: [] }
        ]}
      ]
    },
    {
      id: 'shengming', title: '生命',
      children: [
        { id: 'shengming-yichuan', title: '遗传', children: [
          { id: 'shengming-yichuan-jiyin', title: '基因', children: [] },
          { id: 'shengming-yichuan-bianyi', title: '变异', children: [] }
        ]},
        { id: 'shengming-xibao', title: '细胞', children: [
          { id: 'shengming-xibao-liti', title: '线粒体', children: [] },
          { id: 'shengming-xibao-fenhua', title: '细胞分化', children: [] }
        ]},
        { id: 'shengming-jinhua', title: '进化', children: [
          { id: 'shengming-jinhua-ziranxuanze', title: '自然选择', children: [] },
          { id: 'shengming-jinhua-wuzhong', title: '物种形成', children: [] }
        ]},
        { id: 'shengming-shenjing', title: '神经', children: [
          { id: 'shengming-shenjing-tuchu', title: '突触', children: [] },
          { id: 'shengming-shenjing-yishi', title: '意识', children: [] }
        ]}
      ]
    },
    {
      id: 'lishi', title: '历史',
      children: [
        { id: 'lishi-gudai', title: '古代史', children: [
          { id: 'lishi-gudai-wenming', title: '早期文明', children: [] },
          { id: 'lishi-gudai-diguo', title: '帝国兴衰', children: [] }
        ]},
        { id: 'lishi-jindai', title: '近代史', children: [
          { id: 'lishi-jindai-geming', title: '工业革命', children: [] },
          { id: 'lishi-jindai-zhimin', title: '殖民与独立', children: [] }
        ]},
        { id: 'lishi-xiandai', title: '现代史', children: [
          { id: 'lishi-xiandai-lengzhan', title: '冷战', children: [] },
          { id: 'lishi-xiandai-quanqiu', title: '全球化', children: [] }
        ]}
      ]
    }
  ]
};

</script>
<!-- 与手机端 assets/wanwu 同源的离线词典（自动生成，见 .build-tools/gen-demo-dict.js） -->
<script>
/* 自动生成，请勿手改 —— 来源：LocalTaxonomy.kt 的精选词表 + app/src/main/assets/wanwu/part_*.json。
   重新生成：node .build-tools/gen-demo-dict.js */
window.WANWU_DICT = {"风":["空气","气流","吹","帆船","风力发电","台风","凉爽","沙尘","风向","树叶","寒意","季风","风筝","呼啸"],"雨":["云","水滴","伞","湿润","雷","季节","洪水","屋檐","雨声","彩虹","气象","淋湿","农田","雨衣"],"雷":["闪电","乌云","轰鸣","暴雨","惊雷","天空","窗户","心悸","夏夜","避雷针","雷声","颤抖"],"闪电":["电光","霹雳","划破","夜空","耀眼","雷雨","瞬间","枝状","电荷","云层","惊雷","银蛇","天幕","劈开"],"雪":["冰","白色","寒冷","雪花","滑雪","冬天","雪人","融化","屋顶","暴雪","雪崩","纯净","羽绒服","雪景"],"云":["水汽","天空","雨","白色","云朵","漂浮","乌云","云海","气象","形状","阳光","积雨云","晚霞","遮蔽"],"雾":["朦胧","水汽","清晨","能见度","山间","迷茫","白茫茫","车灯","消散","潮湿","仙境","看不清","秋"],"露":["露水","露珠","清晨","草叶","晶莹","凝结","湿润","夜露","晨露","露华","霜降","叶尖","微凉"],"霜":["霜冻","白霜","结霜","秋霜","霜降","寒意","清晨","草尖","冰晶","凝华","霜花","初霜","瓦上"],"冰":["冰块","冰面","冰川","结冰","冰雹","冰雕","滑冰","寒冷","冰层","融化","冰锥","冰河","冰箱","凛冽"],"彩虹":["雨后","七色","天空","短暂","幸运","桥","阳光","水珠","折射","童话","消失","惊喜"],"极光":["北极","绿光","夜空","磁暴","光带","绚丽","极夜","冰岛","太阳风","帷幕","舞动","高纬","挪威","梦幻"],"霞":["晚霞","朝霞","霞光","火烧云","黄昏","天边","映照","云彩","余晖","橘红","漫天","夕阳","晨光"],"霾":["雾霾","灰蒙","污染","能见度","口罩","尘埃","浑浊","静风","颗粒物","刺鼻","预警","天空"],"台风":["风暴","气旋","登陆","狂风","暴雨","预警","沿海","风眼","掀浪","停课","受灾","旋转","树倒"],"龙卷风":["漏斗","旋转","摧毁","强对流","风柱","席卷","雷暴","吸力","路径","美国","房屋","尘柱"],"冰雹":["冰粒","砸落","雷暴","强对流","农作物","颗粒","骤降","冰球","云中","损伤","屋顶","突袭"],"洪水":["泛滥","淹没","决堤","暴雨","救援","水位","洪峰","泥浆","受灾","防汛","溃坝","河堤","冲毁"],"干旱":["缺水","龟裂","烈日","枯黄","旱情","祈雨","土地","作物","水库","干涸","旱灾","焦土"],"地震":["震级","震中","余震","板块","断层","摇晃","废墟","救援","海啸","预警","地裂","震波","倒塌"],"火山":["岩浆","喷发","火山口","火山灰","熔岩","地热","火山岛","浓烟","富士山","爆发","硫磺","炽热"],"海啸":["巨浪","地震","海岸","淹没","预警","波峰","摧毁","深海","撤退","灾难","滔天","席卷"],"季风":["东南亚","湿润","夏季","降雨","风向","气候","印度洋","交替","雨季","海洋","盛行","南亚"],"潮汐":["涨潮","落潮","月亮","引力","潮水","海滩","潮位","赶海","潮间带","涨落","海水","规律"],"寒潮":["降温","冷空气","北风","霜冻","寒流","预警","凛冬","气温骤降","羽绒服","结冰","南下","刺骨"],"热浪":["高温","酷暑","闷热","避暑","空调","烈日","中暑","伏天","汗流","柏油路","冰块","难耐"],"海":["海水","波浪","沙滩","鱼","船","深蓝","潮汐","海鸥","航海","咸","辽阔","岛屿","珊瑚","恐惧"],"湖":["湖水","湖畔","倒影","芦苇","泛舟","淡水","湖心岛","涟漪","天鹅","清澈","湖光","垂钓","静谧"],"河":["流水","河岸","桥梁","鱼","灌溉","洪水","源头","入海","渡船","泥沙","垂柳","干涸","水电站","流域"],"江":["长江","江水","江面","渡轮","江畔","三峡","渔火","江堤","奔涌","东流","渡江","涛声"],"溪":["溪水","山涧","潺潺","卵石","小溪","清冽","溪谷","涉水","涓涓","溪边","石缝","叮咚"],"瀑布":["飞瀑","水帘","落差","轰鸣","水雾","悬崖","黄果树","彩虹","湍急","泻下","白练","深潭"],"泉":["泉水","温泉","喷泉","泉眼","甘甜","涌出","山泉","矿泉","清泉","汩汩","地下水","泉边"],"山":["山峰","岩石","登山","云雾","森林","海拔","雪线","寺庙","险峻","山脚","日出","驴友","溪流","雄伟"],"峰":["顶峰","雪峰","登顶","陡峭","山脊","峰顶","群峰","云海","岩壁","冲顶","险峻","珠峰"],"谷":["山谷","峡谷","谷底","溪流","幽深","回声","峭壁","垭口","谷地","雾气","鸟鸣","窄口"],"崖":["悬崖","崖壁","陡崖","坠落","海岸","攀岩","绝壁","崖顶","深渊","峭立","惊险","山崖"],"洞":["洞穴","山洞","钟乳石","黑暗","蝙蝠","探险","溶洞","石笋","地下河","幽深","岩壁","回声"],"森林":["树木","鸟鸣","苔藓","阳光","迷雾","鹿","蘑菇","落叶","寂静","氧气","伐木","防火","徒步","深处","绿"],"草原":["牧羊","蒙古包","马","辽阔","风吹草低","蓝天","马头琴","牛羊","篝火","迁徙","野花","地平线","自由","牧场"],"沙漠":["沙丘","骆驼","烈日","绿洲","干旱","骆驼刺","风沙","孤独","月牙泉","渴望","海市蜃楼","夜幕","脚印","无水"],"沙滩":["沙子","海浪","贝壳","脚印","椰树","日光浴","海风","沙堡","海岸","浪花","遮阳伞","细软"],"岛":["海岛","孤岛","椰林","渡船","海岸","灯塔","渔村","环岛","礁石","群岛","椰子","远离"],"礁":["珊瑚礁","暗礁","礁石","触礁","海浪","潜水","礁盘","鱼群","危险","浅滩","礁湖","碎浪"],"湾":["海湾","港湾","停泊","避风","弧形","沙滩","码头","滨海","渔船","宁静","海水","湾口"],"平原":["沃野","农田","一望无际","耕地","麦田","村庄","平坦","辽阔","稻浪","大道","地平线","丰饶"],"高原":["海拔","稀薄","牦牛","雪山","青稞","缺氧","屋脊","草甸","紫外线","经幡","高原红","辽阔"],"盆地":["四周环山","低洼","丘陵","河流汇聚","聚气","湿热","成都","沉积","封闭","雾多","沃土","山围"],"丘陵":["起伏","低矮","茶园","梯田","缓坡","连绵","小山","红土","果林","蜿蜒","坡地","海拔低"],"沼泽":["湿地","泥泞","芦苇","陷落","水草","蚊虫","腐殖","沼泽地","水鸟","淤泥","危险","瘴气"],"冰川":["冰舌","雪山","融化","冰川湖","极地","冰裂隙","冰碛","消融","万年","寒气","冰盖","雪线"],"峡谷":["深切","峭壁","急流","一线天","科罗拉多","栈道","两岸","幽深","漂流","岩层","回响","峡口"],"绿洲":["沙漠","水源","椰枣","驼队","旅人","泉眼","希望","绿意","歇脚","沙海","甘泉","生机"],"湿地":["芦苇","候鸟","沼泽","滩涂","生态","水草","丹顶鹤","淤泥","水乡","保护区","鹭鸟","浅水"],"梯田":["层层","稻作","山坡","云南","灌水","曲线","农耕","田埂","龙脊","等高线","水光","山腰"],"溶洞":["钟乳石","石笋","地下河","喀斯特","滴水","幽暗","石花","千姿百态","岩溶","洞厅","潮气","导游"],"星空":["银河","流星","夏夜","望远镜","露营","渺小","许愿","黑暗","天文","光年","牛郎织女","安静","抬头","宇宙"],"银河":["银河系","星云","天河","牛郎织女","旋转","星光","银心","鹊桥","夏夜","横贯","遥远","星带"],"太阳":["恒星","光","热","日出","日落","地球","光合作用","白天","日食","太阳系","能量","夏天","紫外线","崇拜"],"月亮":["月球","夜晚","潮汐","嫦娥","月光","圆缺","阴晴","中秋","陨石坑","地球","银色","诗词","月食","卫星"],"行星":["轨道","公转","火星","木星","土星","行星环","卫星","恒星系","椭圆","引力","八大行星","探索"],"彗星":["彗尾","哈雷","扫帚星","冰核","周期","掠过","长尾","尘埃","回归","稀薄","轨迹","彗发"],"流星":["划落","许愿","夜空","陨石","燃烧","刹那","流星雨","尾迹","飞快","光痕","仰望","短暂"],"宇宙":["星系","恒星","黑洞","大爆炸","时空","黑暗","无限","行星","暗物质","望远镜","起源","浩瀚","生命","膨胀"],"地球":["行星","大陆","海洋","大气","人类","自转","太阳","生态","岩石","赤道","污染","家园","蓝色","卫星"],"火星":["红色","探测器","沙漠","奥林帕斯山","移民","岩石","荒芜","尘暴","运河","好奇号","稀薄","赤红"],"月球":["环形山","月壤","登月","引力","月海","无风","嫦娥","潮汐锁定","寂静","灰白","玉兔号","真空"],"星系":["旋臂","恒星","黑洞","银河","距离","碰撞","仙女座","庞大","星团","盘状","演化","光年"],"黑洞":["引力","事件视界","吞噬","光速","奇点","深空","人马座","不可见","吸积盘","扭曲","致密","逃逸"],"星云":["尘埃","气体","绚丽","猎户座","恒星摇篮","弥漫","色彩","蟹状","发光","凝聚","遥远","云气"],"恒星":["核聚变","发光","寿命","超新星","主序","光谱","氢氦","燃烧","耀眼","演化","炽热","太阳"],"卫星":["轨道","火箭","发射","通信","导航","遥感","北斗","地面站","太阳能板","信号","太空","星链"],"陨石":["坠落","坑洞","天外","铁镍","撞击","收藏","火流星","碎片","陨石坑","稀有","熔壳","巨响"],"日食":["遮挡","黑影","天象","月影","奇观","观测","日冕","全食","光环","短暂","仰望","太阳"],"星座":["猎户座","北斗","连点成线","占星","黄道","夜空","十二宫","神话","辨认","星图","闪烁","传说"],"太空":["失重","宇航员","真空","空间站","火箭","深空","舱外","漂浮","辐射","浩瀚","使命","星海"],"猫":["动物","猫科","宠物","喵叫","胡须","爪子","老鼠","鱼","毛","慵懒","铲屎官","流浪","猫砂","可爱"],"狗":["犬","宠物","忠诚","吠叫","骨头","遛狗","导盲犬","狼","尾巴","看家","狂犬病","摇尾","陪伴","哈士奇"],"鸟":["羽毛","翅膀","飞翔","鸟巢","鸣叫","迁徙","蛋","天空","麻雀","鹰","树枝","歌唱","昆虫","候鸟"],"鱼":["水","鳞片","鳍","游泳","河流","海洋","钓鱼","鱼缸","鱼饵","产卵","鲤鱼","美味","鳃","群游"],"虫":["昆虫","触角","爬行","甲虫","蚱蜢","蠕动","草丛","幼虫","鸣叫","微小","蜕皮","田野"],"蝴蝶":["翅膀","花丛","毛毛虫","翩飞","斑斓","蜕变","凤蝶","采蜜","轻盈","春色","蛹","花纹"],"蜜蜂":["蜂蜜","蜂巢","花粉","工蜂","蜇人","蜂王","酿蜜","花蜜","嗡嗡","采蜜","六角","勤奋"],"蛇":["鳞片","吐信","缠绕","毒牙","蜿蜒","冷血","冬眠","蛇皮","草丛","蜕皮","无声","爬行"],"虎":["斑纹","百兽之王","森林","咆哮","利爪","威猛","华南虎","猛虎","虎啸","独行","捕猎","王字"],"狮":["鬃毛","草原","吼声","群居","非洲","捕猎","母狮","狮群","威风","獠牙","领地","狩猎"],"大象":["象鼻","象牙","大耳朵","象群","非洲象","笨重","长牙","温和","庞然","象腿","戏水","记忆"],"熊猫":["竹子","黑白","国宝","四川","憨态","熊猫基地","圆滚","竹林","幼崽","珍稀","慢吞","招牌"],"鹿":["鹿角","梅花鹿","森林","灵动","驯鹿","鹿群","呦呦","警觉","跳跃","斑纹","小鹿","林间"],"马":["骏马","马蹄","草原","奔驰","马鞍","鬃毛","骑马","缰绳","嘶鸣","马蹄声","烈马","牧场"],"牛":["奶牛","牛角","耕田","反刍","牧牛","牛皮","哞","水牛","牛犊","犁地","憨厚","牧场"],"羊":["绵羊","羊毛","山羊","牧羊","羊群","咩","羊奶","山坡","温顺","剪毛","羊羔","草原"],"猪":["猪圈","粉红","肥","野猪","猪崽","拱土","贪睡","猪肉","哼哼","圆身","农场","泥坑"],"鸡":["公鸡","打鸣","母鸡","鸡蛋","鸡窝","羽毛","啄食","鸡雏","报晓","翅膀","农场","清晨"],"兔":["长耳","蹦跳","胡萝卜","兔毛","穴居","月兔","白兔","机警","三瓣嘴","跳跃","兔窝","柔软"],"鲸":["喷水","深海","巨大","鲸歌","蓝鲸","搁浅","鲸须","洄游","尾鳍","庞然","浮出","海洋"],"海豚":["聪明","跃出","声呐","群游","友好","海洋馆","吻部","背鳍","救助","嬉戏","灰蓝","跃身"],"鲨鱼":["利齿","背鳍","凶猛","深海","食肉","大白鲨","撕咬","游弋","威慑","银灰","猎食","鳍"],"龟":["龟壳","缓慢","长寿","海龟","爬行","产卵","缩头","龟甲","沙滩","冬眠","沉稳","绿海龟"],"蛙":["青蛙","跳跃","池塘","呱呱","蝌蚪","荷叶","鸣叫","雨夜","鼓膜","捕虫","水边","翠绿"],"蝉":["蝉鸣","夏天","蜕壳","树枝","聒噪","幼虫","知了","高枝","午后","薄翼","地下","喧嚣"],"蚂蚁":["蚁群","搬运","蚁巢","勤劳","工蚁","队列","触角","蚁后","分工","细小","面包屑","搬食"],"蜘蛛":["蛛网","吐丝","捕虫","八足","角落","结网","丝线","毒液","悬垂","耐心","屋檐","猎手"],"鹰":["雄鹰","盘旋","俯冲","利爪","高山","锐眼","翱翔","猎食","鹰巢","啸叫","峭壁","天空"],"孔雀":["开屏","尾羽","华丽","云南","炫耀","翠绿","翎眼","求偶","园中","蓝绿","踱步","艳丽"],"鹤":["丹顶鹤","长腿","湿地","优雅","仙鹤","迁徙","白羽","鸣叫","芦苇","起舞","长寿","涉水"],"企鹅":["南极","黑白","摇摆","冰面","群居","潜水","企鹅岛","绒羽","寒冷","憨萌","孵蛋","海洋"],"狼":["狼群","嚎叫","荒野","野性","捕猎","雪原","孤狼","敏锐","獠牙","夜行","头狼","追踪"],"树":["树根","树干","树枝","树叶","森林","年轮","鸟","树荫","木材","春天","落叶","氧气","种子","松鼠"],"花":["花瓣","花蕊","香气","春天","园丁","玫瑰","绽放","蝴蝶","花瓶","花语","凋谢","色彩","授粉","浪漫"],"草":["绿色","草原","牛羊","根","野火","春天","露水","草坪","药材","割草","荒原","柔软","蚂蚱","生长"],"竹":["竹子","竹笋","竹节","竹林","熊猫","竹叶","挺拔","空竹","竹筏","青翠","成片","竹影"],"松":["松树","松针","松果","常青","松涛","松鼠","黄山松","树脂","针叶","耐寒","苍劲","松林"],"柳":["柳树","柳枝","垂柳","柳絮","河岸","柔条","柳荫","拂水","春风","嫩绿","摇曳","折柳"],"荷":["荷花","荷叶","莲藕","池塘","莲蓬","蜻蜓","清香","并蒂","藕节","夏日","亭亭","淤泥"],"梅":["梅花","寒冬","暗香","傲雪","梅枝","赏梅","腊梅","红梅","花苞","清高","雪中","凌寒"],"兰":["兰花","幽香","清雅","兰草","君子","盆栽","山间","素淡","细叶","芬芳","静","品种"],"菊":["菊花","秋日","重阳","赏菊","花瓣","黄菊","傲霜","花展","盆栽","淡香","层叠","深秋"],"玫瑰":["爱情","花瓣","刺","红","花店","情人节","香气","枯萎","求婚","荆棘","浪漫","一束","园艺"],"向日葵":["葵花","向阳","瓜子","金黄","花盘","田野","追逐阳光","高秆","灿烂","蜜蜂","大片","花田"],"蒲公英":["绒球","飞舞","种子","野地","吹散","黄花","伞絮","随风","田野","飘散","轻盈","童年"],"苔藓":["青苔","潮湿","石阶","阴凉","绒绿","古墙","林下","孢子","贴地","湿气","石缝","绿毯"],"蘑菇":["菌菇","伞盖","雨后","采摘","毒蘑菇","菌丝","腐木","松茸","鲜嫩","林间","菌类","潮湿"],"仙人掌":["沙漠","尖刺","耐旱","肉质","开花","盆栽","储水","墨西哥","干旱","灰绿","孤挺","刺球"],"种子":["播种","发芽","胚芽","土壤","萌发","颗粒","繁衍","破土","休眠","果实","希望","育苗"],"根":["根系","扎根","须根","土壤","吸水","块根","深扎","根须","地下","支撑","根茎","盘结"],"叶":["叶片","叶脉","绿叶","落叶","光合","叶绿素","枝头","枯叶","嫩叶","飘落","掌状","树荫"],"果":["果实","果子","成熟","采摘","果肉","果核","丰收","香甜","果汁","枝头","饱满","硕果"],"藤":["藤蔓","缠绕","攀爬","葡萄藤","藤椅","蔓延","卷须","架子","绿廊","柔韧","紫藤","垂挂"],"芦苇":["芦苇荡","湿地","摇曳","芦花","水边","苇秆","水鸟","秋风","丛生","沙沙","编席","浅滩"],"稻":["稻田","稻穗","插秧","稻米","金黄","水田","收割","稻香","农人","谷粒","弯腰","秋收"],"麦":["麦田","麦穗","小麦","面粉","金黄","收割","麦浪","麦芒","磨面","秋播","饱满","农忙"],"茶":["茶叶","热水","茶壶","苦涩","回甘","茶馆","龙井","泡茶","咖啡因","待客","禅","杯子","清香","提神"],"珊瑚":["珊瑚礁","礁石","彩色","海底","鱼群","白化","触手","热带","斑斓","造礁","潜水","共生"],"银杏":["扇形叶","金黄","秋天","古树","白果","落叶","街道","银杏树","千年","铺地","扇叶","秋色"],"蕨":["蕨类","羽叶","阴湿","林下","孢子","卷叶","绿意","山野","古老","丛生","蕨菜","荫凉"],"泥土":["土壤","湿润","芬芳","蚯蚓","田埂","黑土","耕作","泥巴","大地","朴实","播种","松软"],"岩石":["石头","岩层","坚硬","花岗岩","风化","山体","岩壁","碎石","纹理","沉重","矿脉","沉积"],"沙":["沙粒","细沙","沙丘","脚印","沙滩","流沙","沙漏","扬沙","温暖","颗粒","沙尘","细软"],"日出":["朝阳","黎明","地平线","霞光","破晓","升起","金光","早起","东方","晨雾","希望","天空"],"日落":["夕阳","黄昏","余晖","晚霞","归鸟","暮色","西山","橙红","傍晚","收工","剪影","沉落"],"月光":["银白","洒落","夜色","清冷","月影","皎洁","窗台","柔光","寂静","树影","月色","朦胧"],"星光":["闪烁","遥远","夜空","微光","璀璨","点点","银河","仰望","寒冷","星辉","指明","微弱"],"春":["立春","花开","回暖","嫩芽","燕子","踏青","细雨","复苏","柳绿","风筝","播种","生机"],"夏":["盛夏","蝉鸣","烈日","暴雨","西瓜","避暑","暑假","荷花","闷热","蒲扇","雷阵雨","绿荫"],"秋":["落叶","丰收","凉爽","金黄","大雁","秋风","枫叶","稻谷","中秋","萧瑟","桂香","白露"],"冬":["寒冬","冰雪","腊月","围炉","羽绒","萧瑟","北风","春节","枯枝","暖炉","雪夜","冻土"],"黎明":["破晓","黑夜","第一缕光","鸡鸣","露水","寒意","希望","早起","天际","启程","新生","潮汐","安静"],"黄昏":["夕阳","晚霞","归家","倦鸟","橘色","下班","海边","影子","炊烟","一天","温柔","落幕","街灯"],"天空":["蓝天","云朵","辽阔","飞鸟","苍穹","仰望","晴空","深远","蔚蓝","云霞","无边","高处"],"家":["饭桌","灯光","钥匙","沙发","争吵","门","母亲","归属","拖鞋","阳台","温度","团圆","过年","安定","港湾"],"房子":["居住","砖","屋顶","窗户","门","客厅","卧室","房贷","装修","家","邻居","地基","户型","温暖"],"客厅":["沙发","茶几","电视","地毯","客人","聊天","遥控器","瓜子","灯","全家","喝茶","摆件","拖鞋","窗"],"卧室":["床","被子","枕头","衣柜","窗帘","睡觉","台灯","拖鞋","打呼","关灯","床头柜","蚊帐","私密","梦"],"书房":["书架","台灯","书桌","电脑","安静","看书","椅子","纸笔","眼镜","学习","墨香","抽屉","专注","借书"],"卫生间":["马桶","淋浴","洗漱","镜子","毛巾","牙刷","地漏","排气扇","瓷砖","洗手池","洗澡","湿滑","香皂","憋"],"阳台":["晾衣服","花盆","阳光","晒被子","洗衣机","护栏","通风","绿萝","拖把","雨水","栏杆","风吹","看楼下","衣架"],"门":["门锁","把手","门铃","钥匙","开合","门缝","敲","玄关","木门","铁门","推拉","门口","猫眼","关紧"],"窗":["玻璃","阳光","雨滴","窗帘","远方","夜景","通风","阳台","框架","眺望","冬天","雾气","灯"],"墙":["白灰","挂画","瓷砖","墙纸","墙角","承重","粉刷","裂缝","隔断","钉子","阴凉","掉皮","靠","墙根"],"屋顶":["防水","瓦片","排水","檐口","隔热","天台","坡屋面","太阳能","检修","漏水","天窗","檐沟"],"地板":["木地板","瓷砖","拖地","光脚","打蜡","缝隙","灰尘","凉","地毯","踩","起鼓","防滑","扫地","踢脚线"],"楼梯":["台阶","扶手","上楼","转角","电梯","木头","栏杆","层高","喘气","搬家","楼梯间","防滑条","往下走","回声"],"电梯":["轿厢","井道","缆绳","按钮","对重","曳引","维保","困人","楼层","感应","噪音","应急"],"床":["被子","枕头","床垫","睡觉","翻身","床头","打鼾","赖床","铺床","软硬","床单","梦","醒来","单人"],"被子":["棉花","羽绒","保暖","晒","叠","冬天","被套","厚","踢被子","盖","暖和","潮湿","洗被单","翻身"],"枕头":["荞麦","羽绒","高度","落枕","靠","枕头套","软","抱","颈椎","侧卧","洗","香味","头","压"],"床单":["铺","换洗","纯棉","褶皱","花色","尺寸","床垫","洗衣机","晒","起球","螨虫","凉席","被套","干净"],"沙发":["坐下","靠背","抱枕","客厅","布艺","真皮","弹簧","躺着","看电视","扶手","塌陷","沙发床","毯子","打盹"],"茶几":["客厅","水杯","遥控器","水果盘","玻璃","沙发","磕碰","擦灰","杂志","泡茶","零食","矮","烟灰缸","挪"],"桌子":["桌面","椅子","抽屉","吃饭","写字","木纹","桌腿","圆桌","方桌","磕角","擦","台灯","挪动","桌布"],"椅子":["木头","靠背","四条腿","坐下","办公","餐椅","摇椅","教室","空着","扶手","吱呀","排队"],"衣柜":["挂衣","抽屉","折叠","换季","樟脑","衣架","门板","隔层","木香","找衣服","塞满","除湿","推拉门","整理"],"抽屉":["拉开","滑轨","收纳","袜子","杂物","卡住","锁","一层","翻找","表格","整理","吱","藏","抽屉柜"],"书架":["书","隔板","落灰","分类","书房","靠墙","倾倒","摆件","借书","摞","书脊","打扫","装满","梯子"],"镜子":["玻璃","反射","倒影","梳妆","自拍","裂纹","照见","水银","化妆","审视","真实","墙上","幻觉"],"灯":["灯泡","夜晚","照亮","台灯","路灯","开关","影子","温暖","加班","电","霓虹","守候","熄灭"],"台灯":["书桌","灯罩","护眼","开关","学习","灯泡","夜里","阅读","可调","底座","床头","刺眼","插电","光"],"地毯":["踩","客厅","卧室","毛绒","吸尘","清洗","卷边","花纹","保暖","打翻","边缘","脚感","铺","脏"],"窗帘":["遮光","拉开","轨道","布","清洗","挂钩","透光","灰尘","双层","花色","拉绳","遮阳","飘","窗"],"钥匙":["锁","门","金属","口袋","回家","丢失","配钥匙","车钥匙","密码","归属","叮当","锈"],"门锁":["钥匙","反锁","指纹","密码","锁芯","卡住","撬","换锁","安全","门把","电池","咔哒","忘锁","防盗"],"拖把":["拖地","水桶","拧干","湿","地板","布条","甩","晾","弯腰","油污","干净","海绵","拖把池","腰疼"],"扫帚":["扫地","灰尘","簸箕","墙角","头发","竹","靠门","清扫","弯腰","阳台","沙子","掸","帚把","扬尘"],"垃圾桶":["垃圾袋","扔","满","厨房","异味","套袋","分类","桶盖","苍蝇","塑料","倒","踩踏","角落","收垃圾"],"马桶":["冲水","坐","水箱","盖子","异味","堵","疏通","刷","卫生间","纸巾","漏水","按钮","蹲","冬冷"],"浴缸":["泡澡","热水","放水","洗澡","浴帘","滑","排水","泡泡","溢水","陶瓷","擦洗","沐浴露","累","舒服"],"淋浴":["花洒","热水","洗澡","水压","搓","沐浴露","湿","地漏","起雾","唱歌","快速","停水","水温","排水"],"毛巾":["擦脸","干","挂","湿","毛圈","洗脸","浴巾","换","柔软","发霉","晾","香味","擦手","搓"],"牙刷":["刷牙","牙膏","毛刷","换","口腔","电动","杯","早晚","牙龈","刷头","干净","浴室","三分钟","磨损"],"牙膏":["挤","薄荷","泡沫","刷牙","盖子","用完","美白","含氟","牙","味道","压","管尾","洗漱","凉"],"洗发水":["洗头","泡沫","香味","头皮","按压","冲","护发素","去屑","空瓶","浴室","痒","油腻","挤","顺滑"],"香皂":["洗手","泡沫","滑","香味","肥皂盒","搓","碱","消毒","洗澡","一小块","融化","起泡","干净","掉"],"洗衣机":["滚筒","洗衣液","甩干","脱水","晾衣","漂洗","噪音","定时","水位","滴滴","维修","缠绕","洗衣房","洗衣服"],"冰箱":["冷藏","冷冻","保鲜","制冰","嗡嗡","保鲜盒","剩菜","结霜","断电","门封","牛奶","贴纸","冻住","食物"],"空调":["制冷","制热","遥控器","外机","滤网","温度","电费","吹风","除湿","漏水","噪音","感冒","凉快","维修"],"电视":["屏幕","遥控器","频道","节目","新闻","广告","客厅","春晚","直播","信号","家庭","娱乐","沙发","收视率"],"遥控器":["电视","按键","电池","换台","音量","找不到","沙发缝","红外","开空调","塑料","失灵","摁","摆放","没电"],"风扇":["扇叶","吹风","摇头","档位","嗡嗡","夏天","凉快","罩子","拆洗","噪音","插电","风力","落地扇","转"],"插座":["插头","电线","通电","排插","松动","短路","墙","充电","烧焦","安全","拔","开关","儿童","铜片"],"开关":["按","关灯","面板","双控","坏","咔哒","墙","触电","摸黑","开灯","电线","位置","灵敏","塑料"],"电线":["插头","走线","老化","短路","缠","插座","电工","铜芯","绝缘","打结","绊倒","断路","布线","掉皮"],"衣架":["晾衣","挂","塑料","铁丝","衣柜","变形","鼓包","一排","阳台","收","钩","滑落","木衣架","折叠"],"鞋柜":["门口","拖鞋","换鞋","层板","异味","塞满","玄关","擦","除味","抽屉","关不上","木头","鞋油","雨鞋"],"闹钟":["响","起床","定","铃声","贪睡","指针","电池","半夜","关掉","床头","迟到","秒针","吵","催"],"厨房":["灶台","油烟","菜刀","锅铲","洗碗","蒜","烟火气","菜谱","冰箱","热气","母亲","味道","忙碌"],"锅":["炒锅","锅盖","锅底","煮","烧","生锈","不粘","端","糊","洗锅","灶","铁锅","汤","把手"],"碗":["吃饭","盛饭","瓷","磕","洗碗","一摞","碗柜","破口","汤碗","端","烫","筷子","洗洁精","团圆"],"筷":["夹菜","一双","竹","洗完","摆","掉","左右手","筷子筒","一次性","火锅","搅拌","吃饭","直","筷子"],"勺":["舀汤","汤匙","搅拌","舀","盐","一勺","柄","咖啡","木勺","量","叮当","嘴","倒","铁勺"],"刀":["切菜","锋利","刀刃","菜刀","磨","砧板","割手","刀柄","水果刀","剁","收好","生锈","快","危险"],"砧板":["切菜","木","案板","洗","生熟","发霉","剁肉","菜刀","台面","竖","划痕","撒盐","水渍","厨房"],"菜刀":["锋利","剁肉","刀背","磨刀","刀柄","拍蒜","切菜","生锈","钝","铁","厨房","危险","砍","收"],"炒锅":["翻炒","冒烟","油","锅铲","火","不粘锅","锅底","颠勺","糊","铁锅","洗","香气","盖","灶"],"汤锅":["炖","煲汤","咕嘟","盖","火候","排骨","撇沫","端","烫","不锈钢","慢炖","一锅","粥","灶"],"电饭煲":["煮饭","内胆","按键","保温","米","跳闸","蒸汽","揭盖","洗米","预约","粘锅","电源","香气","剩饭"],"微波炉":["加热","转盘","叮","剩饭","解冻","热气","塑料盒","定时","烤","辐射","转","厨房","烫","炉门"],"烤箱":["烘焙","蛋糕","温度","预热","烤盘","焦","面包","手套","定时","香味","冒烟","锡纸","饼干","烘烤"],"水壶":["烧水","咕嘟","开水","壶嘴","倒","热气","电水壶","盖","水垢","烫","提","茶","壶身","保温"],"杯子":["喝水","玻璃","陶瓷","杯口","茶","洗","磕","把手","保温杯","倒水","干净","桌上","塑料","一口"],"茶壶":["泡茶","壶嘴","茶叶","紫砂","倒","盖","热气","茶垢","提","一壶","功夫茶","烫","香","壶身"],"盘子":["装菜","瓷","端","洗","圆","一摞","破","鱼","花边","油","桌上","碟","磕","摆盘"],"调料":["盐","酱油","醋","花椒","瓶瓶罐罐","调味","架子","一勺","撒","辣","香","放多","味道","味精"],"盐":["咸","撒","一勺","白","颗粒","腌","碘","罐","汤","调","少放","溶","盐罐","海水"],"糖":["甜","白砂糖","方糖","溶","一勺","蛋糕","咖啡","蛀牙","罐","红糖","结晶","撒","热量","甜味"],"酱油":["生抽","老抽","咸","倒","瓶","拌饭","上色","蘸","炒菜","滴","黑","黄豆","提鲜","洒"],"醋":["酸","陈醋","饺子","蘸","瓶","凉拌","一勺","刺鼻","白醋","去腥","米醋","开胃","倒","味道"],"油":["倒","热锅","花生油","冒烟","煎","炸","桶","溅","腻","橄榄油","一滴","灶","油烟","滑"],"辣椒":["辣","红","切","呛","花椒","籽","干辣椒","油泼","出汗","剁椒","手辣","小米椒","香","上火"],"葱":["小葱","切碎","姜","蒜","爆香","葱花","绿","一根","蘸酱","面条","洗","葱白","调味","辛辣"],"姜":["老姜","切片","去腥","蒜","葱","姜汤","辣","驱寒","姜丝","拍","金黄","红糖","炒","姜皮"],"蒜":["大蒜","拍","蒜末","呛","剥","瓣","杀菌","炒菜","蒜蓉","发芽","口臭","紫皮","捣","味"],"围裙":["做饭","系","挡油","挂","布","口袋","脏","洗","厨房","带子","防溅","摘","腰","花布"],"洗洁精":["洗碗","泡沫","去油","按压","冲","瓶子","手干","一滴","海绵","滑","味道","厨房","稀释","用完"],"保鲜膜":["裹","剩菜","冰箱","撕","粘","一卷","西瓜","透明","贴","保鲜","断口","盒子","厨房","打结"],"保鲜盒":["装","冰箱","剩饭","盖","密封","微波","塑料","洗","一摞","便当","水果","扣","冷藏","叠"],"油烟机":["抽风","油烟","灶台","滤网","噪音","清洗","油盒","按钮","厨房","吸","挂","滴油","强档","擦"],"燃气灶":["点火","火苗","旋钮","锅","煤气","蓝火","汤溢","关火","灶眼","打不着","擦","危险","厨房","爆炒"],"米饭":["稻米","电饭煲","香气","碗","筷子","主食","煮","粒粒","配菜","南方","剩饭","白","饱"],"面条":["面粉","拉面","汤","筷子","热气","牛肉面","山西","和面","筋道","早餐","碗","吸溜","浇头"],"馒头":["蒸","白面","发酵","掰","暄软","早餐","咸菜","一屉","凉","面团","酵母","主食","啃","蒸锅"],"包子":["蒸","肉馅","褶","咬","汤汁","早餐","一屉","面皮","热","蘸醋","菜包","灌汤","捏","饱"],"饺子":["面皮","馅","包","春节","醋","沸腾","褶","团圆","擀","韭菜","冷冻","一碗","蒜"],"粥":["熬","米","稀","白粥","咸菜","养胃","烫","一碗","早起","皮蛋","慢火","稠","勺子","清淡"],"汤":["煲","喝","热","排骨","一碗","鲜","撇油","砂锅","饭前","咸","汤勺","烫嘴","冬瓜","营养"],"蛋":["鸡蛋","打","煎","煮","蛋黄","蛋白","炒","蛋壳","番茄","蒸蛋","溏心","冰箱","营养","破"],"豆腐":["嫩","老豆腐","切块","煎","麻婆","豆香","易碎","汤","石膏","白","焯","凉拌","滑","葱"],"白菜":["叶子","菜帮","炒","炖","冬储","便宜","醋溜","一层层","洗","粉条","猪肉","脆","菜心","水分"],"土豆":["削皮","切丝","发芽","淀粉","炖","薯条","土豆泥","圆","芽眼","炒","面","储存","块","便宜"],"番茄":["红","炒蛋","汁","沙瓤","切块","酸甜","汤","生吃","番茄酱","沙拉","蒂","汁水","熟","皮"],"黄瓜":["脆","拍","凉拌","蒜","刺","生吃","切片","绿","敷脸","醋","清口","藤","籽","水分"],"茄子":["紫","红烧","吸油","切条","蒸","蒜泥","软","皮","炒","长","圆","咸鱼","烂","蒂"],"萝卜":["白萝卜","炖","汤","脆","辣","擦丝","晾","腌","皮","冬天","排骨","缨子","水","甜"],"香菇":["干香菇","泡","鲜","香味","伞","蒂","炖鸡","切片","菌","焯","汤","炒","褐色","滑"],"青菜":["炒","绿","叶子","洗","蒜","油","脆","一盘","新鲜","焯","菜梗","家常","水","便宜"],"玉米":["煮","甜","粒","棒","玉米须","烤","淀粉","玉米油","糯","剥","一排排","金黄","汤","须"],"牛肉":["炖","牛腩","切","嚼","红烧","牛排","筋","贵","汤","卤","嫩","涮","火锅","蛋白质"],"猪肉":["五花","红烧","炒","肥","瘦肉","排骨","剁","馅","便宜","腌","炖","猪皮","油","肉末"],"鸡肉":["炖","鸡腿","白切","鸡汤","柴","嫩","鸡翅","鸡皮","炒","香菇","烤","剁","骨头","红烧"],"羊肉":["涮","膻","冬天","汤","串","炖","火锅","羊排","白萝卜","嫩","烤","进补","手抓","汤锅"],"虾":["剥","鲜","虾仁","白灼","虾壳","虾线","炒","弹","海鲜","虾尾","冻","姜","变红","弯"],"螃蟹":["蒸","蟹黄","蟹钳","剥","姜醋","秋天","横着走","壳","鲜","性寒","蟹腿","绑","阳澄湖","香"],"香肠":["切","蒸","咸","腊","灌","一片","炒饭","下酒","红","晾","肠衣","熏","肥","咬"],"牛奶":["奶牛","牧场","白色","钙","早餐","玻璃杯","冷藏","酸奶","奶瓶","煮沸","蛋白","香浓","保质期"],"酸奶":["喝","稠","冷藏","勺子","水果","益生菌","酸","一杯","保质期","舔盖","吸管","草莓","早餐","冰"],"面包":["面粉","烘焙","早餐","酵母","烤箱","黄油","饥饿","麦子","面包店","香甜","三明治","松软","保质期","厨师"],"蛋糕":["生日","奶油","蜡烛","甜","切","一块","烘焙","发胖","草莓","许愿","蛋糕店","巧克力","松软","叉子"],"饼干":["咬","脆","包装","碎","甜","零食","泡牛奶","一包","烤","黄油","掉渣","圆","咸","铁盒"],"巧克力":["甜","苦","融化","一块","可可","礼物","情人节","发胖","包装","掰","夹心","黑巧","牙","丝滑"],"冰淇淋":["冷","化","甜","夏天","舔","甜筒","冰箱","口味","滴","冰柜","雪糕","一口","奶香","凉"],"咖啡":["饮品","咖啡豆","咖啡因","苦涩","提神","咖啡馆","拿铁","手冲","烘焙","产地","杯子","星巴克","熬夜","香气"],"果汁":["榨","橙","甜","一杯","果肉","酸","冷藏","吸管","鲜榨","苹果","冰","维生素","稠","喝"],"汽水":["气泡","冰","甜","一瓶","打嗝","可乐","拉环","冒泡","夏天","塑料瓶","凉","糖","摇晃","冲"],"啤酒":["冰","泡沫","一瓶","烧烤","苦","拉环","下酒","醉","肚子","小麦","开瓶器","杯子","撸串","敬"],"白酒":["辣","高度","一杯","敬","醉","酒桌","茅台","倒","香","上头","粮食","瓶","干杯","劝酒"],"蜂蜜":["甜","冲水","黏","一勺","蜜蜂","罐","结晶","柠檬","润喉","早餐","滴","金黄","花","保质"],"花生":["剥","炒","脆","下酒","红衣","油炸","盐水","花生米","油","壳","香","磨牙","霉","颗粒"],"瓜子":["嗑","葵花","咸","看电视","壳","一把","炒","香","拜年","停不下","舌头疼","袋","五香","干"],"苹果":["果实","植物","土地","果壳","种子","干果","水果","梨子","阳光","氧气","叶子","细胞"],"西瓜":["夏天","瓜瓤","黑籽","冰镇","汁水","切开","沙甜","藤蔓","田里","半个","勺子","消暑","红"],"草莓":["红色","果肉","春天","采摘","奶油","酸甜","大棚","籽","蛋糕","果酱","娇嫩","篮子","香气"],"衬衫":["领子","扣子","袖口","熨","白","上班","扎进","棉","洗","皱","条纹","纽扣","商务","穿"],"外套":["穿","拉链","口袋","挡风","换季","厚","挂","脱","领子","牛仔","冲锋衣","冷","袖子","扣"],"裤子":["裤腿","腰带","口袋","裤脚","长裤","牛仔","松紧","洗","破洞","裤裆","卷起","褶","合身","穿"],"裙子":["裙摆","转圈","夏天","腰","长裙","短裙","连衣裙","风","飘","拉链","纱","穿","好看","丝"],"鞋":["穿","鞋底","鞋带","走路","磨脚","擦","一双","鞋盒","鞋柜","新","脏","运动","高跟","皮鞋"],"帽子":["戴","遮阳","帽檐","冬天","毛线","棒球帽","摘","防晒","款式","针织","风刮跑","秃头","帽架","头"],"围巾":["围","保暖","毛线","冬天","脖子","系","绕","花色","羊绒","风","脱","织","长","暖"],"手套":["戴","手","冬天","保暖","五指","摘","毛线","骑车","触屏","掉","一对","皮革","冻","掌心"],"袜子":["穿","洗","一双","脚","臭","破洞","棉","短袜","丝袜","配","湿","掉","抽屉","脚趾"],"眼镜":["戴","镜片","镜框","近视","擦","起雾","鼻托","摘","摔","度数","配镜","太阳镜","划痕","耳朵"],"手表":["戴","表盘","指针","秒针","电池","时间","手腕","机械表","表带","走慢","摘","看时间","防水","品牌"],"背包":["背","肩带","拉链","装","出门","上学","口袋","重","放下","双肩","书包","隔层","提","磨肩"],"钱包":["装钱","银行卡","拉链","丢","皮","口袋","零钱","身份证","付款","掏","变薄","缝","破","鼓"],"皮带":["系","扣","裤腰","皮","孔","松","紧","腰带","断","金属扣","黑","勒","磨","裤"],"T恤":["圆领","短袖","棉","白","印花","穿","洗","变形","汗","夏天","宽松","领口","便宜","打底"],"毛衣":["毛线","织","起球","扎人","冬天","领口","厚","穿","洗","缩水","羊绒","暖","袖子","勾丝"],"羽绒服":["羽绒","鼓","保暖","冬天","洗","钻毛","拉链","轻","帽子","蓬松","贵","穿","干洗","冻"],"牛仔裤":["牛仔","蓝","口袋","拉链","磨白","硬","洗","破洞","裤脚","皮带","穿","褪色","铆钉","版型"],"睡衣":["睡","棉","宽松","换","晚上","图案","洗","舒适","起夜","一套","家居服","软","叠","穿"],"拖鞋":["穿","踩","浴室","塑料","脚","啪嗒","一双","滑","门口","换","棉拖","破","湿","地上"],"运动鞋":["跑步","鞋带","鞋底","缓震","透气","白","运动","磨","一双","洗","汗","打球","网面","新"],"纽扣":["扣上","掉了","缝","线","扣眼","一颗","大衣","解开","找","衬衣","塑料","圆","针","对齐"],"拉链":["拉上","卡住","齿","拉头","外套","坏","缝","顺滑","裤子","背包","拉开","换","咬布","金属"],"口袋":["装","手插","掏","手机","钥匙","缝","破","裤兜","丢","鼓","内袋","拉链","摸","空"],"熨斗":["熨衣服","蒸汽","烫","平整","插电","温度","衬衫","褶皱","底板","烫坏","加水","收","热","电熨斗"],"洗衣粉":["洗衣服","泡沫","一勺","去污","香","袋子","手洗","泡","洗衣机","结块","用完","白","溶解","伤手"],"丝绸":["滑","光","凉","旗袍","蚕","贵","洗","皱","丝巾","手感","透气","绸缎","亮","薄"],"公司":["老板","员工","写字楼","营业执照","股东","部门","会议室","工资","招聘","加班","前台","工位","团建","人事","业绩","辞职"],"工厂":["车间","流水线","工人","机器","仓库","厂长","安全生产","原料","订单","班次","厂区","设备","质检","工会","宿舍","烟囱"],"企业":["经营","利润","品牌","集团","子公司","董事长","战略","注册资本","纳税","上市","高管","企业文化","年报","并购","供应链","法人"],"银行":["存钱","贷款","利息","柜员","ATM","信用卡","金融","网点","储户","利率","转账","理财","征信","金库"],"医院":["医生","护士","病房","挂号","手术","药","急诊","病人","检查","消毒","救护车","痊愈","走廊","医保"],"学校":["老师","学生","教室","课本","操场","考试","校长","校服","铃声","班级","作业","校园","升学","童年"],"法院":["法官","庭审","判决","律师","原告","被告","法庭","起诉","调解","传票","卷宗","书记员","上诉","执行","旁听席","法槌"],"警局":["警察","报警","笔录","拘留","巡逻","派出所","警车","手铐","审讯","报案","值班","警徽","出警","档案","嫌疑人","监控"],"政府":["政策","官员","公共服务","税收","听证","治理","部门","法律","城市","民生","权力","预算","行政","国家"],"军队":["士兵","军营","训练","军装","武器","将军","阅兵","哨所","队列","军衔","演习","军令","后勤","边防","战备","军旗"],"媒体":["记者","采访","报道","编辑","电视台","新闻","头条","直播","稿件","版面","舆论","发布会","摄像","选题","订阅","评论"],"报社":["排版","印刷","记者","主编","稿件","发行","铅字","校对","专栏","日报","订阅","新闻纸","编辑部","截稿","通讯员","广告"],"电视台":["演播厅","主持人","摄像机","导播","信号","栏目","直播","收视率","编导","灯光","提词器","录制","频道","广告","台标","转播"],"超市":["货架","购物车","收银台","打折","零食","生鲜","塑料袋","冷柜","排队","会员卡","日常","挑拣","周末"],"市场":["批发","摊贩","行情","供需","集贸","人流","叫卖","生鲜","秤","斤两","早市","竞争","价格","货摊","讨价","顾客"],"商场":["专柜","扶梯","中庭","打折","试衣间","收银","品牌","楼层","导购","停车场","促销","橱窗","会员","退货","电梯","保安"],"餐厅":["菜单","服务员","包间","点菜","厨房","买单","筷子","招牌菜","排队","外卖","餐巾","上菜","大堂","餐位","酒水","小费"],"酒店":["前台","客房","房卡","退房","入住","大堂","行李","早餐","标间","房费","打扫","客房服务","钟点房","押金","预订","电梯"],"机场":["跑道","塔台","候机楼","安检","登机口","行李","摆渡车","停机坪","免税店","值机","航班","滑行"],"车站":["售票厅","候车室","站台","广播","检票口","出口","站前广场","时刻表","治安","寄存","换乘","人流"],"邮局":["邮票","信封","寄件","包裹","邮筒","挂号信","邮递员","汇款","柜台","邮戳","EMS","收件","邮政编码","明信片","称重","网点"],"图书馆":["书架","藏书","借阅","安静","阅读","读者","目录","管理员","自习","知识","索引","归还","阅览室","纸张"],"博物馆":["文物","展柜","解说","历史","青铜器","门票","安静","修复","藏品","故宫","参观","玻璃","年代"],"公园":["长椅","草坪","遛弯","健身器材","湖","门票","晨练","凉亭","花坛","散步","长跑","放风筝","石凳","树荫","广场舞","野餐"],"广场":["喷泉","鸽子","地砖","人群","音箱","跳舞","旗杆","长椅","夜市","雕塑","灯柱","围观","街头","宽阔","集会","花坛"],"地铁站":["闸机","站台","换乘","屏蔽门","扶梯","安检","线路图","车厢","刷卡","广播","出口","站厅","高峰","末班车","站名","标识"],"交易所":["挂牌","敲钟","交易员","撮合","竞价","行情屏","席位","监管","熔断","涨跌停","大宗","清算","做市","报价","期货","现货"],"药房":["处方","药师","药盒","柜台","医保卡","抓药","退烧药","药片","冷链","库存","说明书","剂量","开架","口罩","体温计","药店"],"监狱":["囚犯","铁窗","狱警","探视","服刑","牢房","放风","减刑","手铐","高墙","囚服","监规","越狱","劳改","出狱","刑期"],"消防队":["消防车","灭火","水带","云梯","火警","救援","防火","浓烟","制服","警报","演练","头盔","119","抢险","逃生","破拆"],"老板":["办公室","发工资","决策","应酬","秘书","签字","创业","分红","出差","指挥","管理","名片","豪车","裁员","开会","身家"],"员工":["打卡","工牌","工资","加班","请假","老板","工位","绩效","同事","离职","培训","年假","考勤","社保","工服","晋升"],"经理":["部门","开会","汇报","下属","指标","KPI","管理","考核","审批","出差","会议纪要","团队","决策","邮件","述职","调岗"],"同事":["工位","午饭","八卦","协作","群聊","借笔","加班","组队","闲聊","帮忙","团建","吐槽","交接","邻座","聚餐","请假"],"客户":["需求","合同","沟通","报价","签约","售后","投诉","拜访","回访","订单","谈判","甲方","信任","维护","满意度","付款"],"会计":["做账","凭证","报表","发票","审计","报税","借贷","账簿","对账","月末","财务","核算","报销","折旧","预算","Excel"],"销售":["业绩","提成","客户","拜访","电话","签单","话术","指标","跟单","渠道","回款","促销","谈判","名片","考核","压单"],"司机":["方向盘","油门","路况","导航","驾照","副驾","堵车","停车","长途","乘客","加油","后视镜","安全带","红灯","夜班","疲劳"],"厨师":["灶台","菜刀","炒锅","火候","配料","颠勺","厨房","菜单","摆盘","油烟","学徒","刀工","高汤","调味","出菜","围裙"],"服务员":["点单","端菜","包间","围裙","托盘","迎宾","抹布","催菜","小费","排班","顾客","收拾","上菜","微笑","加菜","翻台"],"保安":["门岗","制服","巡逻","登记","监控","门禁","夜班","对讲机","岗亭","放行","站岗","值班","敬礼","巡查","交接班","铁门"],"快递员":["快递车","包裹","派件","签收","驿站","电话","扫码","电动三轮","催单","双十一","面单","揽件","超时","送货","门牌","爬楼"],"程序员":["代码","键盘","加班","bug","咖啡","需求","发际线","GitHub","调试","深夜","逻辑","重构","屏幕","上线"],"设计师":["草图","配色","稿子","甲方","字体","排版","素材","灵感","改稿","像素","软件","方案","审美","输出","标注","定稿"],"律师":["法庭","辩护","当事人","合同","证据","法条","诉讼","西装","收费","正义","唇枪舌剑","卷宗","代理"],"医生":["医院","病人","诊断","处方","手术","白大褂","听诊器","护士","医学","救死扶伤","门诊","值班","药品","责任"],"护士":["医院","白大褂","打针","输液","夜班","病房","医嘱","温柔","口罩","护理","急救","巡房","疲惫"],"教师":["学校","学生","课堂","粉笔","教案","知识","育人","班主任","讲台","寒暑假","师范","尊敬","作业","耐心"],"警察":["治安","警车","制服","抓捕","报警","案件","派出所","巡逻","手铐","正义","危险","刑警","交通","守护"],"消防员":["消防车","水枪","云梯","头盔","火场","救援","119","警报","呼吸器","浓烟","制服","演练","破拆","担架","高温","逆行"],"军人":["军装","队列","哨兵","训练","营房","敬礼","军旗","武器","军衔","演习","站岗","迷彩","班长","口令","巡逻","边防"],"记者":["采访","话筒","摄像机","稿件","真相","发布会","现场","追问","编辑","截稿","舆论","奔波","记录"],"演员":["片场","剧本","导演","镜头","台词","化妆","替身","开机","杀青","试镜","剧组","灯光","走位","首映","明星","重拍"],"作家":["稿纸","灵感","深夜","小说","出版社","编辑","键盘","截稿","构思","手稿","签售","笔名","读者","专栏","书房","大纲"],"画家":["画布","调色盘","颜料","画笔","素描","展览","画室","写生","构图","油画","模特","签名","笔触","画廊","装裱","临摹"],"歌手":["舞台","麦克风","专辑","彩排","粉丝","演唱会","音准","伴奏","录音棚","走音","话筒","灯光","巡演","出道","音域","应援"],"科学家":["实验室","论文","假设","数据","显微镜","诺贝尔","好奇","严谨","失败","突破","同行评议","白大褂","探索","真理"],"工程师":["图纸","项目","参数","现场","设备","验收","方案","施工","调试","标准","加班","技术","接口","规范","评审","建造"],"农民":["土地","锄头","庄稼","播种","收割","汗水","节气","麦田","化肥","收成","谷仓","劳作","晒场","辛劳"],"渔民":["渔船","渔网","出海","码头","捕鱼","风浪","收网","鱼虾","晒网","船员","柴油","潮汐","冰鲜","船舱","海鸥","返航"],"理发师":["剪刀","推子","镜子","洗头","刘海","染发","吹风","发型","围布","理发椅","打薄","办卡","学徒","烫发","碎发","门店"],"外卖员":["电瓶车","头盔","订单","取餐","超时","差评","餐箱","导航","电梯","催单","跑单","雨衣","接单","骑手","送餐","红灯"],"主播":["直播间","弹幕","粉丝","打赏","带货","摄像头","开播","连麦","关注","榜一","流量","补光","话术","下播","人气","涨粉"],"兽医":["诊室","宠物","疫苗","听诊","笼子","打针","绝育","猫狗","病历","驱虫","洗澡","麻醉","手术台","主人","体温","领养"],"翻译":["笔译","口译","词典","语感","同传","术语","稿件","原文","校对","外语","逐字","交传","语序","润色","专业","耳麦"],"导游":["旗子","讲解","团队","景点","门票","大巴","游客","行程","扩音器","小费","集合","路线","购物点","领队","签到","注意事项"],"空姐":["客舱","安全带","餐车","登机","氧气面罩","广播","制服","推车","旅客","起飞","降落","毛毯","微笑","巡舱","服务铃","平飞"],"列车员":["车厢","查票","报站","铺位","开水","餐车","站台","乘务","行李架","换票","旅客","晚点","关门","值乘","卧铺","拖地"],"收银员":["扫码","小票","零钱","收银台","塑料袋","找零","排队","会员卡","装袋","刷脸","退货","盘点","现金","腰疼","轮班","验钞"],"保洁员":["拖把","抹布","垃圾桶","走廊","消毒","清扫","洗手间","水桶","工服","凌晨","杂物","擦玻璃","垃圾袋","异味","弯腰","打扫"],"门卫":["登记","门岗","钥匙","快递","访客","大爷","体温","放行","值守","岗亭","进出","保安服","巡逻","通报","铁门","夜班"],"电工":["电线","电闸","绝缘","万用表","接线","检修","短路","安全帽","配电箱","停电","螺丝刀","漏电","高空","抢修","零线","验电"],"木匠":["刨子","锯子","榫卯","木屑","墨线","凿子","工作台","木料","打磨","上漆","家具","尺子","手艺","徒弟","锤子","刨花"],"裁缝":["剪刀","皮尺","缝纫机","布料","量体","针线","锁边","熨斗","样板","改衣","纽扣","拉链","走线","试衣","手艺","线头"],"邮递员":["邮包","自行车","信件","邮筒","投递","挂号","门牌","邮戳","片区","送信","雨天","制服","分拣","签收","地址","早班"],"工资":["发薪日","银行卡","扣税","底薪","加班费","五险一金","条子","到手","涨薪","欠薪","月结","转账","薪资","福利","绩效","收入"],"税收":["纳税","发票","申报","税率","增值税","个税","税务局","起征点","抵扣","稽查","完税","退税","报税","凭证","滞纳金","免税"],"保险":["保单","保费","理赔","投保","受益人","车险","医保","业务员","免责","续保","定损","意外","寿险","退保","条款","保障"],"股票":["涨停","大盘","散户","券商","开户","K线","割肉","牛市","抄底","成交量","分红","市盈率","套牢","收盘","利好","韭菜"],"基金":["净值","基金经理","申购","赎回","定投","收益率","持仓","份额","募集","风险","指数基金","托管","波动","组合","费率","排行榜"],"贷款":["利息","还款","征信","抵押","额度","月供","房贷","审批","担保","逾期","放款","利率","本金","分期","借条","催收"],"存款":["存折","定期","活期","利息","柜台","银行卡","取款","存单","利率","储蓄","零存整取","ATM","密码","余额","转账","大额"],"合同":["签字","条款","甲方","乙方","违约","盖章","一式两份","期限","附件","定金","履行","解除","纠纷","律师","生效","补充协议"],"商标":["注册","品牌","侵权","类别","核准","异议","续展","标识","假冒","知识产权","申请","公告","专用权","代理","驳回","logo"],"专利":["申请","发明","权利要求","审查","授权","技术","侵权","实用新型","有效期","年费","检索","发明人","公开","驳回","保护","独占"],"发票":["抬头","税号","报销","开票","增值税","专票","作废","红冲","查验","金额","明细","盖章","电子票","抵扣","纸质","流水号"],"工资条":["底薪","绩效","补贴","扣款","社保","公积金","个税","实发","应发","加班","明细","签字","月份","奖金","考勤","核对"],"简历":["学历","经历","技能","照片","求职","投递","模板","自我评价","证书","实习","排版","一页","面试","邮箱","期望薪资","亮点"],"面试":["自我介绍","简历","提问","offer","笔试","HR","紧张","着装","复试","群面","薪资","离职原因","录用","等通知","岗位","寒暄"],"加班":["深夜","打车","泡面","调休","工时","项目","困","通宵","周末","电脑","灯","疲惫","加班费","进度","咖啡","凌晨"],"退休":["养老金","老年","遛弯","广场舞","社保","工龄","手续","回家","钓鱼","带孙","清闲","年龄","告别","茶","棋","养生"],"创业":["公司","融资","创始人","产品","风险","团队","市场","失败","创新","商业计划","投资人","坚持","增长","客户"],"破产":["清算","债务","法院","债权人","重整","倒闭","裁员","欠款","资产","拍卖","失信","注销","资不抵债","申报","变卖","失业"],"涨价":["成本","通胀","调价","标牌","顾客","抱怨","原料","利润","涨幅","物价","肉价","油价","促销","囤货","商家","贵"],"消费":["购物","刷卡","账单","冲动","省钱","折扣","付款","退货","钱包","花呗","预算","理性","剁手","分期","消费券","欲望"],"通货膨胀":["物价","货币","贬值","购买力","央行","加息","泡沫","菜价","存款","紧缩","通胀率","印钞","生活成本","缩水","工资","保值"],"汇率":["美元","兑换","外汇","牌价","升值","贬值","换汇","结汇","跨境","波动","中间价","旅行","手续费","货币","出国","报价"],"房地产":["房价","楼盘","首付","按揭","中介","学区房","户型","开盘","摇号","限购","物业","交房","装修","炒房","土地","售楼处"],"租金":["房东","押金","租约","月付","中介费","合租","水电","退租","涨租","二房东","看房","单间","转租","包物业","到期","租客"],"法律":["规则","法院","法官","律师","权利","犯罪","合同","惩罚","公平","宪法","诉讼","证据","正义","条文"],"犯罪":["嫌疑人","作案","证据","抓捕","判刑","动机","现场","指纹","逃逸","伏法","立案","审讯","同伙","赃物","自首","通缉"],"罚款":["违章","交警","罚单","扣分","缴纳","超速","违停","处罚","摄像头","滞纳金","警告","没收","违规","查处","金额","电子眼"],"身份证":["号码","照片","户口","复印","有效期","办理","补办","芯片","住址","姓名","实名","出示","丢失","派出所","临时","核验"],"户口":["迁入","籍贯","落户","户籍警","本子","农业","城镇","迁移","证明","户主","婚姻","出生","注销","登记","挂靠","随迁"],"政策":["出台","落实","补贴","文件","调整","民生","执行","试点","解读","利好","限制","监管","配套","宣传","落地","效果"],"公务员":["考试","编制","铁饭碗","笔试","政审","入职","窗口","坐班","稳定","晋升","考公","招录","岗位","福利","调岗","盖章"],"外交":["大使","谈判","条约","使馆","访问","声明","签证","护照","制裁","峰会","建交","领事","礼宾","公报","磋商","国书"],"城管":["摊贩","执法","罚款","巡逻","制服","清理","占道","暂扣","冲突","市容","巡查","摊子","劝离","城管车","举报","整治"],"事业单位":["编制","考试","稳定","窗口","福利","晋升","档案","职称","打卡","财政","招考","岗位","调岗","退休","评优","会议"],"国企":["编制","稳定","福利","铁饭碗","领导","会议","年终奖","论资排辈","党建","效益","改制","招聘","工龄","纪检","考核","分房"],"外企":["英文","邮件","弹性","年假","汇报","总部","出差","咖啡","扁平","裁员","时差","视频会议","猎头","薪资","PPT","氛围"],"社区":["居委会","网格员","邻居","业主群","通知","活动室","大爷","大妈","楼道","调解","志愿","宣传栏","登记","独居老人","物业","公告"],"物业":["物业费","保安","维修","电梯","楼道","业主","投诉","停车位","绿化","保洁","门禁","管家","报修","公摊","停水","公示"],"业主":["房子","物业费","诉求","业主群","沟通","交房","装修","车位","投诉","邻居","产权","大会","签名","横幅","协商","物业公司"],"居委会":["大妈","通知","调解","登记","宣传","社区","公章","表格","走访","志愿","活动","议事","证明","楼道","慰问","排查"],"城市":["人口","建筑","交通","地铁","街道","商圈","公园","污染","夜晚","房价","市民","规划师","灯火","喧嚣"],"乡村":["田野","炊烟","鸡鸣","土路","稻田","村口","老屋","小河","集市","留守","插秧","晒谷","狗吠","乡亲","宁静","水牛"],"街道":["路灯","梧桐","行人","店铺","斑马线","深夜","雨水","自行车","叫卖","拐角","熟悉","流浪猫","霓虹"],"小区":["门禁","电梯","楼道","停车位","物业","绿化","快递柜","健身区","邻居","单元门","阳台","遛狗","保安亭","垃圾站","车位","滑梯"],"写字楼":["电梯","工位","玻璃幕墙","前台","会议室","加班","咖啡","打卡","楼层","中央空调","保洁","访客","商务","物业","大堂","隔断"],"购物中心":["美食城","电影院","儿童乐园","地下车库","快闪店","连锁店","奶茶店","导视牌","客流","招商","动线","主力店","开业","周年庆","试营业","逛街"],"红绿灯":["路口","闯灯","倒计时","斑马线","违章","摄像头","等待","放行","交警","信号","拥堵","秒数"],"斑马线":["行人","礼让","过马路","白线","红绿灯","低头族","刹车","路口","推婴儿车","敬老","减速","违章","抓拍","车流","停步","通行"],"公交站":["站牌","候车","上车","投币","刷卡","线路","挤","等车","末班","司机","站台","雨棚","排队","报站","换乘","长椅"],"停车场":["车位","道闸","扫码","计费","地库","倒车","立柱","寻车","月卡","潮汐","监控","坡道"],"路口":["拐弯","红绿灯","斑马线","交警","车流","拥堵","事故","转向灯","堵死","行人","摄像头","喇叭","抢行","掉头","高峰","指挥"],"夜市":["摊位","烤串","灯串","人挤人","小吃","吆喝","城管","塑料凳","啤酒","烟火气","臭豆腐","讨价","收摊","人声","灯笼","油烟"],"菜市场":["摊位","青菜","称重","讨价","活鱼","早市","塑料袋","吆喝","肉铺","豆制品","零钱","新鲜","摊主","水产","杀价","菜篮"],"商铺":["门面","租金","卷帘门","招牌","转让","客流量","营业","房东","装修","收银","货架","店员","水电","试营业","打烊","地段"],"摊贩":["推车","城管","吆喝","占道","秤","收款码","早市","烤肠","罚单","小本生意","露天","收摊","叫卖","零钱","风吹日晒","躲"],"打卡":["指纹","迟到","考勤","上班","下班","工牌","机器","代打","早退","月度","记录","刷脸","排队","准点","提醒","异常"],"出差":["高铁","行李箱","酒店","报销","行程","会议","异地","客户","机票","发票","疲惫","报备","差旅","赶车","返程","标准"],"开会":["会议室","投影","议题","纪要","发言","白板","迟到","走神","结论","主持人","举手","讨论","议程","记录","散会","电话"],"辞职":["交接","离职","申请","领导","签字","最后一天","同事","告别","手续","跳槽","解约","工牌","归还","结算","欢送","新工作"],"招聘":["简历","岗位","面试","HR","发布","筛选","录用","猎头","薪资","入职","校招","社招","要求","福利","双休","试用期"],"社保":["五险一金","缴费","医保","养老","公积金","断缴","基数","参保","转移","个人账户","报销","退休","补缴","权益","查询","单位"],"医保":["报销","挂号","医保卡","定点医院","药费","住院","门诊","起付线","目录","结算","异地","参保","个人账户","比例","自费","刷卡"],"单位":["领导","同事","公章","打卡","食堂","编制","开会","报销","工位","门卫","奖金","请假","调岗","退休","档案","考勤"],"公章":["盖章","审批","文件","红泥","法人","合同","借出","登记","权限","印鉴","圆章","防伪","丢失","备案","用印","签字"],"档案":["牛皮纸袋","密封","人事","调动","存放","查阅","转递","履历","编号","柜子","机密","归档","提取","材料","盖章","保管"],"考勤":["打卡","迟到","早退","请假","全勤","记录","指纹","加班","排班","旷工","补卡","月度","统计","扣钱","异常","刷脸"],"绩效":["考核","指标","KPI","评分","奖金","排名","述职","目标","打分","末位","面谈","季度","晋升","评级","系数","努力"],"出租车":["计价器","招手","网约","起步价","司机","后排","发票","堵车","绕路","空车","夜班","打表"],"公交车":["投币","刷卡","报站","司机","车厢","扶手","让座","高峰","线路","站牌","末班","空调","下车铃","车票","拥挤","候车"],"高铁":["列车","站台","时速","车票","商务座","窗景","准点","铁轨","返乡","春运","平稳","检票","干线"],"网购":["下单","快递","购物车","评价","退款","优惠券","包邮","客服","物流","差评","秒杀","支付","收货","拆箱","七天无理由","店铺"],"广告":["海报","投放","曝光","创意","点击","预算","代言","弹窗","横幅","刷屏","转化","甲方","渠道","效果","文案","植入"],"展会":["展位","展台","名片","洽谈","样品","观众","布展","撤展","主办","签到","宣传册","采购商","展会证","人流量","订单","展馆"],"中介":["房源","佣金","带看","二手房","介绍费","合同","租客","买家","挂牌","跑盘","电话","门店","成交","跳单","手续费","信任"],"数学":["数字","公式","几何","代数","计算","证明","逻辑","数学家","方程","函数","抽象","应用","题目","精确"],"物理":["力学","牛顿","能量","动量","电场","磁场","光学","热学","相对论","量子","实验","定律","单位","粒子","测量"],"化学":["元素","分子","原子","反应","化合物","溶液","酸碱","催化剂","周期表","有机物","实验室","试剂","氧化","燃烧","配平"],"生物":["细胞","基因","蛋白质","物种","进化","生态","植物","动物","微生物","遗传","组织","器官","显微镜","分类","生命"],"地理":["地图","地形","气候","经纬度","板块","山脉","河流","海洋","时区","海拔","土壤","洋流","地震","卫星图","区域"],"历史":["朝代","史料","考古","年代","文明","战争","变革","帝王","编年","遗址","文献","断代","博物馆","变迁","纪年"],"天文":["恒星","行星","星系","黑洞","宇宙","望远镜","轨道","引力","太阳","月球","彗星","星云","光年","天文台","观测"],"医学":["诊断","手术","药物","症状","病历","临床","内科","外科","影像","疫苗","护士","医院","治疗","检验","处方"],"心理学":["情绪","认知","记忆","行为","人格","焦虑","动机","潜意识","量表","咨询","知觉","偏见","共情","压力","成长"],"经济学":["供给","需求","市场","价格","通胀","国内生产总值","货币","利率","投资","贸易","成本","垄断","就业","财政","税收"],"哲学":["存在","真理","伦理","逻辑","意识","自由","辩证法","形而上学","苏格拉底","康德","怀疑","命题","价值","思辨","智慧"],"几何":["三角形","圆形","角度","平行","垂直","面积","体积","勾股定理","坐标","对称","欧几里得","投影","曲线","切线","立体"],"微积分":["极限","导数","积分","函数","连续","微分","曲线","牛顿","莱布尼茨","无穷","面积","变化率","级数","方程","逼近"],"概率":["随机","样本","期望","方差","分布","独立","贝叶斯","骰子","抽签","统计","似然","预测","正态","事件","频率"],"统计":["数据","样本","均值","中位数","回归","方差","调查","图表","显著性","普查","抽样","相关","报表","误差","趋势"],"进化":["达尔文","自然选择","变异","物种","适应","化石","遗传","基因","猿","分支","生存竞争","隔离","演化树","生态位","共存"],"细胞":["线粒体","细胞膜","细胞核","分裂","组织","核糖体","细胞质"],"光合作用":["叶绿体","叶绿素","阳光","二氧化碳","氧气","葡萄糖","植物","叶片","气孔","碳循环","水分","有机物","暗反应","光反应"],"元素":["周期表","原子序数","金属","氧气","氢气","碳","同位素","化合价","门捷列夫","化学式","单质","稀有气体","放射性","原子"],"分子":["原子","化学键","结构式","分子量","极性","空间构型","聚合","有机物","水分子","氢键","分子式","晶体","反应"],"原子":["原子核","电子","质子","中子","轨道","元素","能级","放射性","玻尔","化学键","微观","周期律","裂变"],"电磁":["电场","磁场","麦克斯韦","电流","感应","电磁波","天线","无线电","磁铁","洛伦兹力","屏蔽","变压器","电磁炉"],"引力":["万有引力","牛顿","苹果","潮汐","轨道","黑洞","广义相对论","重力","加速度","天体","失重","时空","质量"],"光学":["折射","反射","透镜","光谱","激光","棱镜","焦距","干涉","衍射","彩虹","显微镜","偏振","成像"],"显微镜":["目镜","物镜","放大","载玻片","细胞","焦距","光源","切片","观察","细菌","电子显微镜","调焦","标本"],"实验室":["试管","烧杯","试剂","酒精灯","通风橱","白大褂","安全","记录","滴定","培养皿","天平","仪器","危险品"],"论文":["摘要","引用","期刊","审稿","数据","实验","结论","查重","参考文献","投稿","学位","学术","同行评议"],"假说":["验证","实验","证据","理论","变量","推翻","观察","推理","科学方法","猜想","对照组","结论"],"望远镜":["镜筒","物镜","目镜","焦距","天文台","观测","星空","折射","反射","三脚架","银河","放大"],"化石":["岩层","恐龙","骨骼","年代","发掘","古生物","遗迹","标本","碳定年","沉积","博物馆","演化"],"病毒":["感染","传播","免疫","变异","宿主","核酸","疫苗","潜伏","消毒","隔离","流行","检测"],"神经":["神经元","突触","大脑","信号","反射","传导","递质","脊髓","疼痛","麻醉","损伤","网络"],"互联网":["网络","网站","浏览器","服务器","网民","信息","光纤","社交","电商","协议","带宽","云端","连接","匿名"],"软件":["程序","系统","安装","更新","版本","授权","界面","功能","补丁","开发者","应用","兼容","卸载"],"硬件":["主板","内存","显卡","电源","机箱","处理器","散热","接口","外设","组装","驱动","性能","硅"],"芯片":["晶体管","硅片","纳米","算力","中央处理器","图形处理器","光刻机","集成电路","制程","散热","功耗","摩尔定律","晶圆","封装","半导体"],"处理器":["核心","主频","缓存","指令","线程","架构","英特尔","跑分","睿频","功耗","制程","运算","散热"],"内存":["容量","频率","颗粒","带宽","缓存","读写","双通道","延迟","掉电","溢出","扩容","插槽"],"硬盘":["存储","容量","机械硬盘","固态硬盘","分区","坏道","读写","备份","接口","转速","格式化","磁道"],"服务器":["机房","机架","运维","负载","集群","带宽","备份","宕机","数据库","冷却","托管","扩容"],"云计算":["弹性","虚拟机","容器","云存储","按量付费","数据中心","分布式","扩容","迁移","微服务","可用区","托管"],"大数据":["数据量","分析","挖掘","画像","报表","采集","清洗","标签","实时","分布式","可视化","推荐"],"人工智能":["机器学习","神经网络","大模型","算法","数据","算力","ChatGPT","机器人","自动化","伦理","深度学习","训练","智能","取代"],"算法":["步骤","排序","复杂度","数据结构","程序","优化","递归","数学","效率","搜索","逻辑","实现","竞赛","代码"],"程序":["代码","程序员","bug","编译","逻辑","函数","软件","调试","运行","需求","版本","测试","重构","键盘"],"代码":["函数","变量","注释","缩进","提交","重构","编译","报错","仓库","分支","命名","规范"],"数据库":["数据表","字段","索引","查询语句","事务","主键","备份","读写","关系","分库","死锁","视图"],"网络安全":["防火墙","漏洞","病毒","黑客","加密","钓鱼","补丁","木马","权限","入侵","扫描","认证"],"密码":["安全","数字","记忆","泄露","锁","加密","验证","忘记","账户","指纹","破解","隐私","登录","重置"],"区块链":["比特币","区块","哈希","去中心化","钱包","挖矿","智能合约","节点","账本","共识","以太坊","代币"],"虚拟现实":["头显","沉浸","眼动","手柄","建模","眩晕","场景","交互","分辨率","游戏","元宇宙","空间定位"],"无人机":["螺旋桨","航拍","遥控","电池","云台","图传","航线","禁飞区","植保","巡检","反制","飞手"],"雷达":["电磁波","探测","回波","天线","测距","隐身","目标","扫描","毫米波","预警","追踪","盲区"],"激光":["光束","切割","焊接","测距","相干","光纤","打印","手术","聚光","波长","雷达","准直"],"传感器":["探测","信号","温度","压力","光电","采集","灵敏度","校准","探头","物联网","精度","模拟量"],"疫苗":["免疫","抗体","接种","病毒","灭活","加强针","冷链","预防","临床","副作用","群体免疫","针剂"],"基因":["双螺旋","染色体","遗传","突变","测序","编辑","蛋白质","亲子","遗传病","表达","克隆","密码子"],"纳米":["尺度","材料","涂层","微粒","半导体","显微镜","分子","石墨烯","药物递送","精度","碳管","薄膜"],"量子":["叠加","纠缠","比特","计算","通信","隧穿","玻尔","干涉","退相干","密钥","量子力学","能级"],"机器人":["机械","传感器","程序","人工智能","工业","服务","人形","替代","维修","科幻","马达","协作","工厂","未来"],"智能手机":["屏幕","电池","摄像头","处理器","应用","系统","指纹","快充","信号","手机壳","无线充电","存储"],"电脑":["屏幕","键盘","鼠标","CPU","软件","程序","互联网","办公","游戏","散热","硬盘","程序员","桌面","死机"],"相机":["镜头","光圈","快门","照片","摄影","胶卷","像素","摄影师","三脚架","构图","曝光","回忆","闪光灯","对焦"],"耳机":["音乐","降噪","耳塞","蓝牙","通勤","隔绝","音量","充电","白噪音","唱歌","线","沉浸"],"键盘":["按键","打字","字母","机械轴","空格","快捷键","输入法","敲击","手速","灰尘","电竞","鼠标","噼啪"],"屏幕":["分辨率","亮度","刷新率","像素","校色","尺寸","蓝光","面板","触控","划痕","边框","色域"],"电池":["充电","容量","锂电","电芯","续航","快充","循环","鼓包","正极","电解液","掉电","散热"],"充电宝":["容量","快充","输出","电芯","过充","接口","便携","安检","数显","放电","安全","额定"],"路由器":["无线","宽带","网速","频段","天线","穿墙","拨号","管理","掉线","信号","中继","接口"],"操作系统":["界面","内核","驱动","应用","更新","权限","桌面","命令行","兼容","安装","补丁","稳定"],"编程":["语法","变量","函数","逻辑","调试","代码","项目","语言","注释","框架","版本","学习"],"开源":["代码","许可证","社区","贡献","仓库","分支","代码托管","协作","文档","免费","维护","协议"],"加密":["密钥","算法","明文","密文","解密","签名","证书","传输","破解","哈希","隐私","安全"],"物联网":["智能家居","传感器","联网","网关","远程","数据","设备","联动","摄像头","协议","场景","芯片"],"光刻机":["芯片","掩膜","曝光","波长","极紫外","精度","台积电","制程","光刻胶","对准","镜头","工艺"],"新能源":["电池","光伏","风电","充电桩","碳中和","电动车","储能","电网","清洁","环保","补贴","转型"],"核能":["反应堆","铀","裂变","辐射","核电","蒸汽","冷却","安全","废料","聚变","能量","发电"],"打印":["喷墨","纸张","文件","排版","耗材","复印件","双面","卡纸","扫描","打印店","墨水","设置"],"视频":["剪辑","播放","分辨率","帧率","字幕","音频","压缩","上传","弹幕","转码","素材","时长"],"网络":["互联网","信号","路由器","带宽","协议","连接","WiFi","服务器","安全","速度","断线","数据","节点","覆盖"],"数据":["采集","存储","分析","报表","备份","清洗","隐私","字段","表格","增长","看板","日志"],"显示器":["面板","分辨率","色域","刷新率","接口","支架","亮度","校色","尺寸","漏光","护眼","响应"],"显卡":["显存","核心","散热","驱动","帧率","渲染","功耗","接口","超频","矿卡","光追","跑分"],"鼠标":["滚轮","按键","灵敏度","无线","光标","手感","垫子","点击","回报率","有线","侧键","握法"],"摄像头":["镜头","像素","监控","夜视","对焦","画面","供电","广角","录像","隐私","红外","存储"],"充电桩":["快充","接口","电费","停车位","功率","扫码","排队","慢充","电网","峰谷","续航","预约"],"智能家居":["语音","网关","灯控","窗帘","场景","联动","远程","传感器","音箱","安防","配网","自动化"],"电影院":["银幕","座椅","爆米花","投影","音响","场次","选座","海报","排片","散场","检票","包场"],"地铁":["通勤","站台","早高峰","换乘","隧道","耳机","陌生人","末班车","扶梯","拥挤","广告","刷卡","城市","疲惫"],"公交":["站牌","刷卡","司机","线路","排队","座位","末班车","投币","报站","拥挤","候车","调度"],"火车":["铁轨","车厢","车站","蒸汽","高铁","乘客","旅途","汽笛","时刻表","站台","卧铺","风景","晚点","春运"],"飞机":["机翼","发动机","机场","飞行员","航线","天空","乘客","颠簸","登机","坠机","航空","云端","商务舱","机票"],"轮船":["甲板","海鸥","汽笛","港口","集装箱","海员","晕船","远航","螺旋桨","客舱","潮水","渡轮","漂泊"],"自行车":["车链","踏板","车铃","轮胎","骑行","上学","共享单车","坡道","刹车","平衡","风","小巷","铃铛"],"摩托车":["引擎","头盔","油门","离合","排量","轮胎","骑士","轰鸣","挡位","护具","弯道","牌照"],"卡车":["货厢","柴油","载重","司机","物流","高速","装卸","挂车","轮胎","货运","限重","尾板"],"隧道":["山体","灯光","挖掘","地铁","穿行","黑暗","出口","盾构机","通风","回声","漫长","贯通"],"桥梁":["桥墩","拱形","斜拉索","悬索","跨江","承重","桥面","通车","护栏","伸缩缝","沉降","引桥"],"大坝":["水库","泄洪","发电","蓄水","混凝土","闸门","三峡","防洪","淤积","坝体","灌溉","移民"],"港口":["码头","集装箱","货轮","吊机","海鲜","灯塔","潮汐","装卸","海关","出海","汽笛","贸易","远方"],"立交桥":["匝道","环形","桥墩","车流","分流","上跨","下穿","汇入","指示牌","限高","拥堵","绕行"],"摩天楼":["玻璃幕墙","电梯","抗震","钢结构","天际线","楼层","地基","阻尼器","观光层","消防","风压","地标"],"高速公路":["车道","匝道","收费站","限速","服务区","护栏","应急道","测速","路肩","隧道","事故","通行费"],"导航":["路线","定位","拥堵","语音","地图","偏离","终点","途经","重算","离线","限行","车位"],"驾照":["科目","考试","路考","扣分","违章","年审","学员","教练","换证","实习期","体检","记分"],"建筑":["混凝土","图纸","梁柱","地标","鸟巢","空间","光","尺度","贝聿铭","施工","立面","城市","结构","美学"],"混凝土":["水泥","砂石","水灰比","浇筑","养护","搅拌","凝固","泵送","裂缝","强度","模板","振捣"],"钢筋":["直径","绑扎","焊接","屈服","配筋","锈蚀","螺纹","骨架","承重","检测","保护层","箍筋"],"砖":["砌墙","砂浆","红砖","空心砖","窑炉","灰缝","承重","隔墙","码放","质量","手工","墙砖"],"玻璃幕墙":["采光","反光","隔热","高层","面板","密封","清洁","抗风","铝框","节能","安全","施工"],"地基":["打桩","承载","沉降","土方","勘察","基坑","夯实","桩基","冻土","抗震","开挖","支护"],"消防":["喷淋","灭火器","疏散","烟感","通道","防火门","报警","演练","消防栓","楼梯","检查","烟道"],"装修":["水电","瓷砖","油漆","吊顶","柜子","地面","预算","工期","通风","甲醛","卫浴","木工"],"户型":["客厅","卧室","厨房","采光","朝向","面积","通风","玄关","阳台","动线","层高","得房率"],"抗震":["结构","阻尼","设防","烈度","地基","摇摆","加固","变形","支座","规范","隔震","汶川"],"足球":["球","球场","球员","球门","梅西","世界杯","射门","观众","教练","红牌","草地","越位","球迷","冠军"],"篮球":["球","篮筐","球场","乔丹","投篮","运球","扣篮","NBA","球队","裁判","犯规","观众","身高","冠军"],"乒乓球":["球拍","发球","旋转","弧圈","削球","台面","球网","世界冠军","快攻","胶皮","回合","计分"],"羽毛球":["球拍","后场","高远球","扣杀","网前","单打","双打","步伐","场馆","换球","发球","拉吊"],"网球":["球拍","发球","底线","大满贯","红土","抢七","截击","上旋","双误","草地","破发","球童"],"排球":["拦网","二传","主攻","一传","发球","扣球","轮转","网高","自由人","局点","接应","赛场"],"游泳":["自由泳","蛙泳","蝶泳","仰泳","泳池","换气","转身","跳水","泳镜","浮力","划水","接力"],"跑步":["配速","跑鞋","心率","拉伸","步频","跑道","呼吸","膝盖","热身","出汗","晨跑","记录"],"瑜伽":["体式","呼吸","冥想","拉伸","平衡","瑜伽垫","柔韧","课程","静心","拜日式","教练","放松"],"健身":["增肌","减脂","器械","深蹲","哑铃","蛋白粉","组数","私教","体重","拉伸","计划","出汗"],"马拉松":["配速","公里","抽筋","补给","撞墙","完赛","号码布","报名","训练","赛道","关门","冲刺"],"滑雪":["雪板","雪道","缆车","摔跤","护具","雪镜","双板","单板","粉雪","雪杖","坡道","索道"],"登山":["背包","帐篷","海拔","氧气","雪山","向导","路线","高反","岩钉","冲顶","冰镐","营地"],"围棋":["棋盘","黑子","白子","定式","打劫","布局","官子","段位","棋谱","目数","让子","柯洁"],"象棋":["楚河汉界","车马炮","将士象","残局","开局","将军","棋盘","卒子","记谱","认输","棋友","对弈"],"麻将":["筒条万","胡牌","碰牌","杠牌","吃牌","庄家","洗牌","听牌","番型","牌桌","东家","点炮"],"扑克":["洗牌","发牌","花色","顺子","同花顺","斗地主","大小王","出牌","牌局","输赢","记牌","底牌"],"裁判":["哨子","黄牌","红牌","判罚","越位","计时","争议","视频回放","规则","边裁","主裁","记录台"],"奥运":["五环","圣火","金牌","开幕式","国家队","纪录","申办","举重","田径","火炬手","场馆","奖牌榜"],"球拍":["手柄","拍面","胶皮","穿线","磅数","握拍","甜区","拍套","底板","护边","挥拍","碳素"],"球场":["草皮","球门","边线","看台","灯光","更衣室","罚球区","场地","球网","观众","积水","养护"],"教练":["战术","训练","暂停","换人","布置","带队","助教","临场","复盘","纪律","激励","赛季"],"电影":["导演","演员","银幕","剧本","镜头","影院","票房","剧情","配乐","摄影","首映","观众","奖项","剪辑"],"电视剧":["集数","追剧","收视","编剧","主演","剧情","卫视","弹幕","大结局","番外","档期","剧集"],"综艺":["嘉宾","主持","游戏环节","真人秀","剪辑","收视","舞台","笑点","季播","话题","互动","花絮"],"动漫":["声优","番剧","漫画","分镜","二次元","剧场版","手办","国漫","连载","画风","角色","追番"],"游戏":["关卡","手柄","存档","副本","皮肤","电竞","剧情","充值","联机","引擎","外挂","成就"],"演唱会":["门票","舞台","灯光","安可","乐队","粉丝","荧光棒","音响","曲目","场馆","抢票","合唱"],"话剧":["舞台","台词","幕间","演员","排练","剧场","剧本","灯光","谢幕","独白","观众","巡演"],"展览":["展品","展厅","策展","门票","导览","布展","文物","画作","研讨会","海报","打卡","文创"],"直播":["弹幕","主播","打赏","流量","带货","连麦","镜头","粉丝","开播","榜一","封禁","回放"],"短视频":["剪辑","点赞","推荐","爆款","算法","竖屏","流量","挑战","封面","完播","博主","刷屏"],"剧本":["角色","台词","场景","冲突","反转","分场","大纲","伏笔","对白","改编","结构","结局"],"明星":["粉丝","经纪","热搜","代言","红毯","八卦","作品","出道","人设","探班","片酬","塌房"],"票房":["预售","排片","首日","破亿","档期","口碑","院线","分账","上座率","黑马","纪录","撤档"],"弹幕":["屏幕","吐槽","刷屏","屏蔽","时间轴","互动","高能","表情","关键词","观众","氛围","铺满"],"音乐":["声音","旋律","节奏","乐器","歌手","专辑","耳机","演唱会","情感","钢琴","流行","古典","乐谱","共鸣"],"爱情":["玫瑰","深夜电话","牵手","心跳","车站","誓言","拥抱","迟到","耳语","合照","雨天","皱纹","柴米油盐","白头","笨拙","早餐","等待"],"憎恨":["咬牙","冷眼","旧账","刀口","反锁","摔门","火苗","伤口","结痂","背影","夜里","不肯","余温"],"喜悦":["嘴角","阳光","雀跃","转圈","汽水","笑出声","眉梢","红晕","轻快","踩着云","撒花","亮堂","心口发烫","小跑","哼歌"],"愤怒":["拍桌","涨红","青筋","砸门","摔杯","炸开","雷声","瞪眼","嘴唇发抖","反击","余震","冷却","不甘"],"哀伤":["灰天","旧毛衣","低垂","空椅","呜咽","落叶","湿眼眶","长夜","遗物","风铃","曲终","雨打窗","褪色","告别"],"欢乐":["笑闹","鞭炮","碰杯","追逐","糖葫芦","喧哗","节拍","跳起来","合影","彩带","欢呼","满脸通红","午后"],"恐惧":["黑巷","屏息","冷汗","回头","楼梯","门缝","颤抖","影子","电梯","脚步声","缩进被角","未知","紧抓"],"孤独":["深夜","一个人的房间","沉默","影子","空椅子","路灯","远方","思念","人群","耳机","自由","冷","窗"],"思念":["旧信","月台","翻照片","远方","名字","归期","同款","发呆","枕边","邮戳","长街","未接来电","年头","旧歌","年关","雨"],"嫉妒":["刺","比较","朋友圈","咬牙","暗处","窃窃","凭什么","灼烧","酸涩","翻涌","侧目","沉默"],"羞愧":["低头","脸红","想钻地缝","道歉","躲闪","耳根","退半步","遮脸","难堪","不敢看","讪讪","手不知道放哪"],"后悔":["如果","倒带","深夜","删掉","旧路","那句话","回不去","反复","叹气","失眠","钥匙","站台","最后一班","错身","捶腿","早知"],"期待":["倒数","门口","邮差","铃声","明天","攥紧","抬头","日历","心跳","张望","车票","新鞋","天亮","预备"],"失望":["落空","垂手","冷掉","空椅子","未接","退回","熄灭","转头","算了","关门","雨停","无声"],"希望":["种子","微光","地平线","春天","破土","清晨","远方","撑着","灯","信","再试一次","嫩芽","候鸟","窗口","启程"],"绝望":["枯井","断线","灰烬","蜷缩","塌陷","下坠","尽头","风停","冷透","最后一滴","荒原","闭眼"],"平静":["湖面","呼吸","午后","茶","风停","合上书","钟摆","浅睡","水纹","远山","空白","安稳","窗纱"],"焦虑":["深夜","心跳","未读消息","截止日期","未来","呼吸","手心出汗","失眠","比较","失控","咖啡","屏幕","逃避","冥想"],"幸福":["清晨","阳光","家人","一碗面","拥抱","笑声","满足","平静","小狗","假期","瞬间","期待","健康","被需要"],"痛苦":["钝刀","夜里","蜷","咬牙","旧伤","喘息","拉扯","裂缝","硬撑","熬","泪","孤","漫长"],"温柔":["毛衣","轻声","暖手","晚风","摸头","热牛奶","棉","雨伞","掌心","低眉","柔光","哄","慢慢说"],"倔强":["抿嘴","不回头","攥拳","站起来","硬扛","撞","反着来","膝盖","疤","顶风","孤勇","再试"],"释怀":["呼一口气","松手","旧照片","笑一下","风","转身","海边","不再问","合上","黄昏","旧账清零","散步","明白"],"执念":["反复","旧名字","攥着","回放","一根线","绕","等","偏要","守","梦里","旧地","十年","刻"],"放下":["松手","背包","台阶","呼气","旧衣","删掉","转身","清空","远行","轻装","看开","归零","不追","往前看"],"心动":["抬眼","愣住","心跳漏一拍","衣角","走廊","对视","脸红","偷偷看","汽水","走廊尽头","小纸条","慌"],"委屈":["抿嘴","眼泪打转","解释不清","喉咙","没人问","鼻子酸","忍住","角落","凭什么","咽下","红眼","抱","缩"],"感动":["鼻酸","握手","眼泪","旧伞","记得","默默","回望","胸口","说不出话","点头","拥抱","微光","暖流"],"悲伤":["灰","雨","空","低头","旧歌","照片","凉","长夜","落叶","背影","无声","沉"],"惊喜":["睁大眼","捂嘴","彩带","礼物","忽然","欢呼","蛋糕","门后","尖叫","不敢相信","转圈","笑泪"],"厌恶":["皱眉","后退","别过脸","馊","油腻","啧","擦手","绕开","反胃","隔夜","嫌","关门"],"愧疚":["低头","欠","那件事","补偿","不敢提","亏欠","道歉","背影","绕开","记得","还","心里堵","无声"],"眷恋":["老屋","门槛","炊烟","手写","巷口","味道","不舍","回头","槐树","夏夜","外婆","行李","再看一眼","根"],"惆怅":["暮色","站台","未寄","低声","旧曲","茶凉","倚窗","说不清","长街","细雨","模糊","归鸟","散场","余味"],"不安":["翻身","看手机","门锁","脚步声","手心","预兆","反复","数羊","坐立","疑","风吹草动","攥被角","心跳"],"欣慰":["点头","笑纹","长舒一口气","长大","回望","值得","老照片","灯下","成绩单","放心","挺直","轻声","圆满"],"寂寞":["空街","单人餐","电视声","半夜","拖鞋","回声","旧沙发","无聊","散步","雨","钟","影子","翻书","一个人"],"热烈":["夏","奔跑","大笑","拥抱","汗","烟花","吻","燃烧","喊","撞","红","鼓点","扑上去","灼","不顾一切"],"冷漠":["侧脸","玻璃","擦肩","不回应","白墙","关窗","公事公办","后退","冷气","路人","疏离","隔"],"慈悲":["俯身","灯","放过","眼泪","众生","老僧","粥","抚","低眉","庙","不忍","渡"],"忐忑":["手心汗","来回走","看消息","屏息","预演","心跳","咬唇","电话","深呼吸","数秒","悬","夜里"],"迷惘":["雾","岔路","站台","看天","无方向","地图","原地","抽烟","走神","夜路","不知","空"],"懊悔":["拍腿","早知道","删了","旧账","雨夜","补","迟","空杯","闭眼","指甲","站台","叹气"],"羞耻":["脸烫","低头","耳根","被看见","遮","角落","难堪","恨不得","躲","红","沉默","手"],"怜悯":["俯身","流浪","老人","雨","低语","柔软","叹","破碗","冷风","喂","不忍","手"],"敬仰":["抬头","碑","名字","仰望","古树","挺直","追随","光","深深一躬","专注","高山","肃然","眼里有光"],"牵挂":["电话","天气","归期","旧屋","饭菜","毛衣","门口","远方","惦记","白发","信","视频","心"],"怀念":["旧照片","老歌","校服","夏天","走神","街角","再也","黄昏","翻","尘","回不去","轻"],"渴望":["伸手","窗","海","奔跑","喉咙","远处","干","火","望","想要","追","热","空杯","仰头"],"厌倦":["重复","闹钟","地铁","翻白眼","丢开","麻木","敷衍","关掉","懒得","转身","空","累"],"疲惫":["肩","深夜","地铁","靠窗","闭眼","哈欠","拖","沙发","解扣","放空","眼袋","一步步","不想动"],"悲悯":["俯身","众生","雨","老屋","目光","叹","低","苦","灯","渡","落泪","不忍"],"虔诚":["跪","香","合十","低眉","钟","石阶","烛","念","一步一拜","晨","雪","经","光","愿"],"坦然":["笑","摊手","坐下","茶","看云","无惧","承认","稳","明白","放下","路","风吹","自在"],"从容":["慢","茶","翻书","步","稳","退一步","不慌","衣角","拈","应","缓","定"],"沉静":["深水","书页","茶烟","雪","呼吸","闭目","石","无声","长","定","影","木","微光"],"恍惚":["午后","光斑","走神","梦","晃","慢半拍","茶凉","车声","雾","虚","醒","愣"],"清醒":["冷水","凌晨","光","明白","坐直","决断","冷","看穿","起身","刀","白","静"],"冲动":["拍桌","脱口","跑","买","冒险","不顾","撞","喊","跳","先做","热血","冲"],"克制":["抿嘴","收手","深呼吸","数到十","垂下","忍住","合上","沉默","握紧","守","慢","尺"],"自由":["风","大海","翅膀","出走","旷野","选择","枷锁","呼吸","独立","旅行","天空","代价","责任","边界"],"理想":["远方","灯塔","少年","图纸","演讲","星空","启程","不服","奔赴","白衬衫","山顶","坚持","汗","行囊"],"信仰":["山","钟","跪","光","经","碑","火","誓","朝圣","坚定","背","守","渡","愿"],"命运":["铁轨","岔口","骰子","潮","手纹","偶遇","不可逆","绳","雾","安排","深夜","站台","认","浪"],"灵魂":["深夜","眼睛","窗","火","独白","重量","透明","镜","深处","叩","骨","影","醒"],"时间":["秒","分","小时","日历","钟表","过去","未来","流逝","年龄","效率","等待","记忆","四季","永恒"],"空间":["走廊","回声","留白","围墙","距离","台阶","屋顶","深处","尺","辽阔","格子","呼吸","空"],"美丽":["晨光","花瓣","侧脸","倒影","青瓷","雪","裙摆","薄雾","晚霞","蝴蝶","光晕","盛开","笑"],"丑陋":["疤","歪","油腻","暗角","扭曲","嫌弃","霉","裂缝","别开眼","腐烂","蛆","暗"],"善良":["让座","弯腰","流浪猫","热汤","伸手","老奶奶","雨伞","记得","温和","救","粥","看顾","心软"],"邪恶":["暗巷","冷笑","钩子","阴狠","面具","贪","影子","刀","诱","黑水","密谋","吞"],"真理":["光","刀","碑","深井","追问","剥","赤裸","尺","镜","辩","骨","醒来"],"正义":["天平","剑","怒","站出","呐喊","法庭","旗","铁","挺身","骨","审判","碑","手","不退"],"权力":["印","台阶","高台","跪","脊背","杖","座位","俯视","印玺","影","墙","网","铁"],"欲望":["火","钩","饥饿","深井","吞","红","蛇","涨","吞没","缠绕","渴","洞","光"],"意义":["追问","灯","碑","为什么","活着","肩","选择","痕","回望","落点","锚","撑","远方"],"存在":["呼吸","脚印","影子","镜","在场","重量","痕","石","回声","被看见","根","夜","证"],"虚无":["雾","回声","灰","坠落","无底","幻","消失","荒","断","空房间","轻","白"],"永恒":["星","石","海","碑","誓言","长夜","循环","不灭","光年","古树","雪线","无终"],"瞬间":["闪光","快门","抬眼","雨滴","火花","心跳","一瞬","照面","坠落","凝固","刹那","亮","呼吸"],"回忆":["旧照片","夏天","校服","老屋","磁带","走神","黄昏","名字","尘","抽屉","雨","慢","回不去"],"乡愁":["月","老屋","炊烟","方言","站台","行李","小河","稻田","归","异乡","船","雪","年"],"青春":["校服","操场","暗恋","单车","晚自习","耳机","夏天","毕业","冲动","痘痘","篮球场","信纸","迷茫","离别","十七岁"],"成长":["摔","疤","背影","肩膀","独自","咬牙","明白","告别","第一次","深夜","扛","长高","沉默"],"告别":["站台","挥手","背影","行李","最后","拥抱","没说完","转身","旧衣","再见","空","风","不回头"],"秩序":["格","队列","线","钟","规章","排列","边界","棋盘","齐","守","层","表","尺","序"],"混沌":["雾","漩涡","乱麻","噪","坍","无序","涌","碎","狂","泥","沸腾","吞","原始"],"因果":["种","链","扣","报","一环","债","花与刺","回声","落子","牵","织","缘","轮","果"],"轮回":["四季","潮","圆","生灭","转","落叶","春","夜昼","归","尘","重复","环","茧","再来"],"无常":["风","云","聚散","花落","骤雨","转瞬","握不住","灯灭","离别","凉","流","夜"],"边界":["线","墙","河","篱","门槛","越","岸","退","半步","尽头","界碑","框"],"沉默":["低垂","抽烟","墙","欲言","抿嘴","空椅","点头","不响","肩","咽下","海","看","夜"],"光明":["晨","窗","灯","雪","白","照","路","亮","暖","敞","峰顶","睁眼","亮堂","破晓"],"黑暗":["夜","深巷","闭眼","冷","无光","沉","影","摸","井","吞","炭","盲","深处","躲","星点"],"遗忘":["尘","空白","旧号码","模糊","删","想不起","褪色","抽屉","淡","走散","灰","轻","河"],"诗歌":["诗人","韵脚","意象","抒情","唐诗","李白","朗诵","分行","隐喻","情感","古典","现代","短小","吟唱"],"词牌":["长短句","平仄","曲调","婉约","江南","酒","愁","拍","吟","柳","婉转","唱"],"散曲":["小令","酒旗","诙谐","市井","叹","俗","元","调","白","笑骂","归隐","乡音"],"绘画":["画笔","颜料","画布","色彩","画家","素描","油画","构图","美术馆","临摹","灵感","线条","展览","表达"],"书法":["毛笔","宣纸","墨","砚台","楷书","行书","王羲之","临帖","笔画","落款","印章","静心","中锋"],"篆刻":["石","刀","印泥","朱红","边款","方寸","冲刀","青田","阴文","阳文","金石","小印","残"],"苏绣":["丝线","针","绷","双面","蚕","劈丝","绣娘","指尖","绢","鸳鸯","软","苏"],"青瓷":["釉","窑","雨过天青","开片","温润","素","瓶","火","泥","冰裂","静","宋","手","影"],"茶道":["沸水","壶","盏","回甘","候","香","敬","慢","汤色","禅","木","静"],"花道":["枝","剪","瓶","留白","斜","器","季节","枯","疏","插","影","雅"],"水墨":["墨","宣","晕","留白","山","烟","皴","黑白","江","竹","湿","意"],"油画":["油彩","画布","厚涂","肖像","静物","调色刀","松节油","暗部","笔触","风景","金黄","画室","层","亮"],"雕塑":["石","凿","泥","铸","肌肉","铜","大理","立","痕","轮廓","基座","凝固"],"摄影":["光圈","快门","ISO","构图","光影","街拍","纪实","焦距","冲洗","瞬间","镜头","取景框","修图","布列松"],"戏剧":["幕","台","灯","妆","台词","冲突","谢幕","换景","独白","戏服","彩排","掌声","悲喜","上场"],"舞蹈":["身体","节奏","舞步","舞台","芭蕾","音乐","旋转","裙摆","编排","观众","汗水","优雅","广场舞","律动"],"旋律":["起伏","哼","记忆","绕","上行","余音","喉","飘逸","重复","扣","雨","歌"],"节奏":["鼓点","心跳","节拍器","快慢","律动","呼吸","舞步","韵律","停顿","说唱","强弱","循环","摇摆"],"和声":["叠","共鸣","层","合唱","厚","柔和","融","色彩","衬托","波","暖","散"],"剪纸":["红纸","剪刀","窗花","纹","对称","镂空","年","兔","鱼","细","贴","碎"],"戏曲":["水袖","髯口","锣鼓","台步","唱腔","脸谱","亮相","戏台","二胡","念白","身段","叫好","胡琴","旦","板"],"汉服":["衣袂","广袖","交领","系带","裙裾","绣","腰带","风","发簪","素","层叠","礼"],"古琴":["弦","七","桐木","泛音","挑","勾","吟","松风","抚","沉","夜","韵"],"园林":["漏窗","假山","廊","竹","月门","曲径","亭","借景","池","苔","移步","幽"],"碑帖":["拓","石","墨","临","残","拓包","拓片","刻","斑驳","古","影","纸"],"壁画":["墙","颜料","飞天","剥落","窟","土","线","斑","供养","幽","彩","岁月"],"陶艺":["泥","轮","窑","釉","火","拉坯","湿","转","裂","烧","器","指尖"],"评弹":["琵琶","三弦","吴语","茶馆","说书","扇","软","调","长篇","客","腔","水乡","夜"],"皮影":["灯","幕","签","影","驴皮","唱","刻","晃","锣","夜","薄","活"],"榫卯":["木","凸","凹","咬","不用钉","老匠","梁","稳","凿","拼","缝","承"],"宣纸":["润","白","纤维","吸墨","帘","软","晕","摊","纸香","薄","韧","千年"],"砚台":["石","墨","磨","池","端","凉","凹","沉","研","痕","包浆","文"],"装裱":["绫","浆","轴","托","边","挂","卷","匠","尺","平","框","古"],"春节":["过年","春联","红包","饺子","鞭炮","团圆","除夕","拜年","春晚","春运","年货","灯笼","守岁","喜庆"],"中秋":["月饼","桂花","团圆","灯笼","赏月","柚子","归家","玉兔","酒杯","嫦娥","桂香","思念","庭院"],"端午":["粽","龙舟","艾","雄黄","屈原","香囊","菖蒲","五彩绳","粽叶","驱邪","河边","午"],"元宵":["汤圆","猜谜","舞龙","花灯","团圆","鞭炮","锣鼓","走百病","纸灯","闹","暖","街"],"清明":["雨","扫墓","柳","纸钱","青团","碑","湿","踏青","香","跪","菊","冷"],"婚礼":["新娘","新郎","戒指","婚纱","誓言","宴席","宾客","教堂","喜庆","鲜花","红包","仪式","祝福","蜜月"],"葬礼":["白花","哀乐","遗像","雨","鞠躬","挽联","亲","土","钟","慢","告别","静"],"生日":["蛋糕","蜡烛","礼物","祝福","年龄","派对","许愿","吹灭","朋友","庆祝","母亲","童年","惊喜","歌声"],"毕业":["学士服","合影","抛帽","离别","操场","留言","散伙饭","行李","车票","老师","泪","拥抱","校门","宿舍","回望"],"聚会":["碰杯","笑闹","老友","火锅","久别","散场","热闹","旧事","合影","聊","酒","夜"],"团圆":["圆桌","热气","笑","灯","归","饭菜","拥抱","久别","热闹","月","围坐","满"],"守岁":["灯","电视","瓜子","鞭炮","围坐","迎新","零点","困","等","年","红","暖"],"灯会":["花灯","谜","人潮","糖画","船灯","桥","挤","闹","光","游","夜","红"],"祭祖":["香","跪","碑","纸","族","雨","鞠躬","烛","念","谱","清","敬","烟"],"寿宴":["寿桃","面","孙","蛋糕","祝","福","合影","热闹","礼","长","聚","暖"],"年夜饭":["鱼","饺子","热气","围坐","春晚","鞭炮","圆桌","家常","母亲","笑","香","守","满"],"红包":["压岁","抢","手指","长辈","拆","好运","厚","递","祝福","点开","群","喜"],"春联":["红纸","墨","门","横批","福","贴","对仗","年","吉","倒","浆糊","街","字"],"月饼":["酥","莲蓉","圆","盒","送","甜","切","桂","五仁","油纸","礼","腻"],"龙舟":["鼓","桨","江","号子","水花","端午","龙头","划","岸","喊","浪","汗","赛","红"],"重阳":["登高","菊","糕","老人","山","秋","茱萸","敬","远望","孝","天","长","暖","寿"],"七夕":["鹊桥","星","针","巧","花","烛","牛郎","红绳","约会","银河","葡萄架","恋","甜"],"妈妈":["母亲","父母","家人","康乃馨","母亲节","陪伴","家庭","汗水","爸爸","父亲","人类","生命","鲜花","怀抱","唠叨","厨房","童年","白发","针线","牵挂","背影"],"水鬼":["游戏","小说","恐惧","吸血鬼","水猴子","传说","故事","手游","动漫","河塘","水草","长发","落水","民间","深夜","禁忌","湿冷","水花","乡野","迷信"],"人马座":["十一月","十二月","木星","火象","自由","乐观","弓箭","半人马","喀戎","远行","哲学","求知","坦率","冬初"],"父亲":["父爱","家庭","孩子","肩膀","严厉","养家","沉默","背影","血缘","父亲节","榜样","责任","白发","教诲"],"母亲":["母爱","家庭","孩子","怀抱","唠叨","厨房","感恩","温柔","血缘","付出","母亲节","衰老","牵挂","摇篮"],"父母":["爸爸","妈妈","养育","恩情","家庭","牵挂","白发","陪伴","孝顺","唠叨","老去","团圆"],"家人":["父母","兄弟姐妹","亲人","饭桌","团圆","陪伴","血缘","老屋","年夜饭","惦记","互相","归属"],"亲情":["血缘","父母","子女","陪伴","牵挂","无条件","团圆","饭桌","守望","传承","眼泪","温暖"],"拜年":["春节","祝福","走亲戚","吉祥话","红包","初一","拱手","串门","热闹","问候","年味","礼数"],"亲戚":["血缘","走动","拜年","长辈","表兄弟","串门","人情","婚丧","聚会","称呼","远房","往来"],"邻居":["隔壁","串门","楼道","借东西","打招呼","小区","和善","守望","噪音","互助","熟人","院墙"],"童年":["玩耍","玩具","学校","父母","糖果","无忧","记忆","暑假","朋友","天真","成长","动画片","哭闹","怀旧"],"摇篮":["婴儿","摇晃","母亲","摇篮曲","襁褓","入睡","木床","轻柔","哼唱","安睡","童年","膝上"],"蜡烛":["火苗","光","生日","许愿","香味","停电","烛光","点燃","融化","蜡油","微光","映照"],"礼物":["包装","惊喜","心意","丝带","拆开","生日","节日","挑选","贺卡","祝福","纪念","赠予"],"拥抱":["温暖","双臂","安慰","重逢","告别","依靠","亲情","贴紧","心跳","鼓励","沉默","眼泪"],"饭菜":["家常","米饭","炒菜","汤","香气","饭桌","热乎","母亲","下厨","饱","剩菜","味道"],"炊烟":["屋顶","晚饭","村庄","柴火","黄昏","升起","归家","烟火气","乡愁","灶台","暮色","袅袅"],"老屋":["瓦","木门","院子","青苔","门槛","记忆","故乡","拆迁","旧物","屋檐","老井","祖辈"],"白羊座":["三月","四月","火星","火象","冲动","直率","开拓","公羊","金羊毛","春分","勇士","热烈","争先","单纯"],"金牛座":["四月","五月","金星","土象","稳重","固执","公牛","宙斯","欧罗巴","绿松石","春末","美食","享受","耐心"],"双子座":["五月","六月","水星","风象","好奇","机敏","双胞胎","卡斯托尔","波吕克斯","沟通","变化","灵活","夏初","学习"],"巨蟹座":["六月","七月","月亮","水象","温柔","顾家","敏感","螃蟹","赫拉","珍珠","母性","守护","夏至","怀旧"],"狮子座":["七月","八月","太阳","火象","自信","慷慨","王者","狮子","尼米亚","赫拉克勒斯","黄金","表演","领导","夏末"],"处女座":["八月","九月","水星","土象","完美主义","细致","麦穗","雅典娜","分析","挑剔","秩序","收获","服务","谦逊"],"天秤座":["九月","十月","金星","风象","平衡","公正","秤杆","忒弥斯","和谐","审美","犹豫","秋分","社交","对称"],"天蝎座":["十月","十一月","冥王星","水象","深刻","专注","蝎子","猎户","神秘","意志","蜕变","深秋","执念","洞察"],"射手座":["十一月","十二月","木星","火象","自由","乐观","弓箭","半人马","喀戎","远行","哲学","求知","坦率","冬初"],"摩羯座":["十二月","一月","土星","土象","务实","坚韧","山羊","潘神","攀登","责任","隐忍","冬至","事业","规划"],"水瓶座":["一月","二月","天王星","风象","自主","创新","水瓶","伽倪墨得斯","科技","博爱","前卫","立春","理性","朋友"],"双鱼座":["二月","三月","海王星","水象","浪漫","直觉","双鱼","阿佛洛狄忒","梦幻","慈悲","艺术","雨水","包容","同情"],"仙女座":["安德洛墨达","秋季","螺旋星系","珀尔修斯","北天","银河","仙后之女"],"唧筒座":["南天","气泵","暗淡","细小","拉卡伊","春季"],"天燕座":["南天","天堂鸟","极南","新几内亚","小星座"],"天鹰座":["天鹰","牛郎星","银河","夏季","宙斯","飞鹰"],"天坛座":["南天","祭坛","银河","半人马","极南"],"御夫座":["御夫","五车二","冬季","山羊","车夫","银河"],"牧夫座":["牧夫","大角星","春季","北斗","农夫","北天"],"雕具座":["南天","凿子","雕刻","暗淡","拉卡伊"],"鹿豹座":["长颈鹿","北天","暗淡","极北","春季"],"猎犬座":["猎犬","北天","春季","查理之心","漩涡星系","牧夫"],"大犬座":["天狼星","冬季","猎户","南天","最亮","大狗"],"小犬座":["小犬","南河三","冬季","猎户","双子","双星"],"船底座":["船底","老人星","南天","南船","银河","星云"],"仙后座":["仙后","北方","王字","秋季","银河","埃塞俄比亚"],"半人马座":["半人马","南天","比邻星","春季","银河","喀戎"],"仙王座":["仙王","北天","秋季","王族","造父变星"],"鲸鱼座":["鲸鱼","秋季","米拉变星","海怪","南天","双鱼"],"蝘蜓座":["南天","变色龙","极南","暗淡","拉卡伊"],"圆规座":["南天","圆规","半人马","暗淡","拉卡伊"],"天鸽座":["天鸽","南天","诺亚","洪水","冬季"],"后发座":["后发","春季","北天","头发","星系团","贝勒尼基"],"南冕座":["南冕","南天","冠冕","射手","暗淡"],"北冕座":["北冕","冠冕","春季","北天","珍珠","王冠"],"乌鸦座":["乌鸦","春季","阿波罗","四边形","南天"],"巨爵座":["巨爵","酒杯","春季","阿波罗","长蛇"],"南十字座":["南天","十字","导航","船底座","银河","澳大利亚","南极","四颗星"],"天鹅座":["天鹅","夏季","银河","北十字","天津四","宙斯"],"海豚座":["海豚","夏季","北天","波塞冬","小星座","四边形"],"剑鱼座":["南天","剑鱼","大麦哲伦","极南","拉卡伊"],"天龙座":["天龙","北天","巨龙","极北","长蛇","赫斯珀里得斯"],"小马座":["小马","北天","秋季","海豚","小星座","马头"],"波江座":["波江","河流","冬季","南天","长河","蜿蜒"],"天炉座":["南天","熔炉","秋季","拉卡伊","星系"],"天鹤座":["南天","天鹤","秋季","长颈","极南"],"武仙座":["武仙","夏季","赫拉克勒斯","北天","大力士","球状星团"],"时钟座":["南天","时钟","摆钟","波江","暗淡","拉卡伊"],"长蛇座":["长蛇","春季","九头蛇","巨爵","乌鸦","最长"],"水蛇座":["南天","水蛇","极南","暗淡","麦哲伦"],"印第安座":["南天","印第安","极南","拉卡伊","暗淡"],"蝎虎座":["北天","蝎虎","蜥蜴","秋季","暗淡"],"小狮座":["小狮","北天","春季","狮子","暗淡"],"天兔座":["天兔","冬季","猎户","兔子","南天"],"豺狼座":["南天","豺狼","春季","半人马","天蝎"],"天猫座":["北天","天猫","猞猁","春季","暗淡"],"天琴座":["天琴","织女星","夏季","银河","俄耳甫斯","竖琴"],"山案座":["南天","桌山","极南","大麦哲伦","暗淡"],"显微镜座":["南天","显微镜","秋季","拉卡伊","暗淡"],"麒麟座":["麒麟","冬季","猎户","独角兽","银河","星云"],"苍蝇座":["南天","苍蝇","南十字","极南","蜜蜂"],"矩尺座":["南天","矩尺","直角尺","天蝎","银河"],"南极座":["南天","南极","极南","拱极","暗淡"],"蛇夫座":["蛇夫","夏季","黄道","医神","巨蛇","银河"],"猎户座":["猎户","冬季","参宿四","腰带","三星","猎户星云","俄里翁","天蝎"],"孔雀座":["南天","孔雀","极南","秋季","羽屏"],"天马座":["飞马","希腊","神话","珀尔修斯","秋夜","四边形","星团","翅膀"],"英仙座":["英仙","珀尔修斯","秋季","流星雨","北天","美杜莎"],"凤凰座":["南天","凤凰","不死鸟","秋季","火鸟","拉卡伊"],"绘架座":["南天","画架","画笔","拉卡伊","暗淡"],"南鱼座":["南天","北落师门","秋季","鱼嘴","水瓶"],"船尾座":["南天","船尾","南船","银河","星团"],"罗盘座":["南天","罗盘","指南针","南船","暗淡"],"网罟座":["南天","网罟","网格","拉卡伊","暗淡"],"天箭座":["北天","天箭","夏季","小星座","银河"],"玉夫座":["南天","玉夫","雕刻","秋季","拉卡伊","星系"],"盾牌座":["南天","盾牌","夏季","银河","星云","索别斯基"],"巨蛇座":["蛇身","夏季","蛇夫","北天","银河","两段"],"六分仪座":["南天","六分仪","春季","长蛇","暗淡"],"望远镜座":["南天","望远镜","极南","拉卡伊","暗淡","观星"],"三角座":["北天","三角","秋季","白羊","仙女"],"南三角座":["南天","三角","极南","银河","南十字"],"杜鹃座":["南天","杜鹃","极南","星团","麦哲伦"],"大熊座":["北斗","北天","春季","大熊","斗柄","摇光","星系"],"小熊座":["小熊","北极星","北天","拱极","小北斗","导航"],"船帆座":["南天","船帆","南船","银河","星团"],"飞鱼座":["南天","飞鱼","极南","拉卡伊","暗淡"],"狐狸座":["北天","狐狸","夏季","天箭","暗淡","星云"],"天文台":["观测","圆顶","望远镜","星空","紫金山","科研","守夜","站址"],"黄道":["太阳","轨迹","十二宫","天球","公转","白道","黄道面"],"北斗七星":["大熊","斗柄","天枢","摇光","指北","勺形","北极星"],"太阳系":["太阳","八大行星","地球","火星","木星","土星","轨道","奥尔特云"],"二十四节气":["立春","惊蛰","清明","夏至","秋分","冬至","农事","物候","黄经","历法"],"星团":["昴星团","球状","疏散","密集","银河","引力","毕星团"],"星图":["坐标","方位","星座","天球","标注","观测","纸质"],"观星":["夜空","银河","流星","望远镜","露营","光害","躺椅"],"月食":["满月","地球","阴影","红月","天象","本影","半影"],"日珥":["太阳","火焰","边缘","磁场","色球","喷发"],"耀斑":["太阳","爆发","磁暴","辐射","黑子","短波"],"超新星":["爆发","恒星","遗迹","亮度","蟹状","中子星"],"中子星":["致密","脉冲","引力","超新星","磁极","旋转"],"白矮星":["恒星","冷却","致密","天狼","晚年","暗淡"],"红巨星":["年老","膨胀","恒星","参宿四","核心","氦核"],"脉冲星":["中子星","射电","周期","灯塔","旋转","信号"],"星轨":["长曝光","旋转","北极星","圆形","夜空","相机"],"天文馆":["球幕","投影","科普","星空","座椅","讲解"],"星系团":["星系","引力","后发","尺度","暗物质","聚集"],"光年":["距离","光速","尺度","星际","衡量","遥远"],"星尘":["尘埃","星际","云气","凝聚","微光","弥漫"],"火箭":["发射","推进","轨道","卫星","燃料","发射场","整流罩"],"空间站":["轨道","失重","舱段","对接","航天员","实验"],"奥丁":["众神之父","独眼老人","长矛","渡鸦","八足马","智慧之泉","诗艺","预言","王座","流浪者","阿斯加德","献祭"],"索尔":["雷神","雷神之锤","妙尔尼尔","红发","力量","雷电","巨人","战车","山羊","铁手套","力量腰带","守护"],"洛基":["诡计之神","变形","恶作剧","火","混血","芬里尔之父","蛇影","背叛","锁链","诸神黄昏","口才","狡诈"],"芙蕾雅":["爱与美","金项链","猫车","战阵","女武神之首","华纳神族","魔法","野猪","珠宝","春花"],"海姆达尔":["守门人","彩虹桥","号角","警觉","听觉","昼夜","诸神黄昏","号声","边界","守望"],"提尔":["战神","独手","誓言","勇气","芬里尔","契约","正义","决斗"],"巴德尔":["光明之神","美与善","槲寄生","盲神","葬礼","复活","春天","不幸","预兆"],"芬里尔":["巨狼","芬里尔狼","锁链","诸神黄昏","吞噬","奥丁","格莱普尼尔","咆哮","挣脱"],"耶梦加得":["巨蛇","尘世巨蟒","环抱世界","深海","腐蚀","索尔","诸神黄昏","海","首尾相衔"],"尼德霍格":["暗龙","世界树之根","啃噬","黑暗","深渊","地底","黑龙","毁灭"],"瓦尔基里":["女武神","战场","挑选","英灵","飞马","铠甲","长矛","盾牌","战歌","命运"],"女武神":["瓦尔基里","战场","英灵殿","铠甲","飞马","长矛","羽衣","挑选","战歌","守护"],"英灵殿":["瓦尔哈拉","奥丁","战死","宴饮","女武神","长矛","盾牌","每日战斗","复活","大厅"],"诸神黄昏":["末日","大战","芬里尔","耶梦加得","苏尔特尔","烈焰","沉没","新生","预言","寒冬"],"世界树":["尤克特拉希尔","九界","树根","枝干","松鼠","老鹰","尼德霍格","生命","庇护","连接"],"阿斯加德":["神域","奥丁","金宫","彩虹桥","英灵殿","城墙","九界","众神","宫殿","天空"],"约顿海姆":["巨人国","霜巨人","山巨人","荒野","岩石","洛基","边界","九界","寒冷","洞穴"],"符文":["卢恩","刻痕","字母","占卜","魔法","石碑","神秘","北欧","铭文","护符"],"矮人":["工匠","锻造","矿洞","巧手","金属","珠宝","地下","妙尔尼尔","巧匠","石头"],"精灵":["光明精灵","森林","轻盈","弓箭","美丽","自然","树梢","月光","歌声","隐身"],"霜巨人":["巨人","冰霜","寒冬","约顿海姆","冰雪","力量","远古","山脉","寒冷","敌意"],"彩虹桥":["比弗罗斯特","彩虹","桥","阿斯加德","海姆达尔","九界","通道","光","守护"],"妙尔尼尔":["雷神之锤","索尔","闪电","短柄","力量","矮人打造","守护","神器","雷霆","回归"],"雷神之锤":["妙尔尼尔","索尔","闪电","雷霆","短柄","矮人","神器","守护","力量","锤击"],"宙斯":["众神之王","奥林匹斯","雷霆","神盾","鹰","公牛","赫拉","权力","天空","裁决"],"赫拉":["天后","婚姻","家庭","孔雀","妒忌","奥林匹斯","王后","石榴","婚礼","守护"],"波塞冬":["海神","三叉戟","海洋","马","地震","宙斯","哈迪斯","三兄弟","海浪","水手"],"哈迪斯":["冥王","冥界","宙斯","波塞冬","三兄弟","珀耳塞福涅","黑帝","地下","审判","刻耳柏洛斯","隐身帽","财富"],"雅典娜":["智慧","战争","猫头鹰","橄榄树","盾牌","雅典","织布","策略","铠甲","处女神"],"阿波罗":["太阳神","光明","音乐","竖琴","弓箭","预言","德尔斐","医神","月桂","艺术"],"阿尔忒弥斯":["月亮","狩猎","弓箭","森林","鹿","处女神","夜","野兽","阿波罗","少女"],"阿瑞斯":["战神","战争","长矛","盾牌","铠甲","斯巴达","勇猛","争斗","奥林匹斯","野猪"],"赫菲斯托斯":["火神","工匠","锻造","铁匠","火山","熔炉","巧手","机械","阿佛洛狄忒","轮椅"],"赫尔墨斯":["信使","飞鞋","商旅","边界","双蛇杖","冥界引路","口才","旅行","机敏","羽帽"],"阿佛洛狄忒":["爱与美","玫瑰","贝壳","鸽子","特洛伊","赫菲斯托斯","金苹果","魅力","海洋","青春"],"得墨忒尔":["丰收","农业","谷物","季节","珀耳塞福涅","大地","麦穗","母神","冬天","农耕"],"狄俄尼索斯":["酒神","葡萄","葡萄酒","狂欢","戏剧","藤蔓","豹","剧场","宴会","陶醉"],"珀尔修斯":["英雄","美杜莎","盾牌","飞鞋","安德洛墨达","海怪","宙斯之子","隐身帽","铜镜","归途"],"赫拉克勒斯":["大力士","十二功业","狮子","九头蛇","宙斯之子","力量","木棒","涅墨亚","英雄","升天"],"忒修斯":["英雄","迷宫","米诺陶","线团","雅典","阿里阿德涅","帆船","公牛","克里特","王子"],"美杜莎":["蛇发","石化","目光","戈耳工","珀尔修斯","海神","姐妹","盾牌","诅咒","传说"],"米诺陶":["牛头人身","迷宫","克里特","忒修斯","线团","公牛","代达罗斯","地下","王宫","路径"],"奥林匹斯":["众神","圣山","宙斯","十二主神","宫殿","云端","神域","希腊","王座","聚会"],"冥界":["哈迪斯","冥河","卡戎","渡船","刻耳柏洛斯","亡魂","审判","珀耳塞福涅","地下","遗忘"],"泰坦":["巨人神","克洛诺斯","宙斯","远古","神族","乌拉诺斯","盖亚","战争","囚禁","力量"],"潘多拉":["第一个女人","盒子","好奇","宙斯","普罗米修斯","希望","封印","人间","礼物","开启"],"拉":["太阳神","太阳船","鹰头","正午","冥界之旅","阿图姆","创世","光明","法老","夜行"],"奥西里斯":["冥王","复活","农业","伊西斯","荷鲁斯","赛特","木乃伊","审判","尼罗河","绿洲"],"伊西斯":["魔法","母神","奥西里斯","荷鲁斯","翅膀","咒语","复活","忠贞","王座","医治"],"荷鲁斯":["鹰神","法老","王权","奥西里斯","伊西斯","赛特","眼睛","天空","守护","王冠"],"阿努比斯":["死神","胡狼头","天平","心脏","称量","木乃伊","防腐","亡灵书","引路","冥界"],"赛特":["沙漠之神","混乱","风暴","奥西里斯","荷鲁斯","争斗","荒野","力量","兄弟","荒漠"],"托特":["智慧","书写","月亮","朱鹭","狒狒","记录","审判","文字","天平","学者"],"巴斯特":["猫神","家庭","音乐","舞蹈","守护","月亮","温柔","贝斯特","欢愉","猫"],"金字塔":["法老","陵墓","吉萨","石砌","三角","沙漠","胡夫","天文","守护","奇迹"],"木乃伊":["防腐","亚麻","香料","石棺","复活","亡灵书","仪式","裹布","冥界","保存"],"亡灵书":["咒语","纸草","冥界","审判","引路","称量","阿努比斯","护身","来世","经文"],"尼罗河":["泛滥","绿洲","埃及","灌溉","船","鳄鱼","纸草","丰饶","三角洲","生命"],"女娲":["造人","补天","蛇身","泥土","五彩石","伏羲","创世","母神","洪水","抟土"],"伏羲":["八卦","人文始祖","蛇身","结绳","渔猎","女娲","古琴","历法","龙","占卜"],"盘古":["开天辟地","巨斧","混沌","天地","巨人","化生","日月","山川","创世","呼吸"],"后羿":["射日","弓箭","太阳","嫦娥","英雄","九日","西王母","不死药","神射","传说"],"嫦娥":["月宫","玉兔","后羿","奔月","桂树","广寒宫","仙子","月圆","中秋","孤寂"],"夸父":["逐日","巨人","桃林","奔跑","太阳","口渴","渭水","执着","木杖","大地"],"精卫":["填海","鸟","衔石","炎帝","东海","执着","羽翼","不屈","木石","传说"],"刑天":["无头巨人","干戚","战神","炎帝","不屈","舞戚","常羊山","勇猛","神话","躯体"],"黄帝":["轩辕","人文初祖","蚩尤","涿鹿","部落","龙","中医","指南车","炎帝","始祖"],"炎帝":["神农","尝百草","农业","火","黄帝","部落","医药","五谷","集市","始祖"],"蚩尤":["九黎","战神","黄帝","涿鹿","铜头","兄弟","兵主","大雾","牛角","勇武"],"龙王":["四海","龙宫","降雨","水晶宫","龙珠","虾兵","海","水族","巡海","龙女"],"凤凰":["不死鸟","涅槃","火","百鸟之王","羽翼","瑞鸟","梧桐","重生","吉祥","翱翔"],"麒麟":["瑞兽","仁兽","独角","祥瑞","龙鳞","鹿身","太平","蹄","吉祥","圣人"],"饕餮":["贪食","怪兽","青铜","纹样","四凶","巨口","山海经","贪欲","图腾","凶兽"],"山海经":["上古","异兽","地理","神话","九尾狐","鲲鹏","记录","山海","奇书","传说"],"昆仑":["神山","西王母","仙境","瑶池","玉","雪峰","天梯","神话","道士","琼浆"],"蓬莱":["仙岛","海市","仙人","长生","方丈","瀛洲","云烟","东海","仙境","渡船"],"月宫":["嫦娥","玉兔","桂树","广寒宫","吴刚","月圆","清冷","银辉","中秋","仙子"],"天庭":["玉帝","神仙","凌霄殿","天宫","蟠桃","天兵","仙气","朝会","云","南天门"],"玉帝":["天帝","天庭","凌霄殿","众神","蟠桃","金阙","天宫","统御","仙班","朝会"],"钟馗":["捉鬼","门神","判官","画像","端午","守护","黑面","宝剑","传说","镇宅"],"八仙":["过海","铁拐李","吕洞宾","何仙姑","张果老","韩湘子","蓝采和","曹国舅","汉钟离","仙术"],"梵天":["创造之神","四面","莲花","毗湿奴","湿婆","三相神","宇宙","经文","天鹅","冥想"],"毗湿奴":["守护","十化身","鱼","龟","野猪","罗摩","黑天","蛇床","海洋","平衡"],"湿婆":["毁灭","舞蹈","三叉戟","第三只眼","雪山","蛇","恒河","苦行","林伽","重生"],"因陀罗":["雷神","金刚杵","天界","雨","战神","阿修罗","白象","云端","雷电","王者"],"迦楼罗":["金翅鸟","蛇","毗湿奴","坐骑","羽翼","巨鸟","蛇敌","飞翔","力量","传说"],"那伽":["蛇神","蛇冠","地下","水","守护","宝石","龙族","缠绕","水池","传说"],"阿修罗":["魔族","天界","争斗","因陀罗","力量","苦行","苏摩","战场","对立","传说"],"须弥山":["神山","世界中心","日月","四大洲","天界","轴心","山海","宇宙","传说","高峰"],"天照大神":["太阳女神","高天原","天岩户","八咫镜","伊势神宫","光明","织造","须佐之男","稻穗","神乐"],"须佐之男":["风暴之神","八岐大蛇","天丛云剑","高天原","天照大神","出云","草薙剑","斩蛇","放逐","勇武"],"伊邪那岐":["创世","伊邪那美","黄泉","禊","天沼矛","岛屿","夫妻","诞生","神话","淤能碁吕"],"八岐大蛇":["八头","八尾","须佐之男","酒","草薙剑","出云","巨蛇","退治","肥河","传说"],"稻荷神":["稻荷","狐狸","丰收","鸟居","伏见","稻米","商业","使者","红色","参道"],"鬼":["鬼怪","角","虎纹","铁棒","桃太郎","豆","驱鬼","传说","妖怪","夜"],"妖怪":["百鬼","天狗","河童","狐","狸","传说","夜行","怪谈","山","变化"],"河童":["水妖","河","头顶盘","黄瓜","相扑","游泳","传说","绿色","小孩","沼泽"],"天狗":["山","长鼻","羽扇","修行","鞍马","飞行","红脸","传说","山林","高傲"],"神社":["鸟居","参道","御守","绘马","神乐","拜殿","石灯笼","杉树","净化","祭礼"],"德鲁伊":["祭司","橡树","槲寄生","预言","自然","仪式","凯尔特","智慧","林间","石阵"],"亚瑟王":["圆桌骑士","石中剑","卡美洛","梅林","湖中仙女","圣杯","不列颠","王者","桂妮薇","传说"],"梅林":["巫师","预言","亚瑟王","魔法","洞窟","智者","变身","指引","橡树","传说"],"湖中仙女":["湖","剑","亚瑟王","赐剑","水面","仙女","手臂","祝福","传说","雾气"],"石中剑":["剑","石头","亚瑟王","拔出","王权","传说","剑柄","铭文","不列颠","选王"],"圣杯":["杯","寻找","圆桌骑士","亚瑟王","神圣","传说","光","骑士","追寻","奇迹"],"巨龙":["龙","喷火","鳞甲","宝藏","洞穴","威尔士","红龙","传说","翼","守护"],"杰洛特":["猎魔人","白狼","希里","叶奈法","特莉丝","狼学派","银剑","钢剑","巫师","萝卜","凯尔莫罕","猎魔"],"叶奈法":["女术士","杰洛特","黑曜石之星","温格堡","魔法","希里","丁香与醋栗","紫色","迪精","范格堡"],"胡萝卜":["橙色","蔬菜","维生素","兔子","地里","炖汤","根茎","沙拉","种植","甘甜","视力","胡萝卜素"],"王者荣耀":["腾讯","手游","英雄","排位","法师","射手","打野","辅助","坦克","五杀","段位","峡谷","李白","妲己"],"传奇":["网游","玛法大陆","战士","法师","道士","攻沙","行会","怀旧","私服","网吧","升级","屠龙刀"],"英雄联盟":["拳头游戏","召唤师","峡谷","德玛西亚","盖伦","排位","团战","下路","符文之地","祖安","打野","上单"],"原神":["米哈游","提瓦特","风神","岩神","元素","祈愿","蒙德","璃月","稻妻","冒险","开放世界","七国"],"我的世界":["沙盒","方块","生存","创造","红石","苦力怕","挖矿","建造","村民","模组","联机","像素"],"塞尔达":["林克","海拉鲁","三角力量","旷野之息","王国之泪","任天堂","解谜","开放世界","大师剑","神庙","冒险","公主"],"黑神话":["悟空","天命人","西游","国产","单机","动作","妖怪","金箍棒","神话","取经"],"艾尔登法环":["魂系","交接地","黄金树","褪色者","开放世界","挑战","魔法","骑士","骨灰","结局"],"最终幻想":["角色扮演","召唤兽","水晶","陆行鸟","剧情","回合","剑与魔法","重制","幻想","日式"],"魔兽世界":["暴雪","艾泽拉斯","联盟","部落","副本","团队","怀旧服","法师","圣骑士","兽人","熊猫人","网游"],"星际争霸":["即时战略","人族","虫族","神族","基地","运营","微操","对战","暴雪","竞技"],"暗黑破坏神":["暴雪","刷装备","地牢","野蛮人","死灵法师","词条","刷图","暗黑","割草","动作角色扮演"],"植物大战僵尸":["豌豆射手","向日葵","坚果","僵尸","塔防","草坪","阳光","关卡","花园","休闲"],"俄罗斯方块":["方块","消除","堆叠","掌机","休闲","高分","旋转","游戏机","经典","节奏"],"魂斗罗":["红白机","横版","射击","双人","通关","经典","八位","关卡","像素","街机"],"超级玛丽":["马里奥","水管","蘑菇","跳跃","金币","关卡","任天堂","像素","横版","公主"],"吃鸡":["大逃杀","跳伞","空投","决赛圈","平底锅","组队","射击","战术","手游","生存"],"斗地主":["扑克","地主","记牌","出牌","顺子","对子","牌局","农民","三人","牌桌"],"五子棋":["棋盘","黑白","连成","对弈","五连","落子","方格","益智","先手","布局"],"电竞":["战队","选手","比赛","训练","解说","赛场","冠军","直播","键盘","竞技"],"主机":["游戏机","家用","电视","手柄","独占","索尼","任天堂","性能","客厅","开机"],"手柄":["摇杆","按键","振动","握把","连线","蓝牙","游戏","舒适","操作","充电"],"掌机":["便携","任天堂","屏幕","口袋","续航","卡带","随身","按键","掌上","游戏"],"街机":["投币","摇杆","按钮","像素","怀旧","游戏厅","格斗","拳皇","排队","热闹"],"海贼王":["路飞","草帽","橡胶","航海","伙伴","大秘宝","恶魔果实","索隆","乔巴","船长","伟大航路","冒险"],"火影忍者":["鸣人","忍者","查克拉","木叶","佐助","小樱","卡卡西","影分身","螺旋丸","羁绊","忍术","村子"],"名侦探柯南":["柯南","侦探","真相","案件","推理","毛利","小兰","黑衣组织","变声蝴蝶结","线索","悬疑","证据"],"哆啦A梦":["机器猫","大雄","静香","胖虎","小夫","四次元口袋","竹蜻蜓","任意门","铜锣烧","时光机","童年","蓝胖子"],"宫崎骏":["吉卜力","动画","龙猫","千与千寻","天空之城","哈尔","风之谷","手绘","治愈","久石让","夏天","飞行"],"千与千寻":["千寻","白龙","汤屋","无脸男","神隐","魔法","名字","河神","宫崎骏","成长"],"龙猫":["宫崎骏","吉卜力","森林","雨伞","猫巴士","姐妹","乡村","治愈","童年","胖乎乎"],"灌篮高手":["樱木花道","流川枫","篮球","湘北","赤木","三井","宫城","全国大赛","青春","热血"],"进击的巨人":["巨人","围墙","调查兵团","自由","艾伦","三笠","阿尔敏","立体机动","悬念","战斗"],"蜡笔小新":["小新","野原","幼稚园","动感超人","青椒","爸爸","妈妈","妹妹","搞笑","日常"],"迪士尼":["米老鼠","唐老鸭","公主","动画","乐园","城堡","童话","白雪公主","狮子王","玩具总动员","烟花","童年"],"漫威":["复仇者联盟","超级英雄","钢铁侠","蜘蛛侠","美国队长","雷神","绿巨人","彩蛋","宇宙","漫画","战衣","反派"],"蜘蛛侠":["彼得帕克","吐丝","纽约","攀爬","英雄","战衣","少年","面具","责任","都市"],"钢铁侠":["托尼","盔甲","方舟反应堆","飞行","富豪","科技","天才","掌心炮","牺牲","马克"],"星球大战":["绝地武士","光剑","原力","银河","帝国","反抗军","机器人","太空","黑武士","星战"],"哈利波特":["霍格沃茨","魔法","巫师","伏地魔","哈利","赫敏","罗恩","魔杖","魁地奇","分院帽","禁林","咒语"],"魔戒":["指环王","中土","佛罗多","甘道夫","精灵","矮人","至尊戒","远征队","夏尔","史诗"],"权游":["冰与火之歌","维斯特洛","铁王座","龙","家族","凛冬","骑士","王国","权谋","长城"],"西游记":["孙悟空","唐僧","猪八戒","沙僧","取经","妖怪","金箍棒","花果山","天庭","八十一难"],"希里":["猎魔人","杰洛特","叶奈法","上古之血","燕子","时空","凯尔莫罕","剑术","命运","白发"],"巫师":["猎魔人","杰洛特","银剑","法印","魔药","怪物","委托","凯尔莫罕","白狼","中世纪"],"只狼":["忍者","弹反","苇名","龙胤","战国","挑战","魂系","剑术","复仇","刀"],"赛博朋克":["未来","霓虹","机械","改造","都市","科技","夜之城","黑客","义体","反乌托邦"],"刺客信条":["刺客","信仰之跃","袖剑","历史","潜行","开放世界","兄弟会","屋顶","任务","古代"],"古墓丽影":["劳拉","探险","古墓","遗迹","解谜","攀爬","丛林","宝藏","弓箭","冒险"],"宝可梦":["皮卡丘","训练家","精灵球","进化","属性","道馆","收集","对战","伙伴","动画"],"奥特曼":["变身","怪兽","地球","防卫队","巨大","英雄","童年","能量灯","特摄","光"],"变形金刚":["汽车人","霸天虎","机器人","变形","塞伯坦","擎天柱","大黄蜂","战斗","科幻","玩具"],"数码宝贝":["亚古兽","进化","数码世界","搭档","冒险","徽章","童年","动画","勇气","伙伴"],"圣斗士":["星矢","雅典娜","黄金圣衣","小宇宙","十二宫","青铜","热血","星座","银河","战斗"],"樱桃小丸子":["小丸子","樱桃","小学","家庭","日常","搞笑","童年","姐姐","爷爷","清水"],"三体":["刘慈欣","科幻","宇宙","外星","黑暗森林","面壁者","飞船","文明","危机","硬科幻"],"流浪地球":["科幻","地球","发动机","太空","危机","家园","行星","牺牲","刘慈欣","未来"],"黑客帝国":["矩阵","虚拟世界","代码","尼奥","胶囊","机器","觉醒","科幻","绿色代码","反抗"],"加勒比海盗":["杰克","海盗船","宝藏","大海","船长","诅咒","冒险","航海","骷髅","风趣"],"番剧":["追番","更新","季度","弹幕","动画","声优","剧情","新番","完结","字幕"],"国漫":["国产动画","国风","连载","画风","剧情","神话","漫画","崛起","观众","改编"],"声优":["配音","声音","角色","演技","录音","动画","广播剧","台词","嗓音","幕后"],"单机游戏":["剧情","离线","通关","存档","开放世界","动作","角色扮演","沉浸","结局","耐玩"],"独立游戏":["小团队","创意","像素","玩法","艺术","小众","口碑","开发","叙事","试玩"],"模拟经营":["建造","经营","规划","资源","城市","农场","策略","管理","布局","时间"],"泰坦尼克号":["沉船","冰山","爱情","海洋","经典","电影","灾难","杰克","露丝","悲壮"],"阿凡达":["潘多拉","蓝色","外星","纳美","飞行","森林","科幻","电影","立体","自然"],"侏罗纪公园":["恐龙","公园","琥珀","岛屿","基因","冒险","电影","霸王龙","逃逸","科普"],"松弛感":["穿搭","心态","从容","随性","生活","不焦虑","氛围","自在","周末","咖啡","慢节奏","治愈"],"班味":["上班","通勤","疲惫","打工","加班","工位","抱怨","周末","下班","职场","倦怠","格子间"],"精神内耗":["焦虑","纠结","想太多","自责","失眠","压力","消耗","情绪","躺平","自我怀疑","疲惫","治愈"],"情绪价值":["陪伴","安慰","共情","倾听","恋爱","朋友","温暖","关心","沟通","关系","支持","理解"],"破防":["感动","崩溃","泪目","心酸","瞬间","共鸣","防线","难过","回忆","戳中","忍不住","情绪"],"躺平":["摆烂","放弃","内卷","佛系","低欲望","休息","沙发","无力","妥协","轻松","节奏","年轻人"],"内卷":["竞争","加班","考试","排名","焦虑","资源","消耗","学生","职场","比拼","疲惫","效率"],"搭子":["饭搭子","球搭子","旅游搭子","健身搭子","朋友","陪伴","兴趣","临时","社交","周末","约饭","散步"],"发疯文学":["段子","吐槽","夸张","排比","情绪","文案","幽默","网络","评论","宣泄","搞笑","模仿"],"早八人":["学生","上课","早起","闹钟","通勤","早餐","困倦","宿舍","教室","排队","咖啡","公交"],"治愈":["温暖","安慰","疗愈","音乐","阳光","猫咪","风景","放松","柔软","陪伴","安心","慢生活"],"解压":["放松","捏捏","泡沫纸","运动","音乐","散步","呼吸","玩具","舒缓","压力","睡前","按摩"],"多巴胺":["穿搭","亮色","快乐","情绪","配色","时尚","治愈","明快","生活","热度","鲜艳","活力"],"美拉德":["棕色","穿搭","秋冬","焦糖","配色","时尚","温暖","大衣","氛围","大地色","质感","复古"],"白开水妆":["妆容","清透","底妆","裸妆","自然","素颜","护肤","淡妆","干净","日常","光泽","简洁"],"情绪稳定":["冷静","理性","成熟","沟通","心态","从容","边界感","自洽","低落","调节","平和","可靠"],"citywalk":["散步","城市","街巷","路线","周末","拍照","咖啡","漫游","老城区","小店","步行","松弛"],"露营":["帐篷","天幕","营地","篝火","野餐","户外","星空","折叠椅","烧烤","周末","自然","装备"],"飞盘":["户外","运动","草坪","接盘","周末","社交","奔跑","俱乐部","投掷","轻运动","公园","朋友"],"围炉煮茶":["炭火","茶壶","冬天","橘子","红薯","聊天","院子","慢生活","朋友","暖意","陶壶","周末"],"预制菜":["速食","冷链","加热","外卖","餐厅","包装","保质期","方便","半成品","厨房","口味","争议"],"拼单":["优惠","团购","凑单","外卖","省钱","下单","满减","奶茶","分享","便宜","订单","团购群"],"平替":["替代","性价比","便宜","大牌","同款","省钱","选择","对比","化妆","国货","实用","预算"],"反向消费":["省钱","理性","性价比","折扣","二手","极简","比价","年轻人","实用","预算","观念","冷静"],"极简":["断舍离","留白","简约","收纳","克制","少而精","设计","生活","清单","清爽","秩序","舍弃"],"断舍离":["整理","丢弃","收纳","衣柜","清理","极简","放下","旧物","分类","清爽","习惯","空间"],"收纳":["整理","盒子","抽屉","衣柜","标签","分类","空间","整齐","挂钩","厨房","杂物","习惯"],"骑行":["自行车","公路","头盔","骑行服","绿道","踏频","通勤","周末","风景","体力","车灯","路线"],"徒步":["登山","背包","路线","户外","山野","水壶","徒步鞋","体力","风景","队伍","营地","清晨"],"出片":["拍照","构图","光线","滤镜","机位","背景","姿势","摄影","朋友圈","穿搭","角度","修图"],"氛围感":["灯光","音乐","布景","照片","蜡烛","咖啡","香薰","慢节奏","温柔","布置","色调","情绪"],"探店":["餐厅","美食","测评","小店","咖啡","菜单","推荐","试吃","拍照","排队","口碑","街巷"],"盲盒":["抽盒","隐藏款","手办","惊喜","收藏","系列","拆盒","玩具","概率","二手","交换","期待"],"谷子":["动漫","周边","徽章","亚克力","收藏","痛包","展台","交换","限量","二次元","摊位","粉丝"],"二次元":["动漫","漫画","手办","漫展","周边","谷子","同人","声优","番剧","画风","粉丝","角色"],"漫展":["角色扮演","摊位","同人","周边","签售","排队","门票","摄影","舞台","粉丝","场馆","谷子"],"手办":["模型","收藏","树脂","摆件","展示柜","涂装","限量","预订","二次元","细节","包装","玩家"],"快手":["短视频","直播","老铁","乡村","带货","双击","关注","评论","主播","接地气","段子","热门"],"带货":["直播","主播","销量","商品","口播","优惠","链接","下单","选品","粉丝","转化","佣金"],"网红":["探店","流量","粉丝","拍照","知名度","争议","账号","爆红","人设","商业","镜头","打卡"],"爆款":["销量","热门","推荐","爆红","话题","模仿","商品","算法","跟风","标题","转发","热度"],"热搜":["话题","榜单","讨论","争议","明星","转发","评论","热度","新闻","刷屏","排名","围观"],"流量":["曝光","粉丝","推荐","转化","热门","数据","账号","涨粉","点击","算法","投放","关注"],"上热门":["推荐","流量","视频","点赞","算法","标题","封面","涨粉","曝光","完播","评论区","运气"],"竖屏":["手机","短视频","画幅","横屏","拍摄","构图","手持","画面","观看","剪辑","比例","沉浸"],"推荐算法":["流量","兴趣","标签","推送","曝光","数据","内容","精准","冷启动","点击","模型","分发"],"剪辑":["素材","时间线","转场","配乐","字幕","调色","导出","卡点","节奏","软件","成片","特效"],"翻拍":["模仿","镜头","原作","跟风","剧情","对比","字幕","复刻","热门","短视频","表演","版本"],"跟拍":["镜头","路边","采访","随机","路人","短视频","提问","剪辑","街头","互动","话题","真实"],"卡点":["音乐","节奏","剪辑","转场","踩点","鼓点","视频","变速","对拍","特效","舞蹈","成片"],"点赞":["互动","收藏","转发","评论","数量","喜欢","支持","双击","鼓励","数据","关注","分享"],"关注":["粉丝","订阅","点赞","互动","推荐","账号","取关","提醒","列表","收藏","更新","追踪"],"评论区":["留言","吐槽","互动","神评","点赞","回复","热评","话题","盖楼","网友","玩梗","气氛"],"全职妈妈":["带娃","家务","育儿","家庭","接送","辅食","陪伴","辛苦","牺牲","重返职场","琐碎","耐心"],"职场妈妈":["上班","带娃","通勤","加班","平衡","接送","家务","压力","愧疚","育儿","时间","支持"],"鸡娃":["补习","作业","兴趣班","成绩","家长","焦虑","升学","陪读","排名","课外","期望","竞争"],"辅导作业":["家长","耐心","数学","崩溃","讲解","陪读","课本","错题","发火","孩子","订正","晚上"],"亲子":["陪伴","游戏","绘本","出游","拥抱","沟通","成长","家庭","周末","手工","睡前","互动"],"带娃":["哄睡","辅食","玩具","尿布","遛娃","陪伴","疲惫","幼儿园","户外","喂奶","耐心","作息"],"二胎":["家庭","育儿","大宝","二宝","压力","陪伴","开销","兄弟姐妹","房子","分工","计划","热闹"],"三胎":["家庭","育儿","开销","房子","精力","兄弟姐妹","计划","讨论","补贴","陪伴","压力","选择"],"隔代":["祖辈","带娃","宠溺","观念","育儿","陪伴","代沟","老人","家庭","习惯","沟通","疼爱"],"家务":["扫地","洗碗","拖地","收纳","分工","晾衣","擦桌","清洁","琐碎","疲惫","整洁","习惯"],"烟火气":["菜市场","早餐","街边","生活","热闹","小摊","排档","人情","锅气","市井","寻常","温暖"],"特种兵旅游":["周末","行程","打卡","景点","赶路","攻略","火车","夜景","紧凑","省钱","体力","照片"],"显眼包":["搞笑","出场","夸张","性格","反差","表情","镜头","活宝","逗乐","热搜","自信","朋友"],"电子榨菜":["下饭","视频","综艺","追剧","吃饭","解说","背景音","陪伴","弹幕","解闷","循环","碎片时间"],"摸鱼":["上班","偷懒","茶水间","刷手机","划水","下班","老板","工位","效率","小憩","聊天","心虚"],"社恐":["聚会","打招呼","尴尬","安静","独处","紧张","电话","陌生","回避","内向","社交","躲开"],"社牛":["社交","自来熟","搭话","热情","聚会","破冰","朋友","主动","场合","健谈","气氛","勇气"],"边界感":["距离","尊重","拒绝","亲密","沟通","分寸","关系","隐私","独立","舒服","界限","成熟"],"钝感力":["迟钝","心态","从容","抗压","不在意","情绪","稳定","过滤","慢半拍","洒脱","自洽","包容"],"老钱风":["质感","羊绒","低调","剪裁","配色","经典","格调","大衣","珍珠","素色","优雅","投资"],"沉浸式":["体验","画面","音效","环境","专注","代入","房间","灯光","第一视角","氛围","放松","细节"],"助眠":["白噪音","睡前","音乐","呼吸","泡脚","床垫","香薰","放松","失眠","环境","早睡","安静"],"白噪音":["雨声","风扇","海浪","助眠","专注","耳机","循环","安静","背景音","放松","深夜","轰鸣"],"尴尬":["冷场","摸后脑","干笑","转移话题","脸热","脚趾抠地","退半步","装作无事","空气凝固","岔开"],"内疚":["亏欠","低头","补偿","那件事","不敢提","心里堵","还","绕开","记得","道歉"],"惭愧":["不如人","脸红","低头","敬佩","差距","努力","羞赧","讪讪","自省","奋起"],"憧憬":["远方","画卷","少年","光","设想","未来","蓝图","向往","梦里","启程"],"失落":["空手","垂肩","落空","转身","无声","散场","凉","望向别处","叹","背影"],"迷恋":["盯着","反复看","收藏","上头","绕不开","心跳","上瘾","照片","夜不能寐","靠近"],"暗恋":["偷看","纸条","走廊","名字","心跳","藏","校服","假装","好友圈","不说"],"倾慕":["仰望","追随","光","才华","靠近","写信","屏保","默默","敬","倾心"],"怅惘":["暮色","未寄","低声","旧曲","茶凉","倚窗","说不清","长街","细雨","散场"],"淡然":["笑","放下","茶","看开","不争","云","退一步","无谓","从容","自在"],"落寞":["空街","独影","路灯","旧椅","无人语","风","侧脸","远去","冷清","低头"],"孤寂":["深宵","四壁","回声","远山","无应","空白","雪","独行","静默","心口"],"凄清":["冷月","空庭","落叶","孤灯","风声","霜","夜半","无人","瘦影","寒"],"苍凉":["大漠","残阳","废墟","风沙","古道","枯树","远空","荒原","孤烟","岁月"],"空灵":["山寺","钟","雾","水声","留白","雪","远处","清音","禅","虚静"],"悠然":["南山","钓","茶","云","慢步","闲看","柳","风","不迫","自得"],"恬淡":["素","茶","书","无争","窗","慢","清","竹","心安","日常"],"闲适":["午休","藤椅","猫","阳光","茶","报","庭院","微风","无事","慢"],"惬意":["微风","冰饮","靠椅","晴日","海边","舒展","哼歌","慢","满足","笑"],"舒坦":["松口气","热水澡","躺平","暖","空","卸下","舒展","无压","窗外","睡意"],"郁结":["闷","胸口","叹气","绕","无解","眉心","夜","酒","写不下","堵"],"烦闷":["燥","来回走","看天","摔笔","闷雷","热","无事","刷屏","叹","窗外"],"懊恼":["拍额","早知","嘴快","出错","回头","改不了","闷","自责","叹","重来"],"恼火":["火气","瞪","拍桌","听不进","摔","青筋","骂","摔门","空气","压"],"愤懑":["不平","握拳","夜不能寐","呐喊","积压","无处说","胸中块垒","问","怒目","写"],"悲愤":["泪","咬牙","不公","站起","控诉","颤抖","怒","痛","声","碑"],"义愤":["挺身","不公","怒","出声","不平","援手","骨","问","站","光"],"亢奋":["红眼","停不下","话多","夜不睡","躁","跃起","计划","热","脉冲","冲"],"振奋":["起立","掌声","号角","挺直","光","前方","热","握拳","呐喊","醒"],"激昂":["演讲","红旗","热血","高歌","眉扬","潮","战","声","挺进","火"],"昂扬":["抬头","步伐","朝霞","号","不服","上","劲","风","山巅","歌"],"欢欣":["笑纹","雀跃","相拥","泪光","转圈","好消息","暖","亮","跳","报"],"怡然":["自乐","花前","浅笑","茶","慢","鸟","窗","无扰","心宽","步"],"陶然":["微醺","忘形","歌","风","晚霞","不觉","沉","笑","杯","乐"],"莞尔":["嘴角","不语","含笑","低头","眼弯","会心","轻","温","看穿","柔"],"凄楚":["孤灯","旧事","冷雨","低眉","无人懂","咽下","瘦","夜","衣薄","叹"],"缠绵":["雨","绕指","不舍","旧曲","低语","夜","绕","柔","难分","梦"],"缱绻":["相拥","发丝","低语","风","慢","不舍","温柔","夜","绕","眉"],"酸楚":["鼻酸","旧信","无人陪","雨","低头","忍","远","空","凉","咽"],"落落寡欢":["独坐","不语","疏离","笑不起来","人群外","低头","窗","冷","隔","淡"],"欢畅":["碰杯","大笑","放开","歌","风","同行","热","夜","畅谈","舞"],"释然一笑":["松手","明白","不再问","风","远","轻","回头","黄昏","茶","静"],"戚戚":["同悲","相顾","旧伤","叹","相怜","夜雨","知","泪","惺惺","穷"],"恻隐":["不忍","伸手","老弱","雨","低","软","叹","喂","怜","心"],"幽怨":["深闺","月","琴","未寄","低眉","旧约","等","泪","风","怨"],"怨怼":["别过脸","旧账","冷","不答","背","刺","积","话里有话","远","凉"],"愠怒":["抿嘴","侧目","不语","转身","压","眉","哼","不悦","退","冷"],"悻悻":["不服","撇嘴","转身","嘴硬","退","别过","不认","回头","哼","气"],"怅然若失":["空手","回望","原地","风","背影","说不出","凉","站定","远","静"],"懵懂":["少年","雾","未知","好奇","撞","问","错","笑","学","睁眼"],"木然":["不动","空望","无表情","坐","听不见","隔","凉","慢","僵","灰"],"战栗":["寒","抖","后退","牙关","恐惧","风","惊","缩","立毛","颤"],"悸动":["心跳","一瞬","抬眼","慌","脸红","屏息","近","声","夜","甜"],"心安":["灯下","饭香","归","暖","无虞","书","家人","雨停","床","静"],"惶惑":["四顾","不懂","乱","问","退","雾","手心汗","错愕","张望","生疏"],"释怀不了":["旧照","绕","夜","名字","信","未说","余温","风","回头","等"],"意难平":["旧事","不甘","夜","翻涌","问","泪","梗","多年","风","叹"],"心软":["低头","伸手","叹","孩子","眼","不忍","暖","妥协","笑","怜"],"倨傲":["抬颌","不屑","背手","俯视","冷","不答","远","笑","退","孤"],"谦卑":["低头","让","听","学","敬","退半步","躬","无争","温","空杯"],"局促":["手足无措","眼神飘","站也不是","坐也不是","讪讪","缩","汗","退","不敢","生疏"],"坦然处之":["笑","摊手","茶","看云","无惧","承认","稳","明白","路","自在"],"百感交集":["泪","旧","人海","重逢","说不出","胸","多年","风","静","叹"],"不寒而栗":["脊背","冷","回想","后退","抖","空","夜","影","闭眼","惊"],"心如止水":["湖面","坐","无波","茶","闭目","远山","静","空","风停","定"],"心潮澎湃":["浪","胸","热","夜不能寐","想","前","鼓","泪","立","光"],"五味杂陈":["酸甜苦辣","说不清","旧","人","笑泪","喉","叹","静","风","余味"],"如释重负":["卸下","长舒","瘫坐","笑","天亮","过关","泪","松手","轻","睡"],"怅然":["回望","空","风","远","静","说不出","凉","站","旧","淡"],"凄婉":["旧曲","低眉","月","泪","远","风","咽","怨","夜","瘦"],"自由意志":["选择","责任","羁绊","怀疑","决定","偶然","必然","挣扎","光","路"],"宿命":["线","绳","星图","偶遇","不可逆","手纹","雾","安排","认","浪"],"因果报应":["种","报","债","镜","轮回","回声","落子","缘","果","清"],"本真":["素","清水","赤子","卸妆","归","镜","童年","山","无饰","静"],"荒芜":["废墟","杂草","空城","风","尘","枯","断壁","寂","岁月","灰"],"丰盈":["果实","满","光","笑","收获","暖","圆","盛","秋","心"],"残缺":["断臂","月缺","补","留白","痕","不圆","美","遗憾","拼","缺角"],"圆满":["圆","合","团圆","灯","满","归","笑","足","月","圆桌"],"虚妄":["镜花","泡沫","梦","空","雾","幻","影","说破","散","轻"],"实在":["石","落地","重量","手感","根","土","真","踏实","锚","稳"],"喧嚣":["人海","喇叭","市井","噪","夜市","沸","声","攘","闹","尘"],"静谧":["雪夜","空山","灯微","无人","水声","落针","远","闭目","林","星"],"深邃":["井","夜空","眼","海","思","谷","渊","古","默","沉"],"浅薄":["浮","表面","笑谈","一知","风过","易逝","轻","浅","淡","散"],"辽阔":["旷野","海平线","天","草原","无界","远","风","自由","苍穹","展"],"逼仄":["巷","隔间","低顶","拥挤","压","喘","墙","窄","闷","退"],"混沌初开":["光","蛋","雾散","创世","鸿蒙","第一声","裂","醒","分","始"],"阴阳":["黑白","昼夜","太极","互补","鱼","平衡","冷暖","虚实","轮","和"],"道":["自然","无为","水","路","行","简","朴","天","心","一"],"禅":["空","坐","茶","钟","竹","静","顿悟","落花","无","境"],"顿悟":["一瞬","灯亮","拍案","笑","放下","窗外","旧话","通","醒","轻"],"执迷":["绕","旧","不肯","偏","守","梦里","问","十年","刻","盲"],"随缘":["风","放手","不强求","遇","淡","信","流水","聚散","安","顺"],"知止":["边界","收手","知足","退","不贪","明","止","稳","静","度"],"格物":["器物","穷理","观","析","手","匠","细节","知","磨","真"],"慎独":["无人","自省","灯下","心","守","暗室","诚","独","镜","清明"],"浩然之气":["正气","天地","挺直","歌","不屈","光","胸","养","大","立"],"悲天悯人":["众生","苦","俯身","叹","天下","泪","不忍","怀","远","慈"],"超脱":["抽身","云上","不染","笑","放下","远","观","轻","自在","空"],"入世":["烟火","忙","红尘","担","行","市","情","事","热","扛"],"出世":["山","寺","云","隐","静","远","不争","空","归","闲"],"春风":["绿","花开","暖","柳","燕","纸鸢","田","犁","笑","水"],"秋雨":["凉","梧桐","愁","灯","窗","落叶","孤","夜","寒","思"],"冬雪":["白","寂静","炉","梅","归","灯","寒","静","盖","年"],"夏雷":["震","雨","暗","窗","惊","闪","夜","闷","洗","醒"],"朝霞":["东","金","山尖","启程","鸟","露","绸","醒","远","歌"],"晚霞":["西","橘","江","归鸟","长堤","炊烟","醉","云","影","缓"],"大海":["潮","咸","帆","深处","蓝","浪","贝壳","风","远","呼吸"],"山峦":["叠","云绕","登","石","松","远","雾","脊","寺","俯"],"江河":["奔","桥","舟","岸","泥沙","落日","声","远","流","映"],"湖泊":["镜","倒影","静","舟","荷","柳","雾","鱼","月","闲"],"雪山":["白","冷","峰","云","登","氧","静","神","远","洁"],"竹林":["绿","风","影","笋","静","雨","幽","鸟","径","凉"],"梅花":["寒","香","雪","枝","傲","红","疏影","冬","骨","报春"],"枫":["红","秋","霜","山","叶","火","晚","径","染","别"],"萤火":["夜","微光","夏","草","孩","追","暗","梦","星","静"],"候鸟":["南","迁徙","秋","队","远方","归","天","季","翅","信"],"蝶":["花","翅","舞","蛹","春","彩","追","落","梦","轻"],"蚁":["穴","队","力","搬","合","微","路","巢","勤","群"],"犬":["尾","忠","归","门","陪","跑","嗅","守","摇","家"],"学生":["书包","课桌","考试","操场","笔","校服","题","铃","困","青春"],"工人":["扳手","车间","安全帽","焊","汗","夜班","机","蓝","力","厂"],"商人":["账","市","价","约","柜","算","信","船","利","路"],"图书管理员":["书架","静","索引","灯","尘","借","墨","窗","老","守"],"诗人":["月","酒","分行","愁","意象","夜","纸","风","狂","吟"],"音乐人":["弦","谱","夜","厅","泪","指","歌","灯","共鸣","独奏"],"匠人":["手","凿","磨","老","纹","静","传","器","心","年"],"法官":["槌","庭","天平","卷","正","沉默","法","镜","判","威"],"环卫工":["帚","晨","街","橙","霜","静","扫","灯","城市","辛"],"店主":["门","柜","笑","账","灯","客","货","茶","守","街"],"旅人":["包","路","远方","票","风","客栈","地图","落日","独","遇"],"归人":["灯","门","饭香","白发","笑","路","雨","家","暖","候"],"航天":["火箭","星","轨","舱","发射","宇宙","月","宇航员","焰","梦"],"脑机接口":["神经","信号","意念","接口","未来","电极","交互","思维","突","界"],"元宇宙":["虚拟","世界","身份","沉浸","社交","空间","数字","镜","未来","链"],"小说":["故事","人物","情节","作者","虚构","章节","读者","长篇","结局","叙事","出版社","悬念","爱情","想象"],"散文":["随笔","抒情","朱自清","段落","叙事","闲适","游记","语言","情感","短篇","杂感","散文诗"],"神话":["神","创世","英雄","天","雷","镜","古","梦","力","秘"],"传说":["民间","古","口","异","山","水","仙","怪","老","奇"],"民乐":["二胡","笛","古筝","琵琶","丝竹","宫商","山水","夜","韵","愁"],"瓷器":["釉","窑","天青","开片","温润","瓶","火","泥","宋","影"],"玉器":["温","润","雕","君子","白","佩","古","光","石","德"],"酒":["酒精","葡萄","白酒","醉","宴席","酒杯","酿造","茅台","干杯","宿醉","酒吧","礼仪","粮食","戒酒"],"香道":["炉","烟","静","篆","沉","慢","味","幽","室","闲"],"年画":["红","门神","胖娃","喜庆","木版","色","春","福","贴","俗"],"风筝":["线","风","天","纸","童","春","远","鸢","放","云"],"钟":["摆","声","旧","墙","时","滴答","老","静","忆","慢"],"伞":["雨","握","遮","肩","风","路","湿","陪","开","灰"],"书":["纸张","文字","作者","阅读","知识","书页","图书馆","出版","小说","油墨","书架","故事","教材","思考"],"信":["纸","邮戳","字","远","旧","墨","寄","等","月","温"],"照片":["影像","回忆","相机","瞬间","合影","相册","笑容","褪色","拍摄","风景","亲人","定格","滤镜","时间"],"杯":["茶","热","握","空","酒","唇","暖","碎","桌","独"],"路":["远","脚步","灯","雨","归","岔","尘","夜","行","前"],"桥":["跨江","桥墩","钢索","拱","渡河","连接","赵州桥","悬索","桥面","交通","建筑师","断裂","岸边","隧道"],"码头":["船","水","风","远","缆","鸥","汽笛","箱","归","咸"],"集市":["喧","摊","鲜","人","叫卖","烟火","色","早","俗","暖"],"茶香":["沸","盏","雾","静","苦","回甘","窗","慢","友","淡"],"月":["夜","思","圆","窗","影","诗","归","凉","孤独","光"],"星":["夜","愿","远","渺","闪","孩","秘","仰","海","静"],"火":["燃烧","火焰","热量","森林大火","取暖","灶台","烟","灰烬","危险","火柴","烧烤","光","氧气","燎原"],"水":["液体","河流","雨","饮用","海洋","冰","水分子","渴","灌溉","洪水","水杯","生命","污染","透明"],"立春":["回暖","芽","风","耕","燕","绿","希望","田","新","醒"],"谷雨":["雨","茶","种","田","润","绿","秧","水","春","丰"],"立夏":["热","绿","蝉","荷","风","长日","瓜","少年","光","盛"],"小满":["麦","渐满","田","风","青","水","穗","丰","温","望"],"芒种":["忙","收","种","稻","汗","田","镰","夏","累","丰"],"夏至":["最长","热","阳","昼","蝉","冰","光","盛","烈","午"],"立秋":["凉","叶","风","收","黄","思","渐","桂","远","静"],"白露":["凉","珠","草","晨","寒","月","清","衣","秋","静"],"秋分":["均","凉","月","桂","黄","夜","平","思","远","静"],"寒露":["凉","菊","寒","露","衣","秋深","风","远","瘦","静"],"霜降":["霜","红","寒","枫","收","衣","秋末","白","凉","静"],"立冬":["寒","藏","炉","衣","归","夜长","温","冬","静","暖"],"小雪":["初雪","寒","白","静","衣","灯","归","冷","冬","悄"],"大雪":["雪","白","厚","静","寒","盖","炉","归","年","寂"],"冬至":["最长夜","饺","阳生","寒","灯","家","数九","归","暖","静"],"小寒":["冷","风","衣","梅","寒","炉","冬深","白","静","缩"],"大寒":["最冷","风","雪","炉","衣","岁末","静","归","望","春"],"清晨":["光","鸟","露","醒","微","窗","静","咖啡","远","新"],"正午":["烈","影短","热","饭","光","长","闲","蝉","白","倦"],"深夜":["静","灯","独","思","黑","钟","无眠","风","影","凉"],"旧年":["雪","灯","归","饭","忆","钟","别","暖","远","静"],"新春":["红","炮","团","笑","灯","归","愿","暖","新","闹"],"等待":["钟","路口","信","远","椅","风","灯","盼","静","归"],"重逢":["人海","笑","泪","旧","名","抱","多年","话","茶","暖"],"相遇":["人海","目光","缘","一刻","笑","路","风","巧","开","暖"],"错过":["车","背影","路口","秒","叹","旧","空","回望","散","凉"],"选择":["岔路","心","担","后果","夜","秤","前","手","决","路"],"坚持":["夜","汗","不弃","路","伤","光","咬牙","远","信","终"],"放弃":["松手","空","叹","转身","轻","放下","不再","风","远","静"],"出发":["包","路","晨","票","风","远","决心","门","光","前"],"归来":["灯","门","饭香","白发","笑","路","雨","家","暖","候"],"迷失":["雾","岔","地图","夜","空","问","转","无向","站","凉"],"找到":["光","路","笑","握","答案","出口","风","明","归","定"],"破碎":["裂","声","散","拾","镜","心","地","凉","静","拼"],"愈合":["时间","疤","暖","静","新","笑","光","慢","淡","生"],"燃烧":["火","热","光","尽","舞","红","烈","灰","夜","燃"],"熄灭":["灰","凉","风","暗","静","余烟","空","落","终"],"生长":["芽","光","土","雨","慢","绿","力","春","向上","静"],"凋零":["叶","风","秋","落","黄","静","凉","地","叹","归"],"聚合":["人","光","力","圆","暖","声","聚","火","心","一"],"离散":["风","远","人","散","空","路","凉","各","静","叹"],"沉默如金":["不语","稳","思","墙","值","静","守","明","重","淡"],"言外之意":["弦外","眼","停","留白","笑","未说","会心","远","风","懂"],"破釜沉舟":["决","退路","火","战","背水","孤","志","焚","前","勇"],"柳暗花明":["转","光","路","村","笑","疑","开","新","暖","遇"],"守得云开":["等","夜","光","风","终","笑","明","静","归","愿"],"眼睛":["瞳孔","睫毛","眼神","泪水","凝视","虹膜","近视","眨眼","目光","窗户","疲惫","清澈","对视","眼影"],"手":["手指","掌纹","握手","抚摸","写字","劳动","冰冷","温暖","拳头","指甲","牵手","老茧","手势","十指"],"心":["跳","暖","伤","静","灯","声","空","软","血","语"],"声音":["声波","嗓音","回响","噪音","音乐","耳膜","呼唤","寂静","沙哑","广播","方言","分贝","录音","回声"],"气味":["香","旧","雨","饭","花","记","风","暖","熟悉","远"],"温度":["暖","凉","手","火","茶","风","肤","冬","夏","触"],"触碰":["手","暖","颤","近","肤","轻","温","握","心","静"],"呼吸":["空气","肺","鼻子","起伏","冥想","屏息","急促","平静","氧气","深呼吸","睡眠","生命","雾"],"梦境":["夜","飞","迷","旧","光","无逻辑","醒","奇","泪","虚"],"记忆":["老照片","气味","抽屉","童年","遗忘","碎片","海马体","重逢","声音","滤镜","疼痛","时间","梦","模糊"],"巷":["老","墙","猫","静","雨","晾","远","深","门","凉"],"书店":["架","静","墨","灯","纸","窗","老","翻","茶","守"],"咖啡馆":["香","窗","坐","杯","慢","乐","午后","人","暖","闲"],"老街":["青石","檐","铺","旧","香","慢","人","灯","雨","味"],"山野":["风","绿","溪","鸟","静","草","远","石","云","归"],"海岸":["浪","沙","风","远","咸","帆","落日","贝","蓝","静"],"庭院":["树","石","漏窗","竹","茶","静","影","月","闲","绿"],"笔":["墨水","笔尖","书写","铅笔","钢笔","圆珠笔","字迹","签名","记录","削","笔帽","灵感","纸张"],"墨":["香","砚","写","黑","研","纸","晕","老","深","静"],"纸":["白","写","折","旧","轻","墨","信","窗","薄","暖"],"砚":["石","墨","磨","凉","池","老","沉","文","端","静"],"琴":["弦","音","夜","指","松风","静","古","抚","韵","独"],"棋":["盘","谋","手谈","黑白","静","劫","算","对弈","心","势"],"剑":["光","侠","寒","舞","鞘","风","锋","影","侠客","啸"],"灯盏":["暖","夜","油","黄","静","影","旧","读","微","家"],"扇":["风","夏","墨","骨","摇","慢","雅","开","凉","手"],"舟":["水","桨","河","风","远","影","渔","静","渡","月"],"车":["路","风","远","灯","行","轮","窗","旅","夜","速"],"钟楼":["摆","声","城","旧","高","时","远","静","忆","风"],"古桥":["水","石","老","苔","风","远","影","过","静","月"],"面":["汤","热","巷","早","香","碗","蒸汽","暖","手","店"],"水果":["苹果","香蕉","菠萝","橘子","葡萄","西瓜","草莓","梨","芒果","樱桃"],"糖果":["甜","童","彩","纸","化","笑","兜","节","红","暖"],"茶点":["甜","盏","慢","午","雅","配","闲","香","细","友"],"火锅":["热","红","聚","喧","冬","涮","朋","汗","沸","闹"],"粽子":["端午","叶","香","糯","甜咸","河","旧","蒸","家","俗"],"红":["火","喜","血","旗","唇","梅","暖","烈","符","心"],"白":["雪","纸","净","月","素","始","空","光","衣","纯"],"黑":["夜","墨","影","深","盲","沉","神秘","发","寂","幕"],"青":["竹","天","瓷","山","春","淡","水","雅","松","远"],"绿":["草","春","树","希望","风","叶","野","静","生","凉"],"蓝":["海","天","静","远","深","夜","冷","梦","阔","谧"],"金":["光","秋","贵","阳","奖","稻","夕","辉","值","暖"],"紫":["暮","花","雅","神秘","罗","远","梦","贵","雾","幽"],"光":["明","晨","窗","影","暖","黑","希","路","灯","眼"],"影":["光","墙","静","夜","孤","长","随","黑","凉","默"],"友情":["酒","路","信","笑","并肩","远","懂","约","久","暖"],"师生":["讲台","目光","字","灯下","种","远","信","恩","书","静"],"陌生人":["路","眼","笑","擦肩","暖","雨","伞","善","远","偶"],"故乡":["老屋","炊烟","田埂","乡音","归途","母亲","槐树","中秋","漂泊","站台","泥土","童年","月亮","陌生","回不去"],"他乡":["异","客","语","月","独","路","风","寄","远","凉"],"少年":["校服","球场","风","梦","错","笑","汗","远","光","勇"],"老去":["白发","椅","旧","慢","镜","茶","忆","静","手","暖"],"孩子":["笑","手","问","童","闹","暖","纯","眼","跑","软"],"故人":["旧","信","月","茶","话","远","笑","忆","别","暖"],"奔跑":["风","汗","路","少年","自由","快","心跳","远","笑","晨"],"行走":["路","影","风","慢","街","远","思","夜","鞋","静"],"停留":["站","望","风","景","静","久","心","路口","灯","慢"],"仰望":["星","山","高","光","梦","敬","远","颈","静","希"],"俯身":["花","老","怜","拾","水","低","静","暖","手","柔"],"倾听":["声","静","耳","夜","风","你","远","心","温","默"],"书写":["笔","纸","夜","心","墨","思","灯","字","静","旧"],"歌唱":["声","风","夜","乐","笑","泪","远","心","歌","暖"],"哭了":["泪","肩","夜","静","泄","暖","旧","手","雨","软"],"笑了":["眼","暖","风","光","轻","你","静","甜","心","开"],"睡去":["灯","静","床","梦","夜","慢","暖","黑","听","归"],"醒来":["光","窗","晨","伸","静","新","鸟","凉","茶","暖"],"一":["独","始","全","简","纯","线","初","无对","专","元"],"零":["空","始","无","圈","净","归","虚","点","静","圆"],"圆":["满","月","环","无终","合","镜","回","团","润","轮"],"方":["正","格","矩","块","界","稳","石","桌","寸"],"上":["高","天","升","仰","顶","前","进","光","风","远"],"下":["地","沉","降","低","底","退","落","根","静","土"],"远":["路","山","思","行","云","梦","归","望","风","别"],"近":["手","声","你","温","触","眼","风","笑","暖","在"],"大":["海","山","天","容","梦","力","阔","象","志","世"],"小":["花","童","灯","声","微","手","巷","暖","细","静"],"快":["风","跑","时","讯","刀","笑","心","速","春","流"],"慢":["茶","光","老","思","钟","风","午","书","静","归"],"美":["晨光","花瓣","侧脸","倒影","青瓷","雪","裙摆","薄雾","晚霞","笑"],"丑":["疤","歪","油腻","暗角","扭曲","霉","裂缝","烂","嫌","暗"],"善":["让座","伸手","热汤","温","救","守","心软","怜","暖","行"],"恶":["暗巷","冷笑","贪","面具","刀","诱","黑水","密","吞","狠"],"真":["镜","赤","骨","光","追问","剥","诚","实","裸","醒"],"假":["面具","戏","伪","影","谎","空","套","妆","幻","饰"],"有":["在","实","物","握","具","现","存","形","重","满"],"无":["空","虚","静","忘","灭","道","境","失","寂","归"],"动":["风","流","行","变","浪","生","舞","转","活","时"],"静":["水","夜","坐","禅","山","空","止","思","影","定"],"变":["时","风","叶","潮","世","新","移","转","生","迁"],"恒":["星","石","海","誓","古","不灭","月","环","守","久"],"生":["芽","光","血","新","长","春","气","始","暖","望"],"死":["灰","静","别","终","凉","逝","墓","夜","空","归"],"爱":["情感","心动","亲密","付出","承诺","嫉妒","思念","婚姻","浪漫","牺牲","温柔","失恋","亲情","永恒"],"恨":["牙","火","旧","背","伤","冷","余","刀","远","烬"],"知":["书","问","光","路","师","思","镜","明","学","悟"],"惑":["雾","问","路","迷","暗","疑","思","停","茫","探"],"勇":["前","心","战","站","不退","光","脊","孤","决","火"],"怯":["退","低","抖","影","声","夜","躲","凉","缩","怕"],"电":["电流","电压","电池","发电","电线","灯泡","触电","能源","磁","插座","电网","闪电","安全","瓦特"],"虹":["雨","七","天","望","童","惊","彩","桥","希","笑"],"日":["光","暖","升","昼","东","金","望","长","明","生"],"四季":["春","夏","秋","冬","轮","耕","衣","景","时","变"],"桃":["春","粉","花","果","笑","溪","艳","园","红","香"],"李":["白","春","果","酸","园","小","繁","风","青","香"],"杏":["黄","春","果","村","暖","小","酸","风","粉","熟"],"桂":["秋","香","月","金","小","夜","甜","风","园","蜜"],"茉莉":["白","香","夏","小","茶","清","夜","淡","纯","润"],"蔷薇":["粉","篱","刺","夏","艳","园","风","攀","红","香"],"丁香":["紫","愁","春","小","雨","巷","香","结","淡","怨"],"梧桐":["秋","雨","叶","黄","街","老","凉","影","落","静"],"芭蕉":["雨","绿","叶","南","夏","听","影","愁","窗","湿"],"苔":["湿","石","绿","静","阴","老","院","雨","微","幽"],"莲":["水","清","粉","佛","叶","夏","洁","子","出尘","静"],"芦":["秋","水","白","风","岸","雪","老","飞","静","远"],"狐":["林","狡","尾","夜","传说","智","雪","眼","幻","野"],"蜂":["花","蜜","群","刺","勤","黄","翅","忙","巢","甜"],"雀":["枝","晨","小","闹","檐","跳","米","冬","群","声"],"鸦":["黑","枝","哑","暮","老","寒","影","预","静","孤"],"燕":["春","檐","剪","风","南","泥","归","斜","雨","家"],"鸥":["海","翅","白","浪","叫","码头","风","远","盐","飞"],"猿":["山","啼","林","夜","古","哀","雾","远","影","愁"],"凄然":["孤灯","冷雨","低眉","无人懂","咽下","瘦","夜","衣薄","叹","凉"],"戚然":["同悲","相顾","旧伤","叹","相怜","夜雨","知","泪","惺惺","穷"],"怆然":["泪","大恸","旧","风","碑","远","胸","逝","叹","凉"],"黯然":["低","别","灯","神伤","退","静","旧","泪","远","凉"],"萧索":["秋风","空枝","老街","凉","无人","落","淡","远","静","黄"],"岑寂":["深山","雪","空","无声","远","灯","夜","独","静","凉"],"清寂":["月","院","茶","无人","风","影","淡","远","静","空"],"欣悦":["笑","光","暖","雀跃","风","好消息","抱","跳","亮","歌"],"欢跃":["跳","笑","跑","风","光","朋","歌","汗","闹","暖"],"怡悦":["浅笑","花","风","茶","慢","鸟","窗","无扰","心宽","静"],"感佩":["敬","才","低头","鼓掌","眼里有光","学","远","挺直","叹","心"],"景仰":["仰望","名","高山","碑","光","追随","肃","深躬","专注","敬"],"倾心":["你","笑","暖","近","信","梦","光","手","真","守"],"芥蒂":["旧","隔","刺","远","不语","凉","绕","淡","退","解"],"隔阂":["墙","远","不语","凉","静","误","退","空","风","散"],"怨艾":["低","旧","叹","凉","自","不语","远","泪","夜","瘦"],"怏怏":["不乐","退","撇嘴","低","别过","闷","远","不悦","静","凉"],"悒悒":["不乐","低","愁","窗","雨","远","凉","静","旧","叹"],"陶陶":["乐","酒","风","笑","慢","忘","歌","暖","闲","然"],"融融":["暖","家","笑","饭","灯","春","聚","风","和","乐"],"熙攘":["人","市","喧","闹","街","声","攘","忙","烟火","沸"],"沸腾":["热","闹","声","血","场","欢","浪","燃","喊","涌"],"静默":["无","坐","夜","山","雪","思","空","影","灯","远"],"屏息":["静","近","惊","听","暗","心","风","秒","凝","望"],"凝神":["静","注","眼","思","灯","写","画","听","一","定"],"出神":["望","远","停","想","静","风","空","慢","落","虚"],"失神":["空","望","停","凉","落","惊","远","静","淡","恍"],"恍然":["明","拍","悟","笑","灯","通","旧","觉","轻","醒"],"猛然":["惊","醒","回","跳","觉","声","光","冷","退","记"],"猝然":["惊","变","倒","凉","瞬","乱","退","暗","失","风"],"怡然自得":["乐","闲","花","茶","慢","鸟","窗","无扰","心宽","步"],"怅怅":["空","回望","风","远","静","说不出","凉","站","旧","淡"],"恻怛":["悲","怜","心","旧","泪","远","叹","软","伤","静"],"萌芽":["芽","春","光","新","生","希望","土","雨","绿","始"],"抽枝":["绿","春","长","风","生","树","力","向上","夏","静"],"开花":["艳","春","香","蜂","光","笑","粉","风","绽","暖"],"结果":["实","秋","甜","累","枝","收","汗","丰","圆","望"],"落叶":["秋","黄","风","地","凉","静","归","叹","枝","散"],"飘零":["风","远","落","孤","凉","水","身","散","薄","叹"],"沉淀":["静","底","时","清","思","慢","水","久","真","厚"],"升华":["火","光","高","净","变","境","脱","新","远","明"],"消融":["暖","水","静","消","春","雪","散","无","化","柔"],"凝结":["冷","晶","静","聚","露","霜","固","时","形","定"],"聚拢":["人","光","力","暖","声","火","心","圆","近","合"],"散开":["风","人","云","水","声","远","凉","各","静","淡"],"蔓延":["绿","火","声","思","藤","夜","潮","远","长","生"],"收缩":["凉","冬","静","小","退","紧","夜","影","收","内"],"膨胀":["热","气","声","涨","满","夏","鼓","力","炸"],"回响":["声","山","空","远","钟","余","静","耳","久","深"],"余韵":["声","茶","诗","风","远","淡","静","长","味","思"],"油条":["脆","早","豆浆","油","金","街","热","咬","摊","香"],"豆浆":["白","早","热","豆","甜","配","暖","杯","摊","香"],"红烧肉":["油","酱","香","慢","家","软","褐","饭","暖","锅"],"糖醋排骨":["甜","酸","酱","褐","炸","香","筷","宴","热","童"],"烤鸭":["皮","脆","薄饼","酱","葱","京","香","片","宴","油"],"涮羊肉":["锅","白","麻酱","冬","薄","沸","朋","暖","筷","北"],"牛肉面":["汤","热","辣","早","香","碗","蒸汽","暖","手","街"],"米线":["汤","热","酸","南","滑","碗","香","辣","早","软"],"馄饨":["皮","汤","鲜","小","煮","早","暖","碗","街","薄"],"煎饼":["脆","酱","早","摊","薄","香","卷","街","热","朴"],"烧麦":["笼","馅","早","顶","米","香","白","褶","铺","热"],"奶茶":["甜","奶","茶","杯","暖","街","珍珠","闲","饮","香"],"绿茶":["清","苦","叶","春","杯","香","淡","改","墨","窗"],"面包师":["炉","粉","香","早","手","软","暖","黄","晨","甜"],"花艺师":["花","剪","瓶","色","雅","手","鲜","静","美","枝"],"摄影师":["镜头","光","快门","景","瞬间","旅","夜","街","影","捕捉"],"舞者":["旋转","脚尖","节拍","裙","汗","镜","力","美","落","台"],"导演":["镜","剧","喊","剪","想","片","场","灯","故","梦"],"渔夫":["船","网","河","早","鱼","风","咸","归","涛","静"],"牧民":["羊","草","帐","远","风","歌","鞭","天","迁","静"],"樵夫":["斧","山","柴","汗","林","晨","担","鸟","静","归"],"园丁":["花","剪","水","绿","手","春","静","泥","香","养"],"邮差":["包","车","巷","笑","信","铃","远","雨","门","暖"],"清洁工":["帚","晨","街","橙","霜","静","扫","灯","城","辛"],"衣":["布","针","身","暖","色","柜","洗","叠","旧","风"],"衫":["白","领","布","身","风","夏","旧","衣","色","软"],"裙":["摆","风","旋","色","夏","舞","腰","柔","花","美"],"裤":["腿","布","腰","袋","行","旧","色","洗","膝","便"],"帽":["头","阳","冬","檐","遮","风","色","旅","老","暖"],"包":["肩","物","路","旧","带","旅","装","色","手","重"],"表":["腕","时","针","滴答","旧","光","约","静","分","金"],"戒指":["指","金","圆","誓","光","礼","环","旧","爱","闪"],"项链":["颈","链","坠","光","礼","金","美","心","垂","闪"],"梳子":["发","木","齿","镜","晨","旧","顺","女","手","柔"],"手帕":["白","角","旧","泪","嗅","女","软","绣","怀","香"],"灯笼":["红","节","暖","夜","光","悬","童","影","喜","笼"],"船":["水","帆","桨","河","风","远","影","渔","渡","月"],"塔":["高","风","远","铃","古","登","云","影","寺","俯"],"楼":["高","窗","城","灯","层","影","居","风","夜","立"],"亭":["园","柱","歇","风","水","古","静","角","坐","闲"],"城墙":["古","砖","厚","远","史","门","风","老","立","肃"],"村落":["屋","田","烟","狗","静","远","老","树","鸡","乡"],"古筝":["弦","码","指","流水","韵","雅","拨","曲","夜","清"],"笛":["竹","孔","声","山","牧","清","唇","曲","风","远"],"琵琶":["弦","抱","轮","珠","曲","夜","声","女","拨","怨"],"钢琴":["键","黑","白","曲","厅","指","声","夜","泪","共鸣"],"小提琴":["弦","弓","颈","曲","夜","厅","声","手","悲","共鸣"],"太极":["慢","圆","静","柔","晨","气","老人","水","境","和"],"头":["思","发","帽","重","晕","枕","风","低","想","顶"],"脚":["路","鞋","步","尘","远","累","地","行","趾","停"],"耳":["声","听","镜","风","远","低","铃","静","侧","微"],"口":["言","食","笑","渴","唇","声","味","闭","语","热"],"血":["红","热","脉","命","伤","流","心","温","涌","色"],"骨":["硬","老","架","寒","白","支","古","冷","脊","坚"],"发":["黑","风","梳","长","白","镜","柔","肩","洗","乱"],"脸":["笑","镜","红","泪","风","颊","妆","光","表情","温"],"病":["床","药","医","热","弱","静","母","叹","养","凉"],"药":["苦","瓶","医","病","白","效","味","救","片","温"],"汗":["热","跑","肤","咸","力","夏","滴","疲","光","透"],"眠":["夜","床","静","梦","黑","灯","归","慢","沉","安"],"开始":["晨","笔","路","光","门","念","起","新","第一","前"],"结束":["落","灯","归","静","句","散","终","旧","空","远"],"过程":["路","时","变","步","流","长","细","行","果","验"],"原因":["因","根","问","源","链","果","始","探","故","理"],"变化":["时","风","叶","潮","世","新","移","转","生","迁"],"稳定":["石","静","稳","山","不摇","恒","平","根","定","安"],"混乱":["乱","噪","雾","翻","无序","失","狂","碎","涌","变"],"清晰":["镜","光","明","水","图","思","醒","目","白","净"],"模糊":["雾","远","旧","泪","夜","淡","失","虚","影","昏"],"真实":["镜","赤","骨","光","追问","剥","诚","实","裸","醒"],"虚幻":["镜花","泡沫","梦","空","雾","幻","影","说破","散","轻"],"简单":["一","白","净","少","明","淡","素","直","空","易"],"复杂":["网","层","乱","多","谜","缠","深","变","繁","思"],"明亮":["光","晨","灯","白","窗","晴","眼","笑","亮","净"],"轻盈":["风","舞","步","羽","云","笑","跳","薄","飞","柔"],"沉重":["石","担","夜","心","步","旧","压","影","默","喘"],"锋利":["刀","光","刃","割","寒","切","锐","伤","冷","金"],"柔软":["棉","风","声","手","绒","心","云","温","落","柔"],"坚硬":["石","铁","骨","冷","敲","山","坚","不破","壳","固"],"宽广":["海","天","原","胸","路","远","无界","阔","怀","展"],"狭窄":["巷","隙","逼","挤","压","退","闷","矮","局","促"],"古老":["石","树","城","钟","卷","痕","远","静","根","尘"],"崭新":["光","白","始","未拆","晨","亮","初","净","前","生"],"短暂":["瞬","花","夏","火","梦","落","光","露","歌","逝"],"想":["夜","你","远","眉","旧","静","灯","风","念","空"],"念":["旧","名","远","信","月","风","你","灯","落","静"],"读":["书","灯","静","夜","墨","窗","慢","思","纸","声"],"听":["声","静","耳","夜","风","你","远","心","温","默"],"看":["远","景","窗","你","书","山","夜","光","静","久"],"落":["叶","雨","雪","泪","地","风","晚","花","降","静"],"升":["日","旗","烟","光","气","月","晨","高","梦","前"],"沉":["水","夜","静","石","默","落","深","凉","黑","思"],"开":["门","花","窗","始","笑","伞","眼","新","灯","怀"],"关":["门","窗","灯","旧","夜","别","静","远","闭","凉"],"来":["风","你","信","客","春","归","路","声","暖","期"],"去":["路","风","远","背影","车","旧","凉","离","别","空"],"忘":["尘","空白","旧号","模糊","淡","走散","灰","轻","河","风"],"记":["旧","香","声","照片","街","夏","名","风","深","暖"],"等":["钟","路","信","远","椅","风","灯","盼","静","归"],"追":["风","你","梦","光","远","跑","车","影","慌","望"],"除夕":["团圆","饭","守岁","炮","红","夜","归","旧","迎","暖"],"腊八":["粥","腊","暖","冬","豆","香","旧","母","早","家"],"小年":["糖","灶","扫","归","红","近","旧","暖","春","忙"],"烛":["焰","夜","暖","泪","光","红","旧","静","愿","暗"],"盘":["菜","圆","桌","盛","白","厨","摆","净","香","暖"],"剪":["布","纸","咔","手","花","指","开","合","细","红"],"锁":["门","钥","旧","安","铁","闭","守","暗","扣","静"],"箱":["旧","衣","尘","远","行","装","木","藏","重","忆"],"篮":["编","果","野","手","竹","轻","采","暖","花","食"],"绳":["结","缚","长","旧","船","晾","牵","断","手","绕"],"铃":["声","风","门","颈","清","寺","童","远","响","脆"],"晨":["光","鸟","露","醒","微","窗","静","咖啡","远","新"],"昼":["光","热","长","忙","日","喧","景","行","明","活"],"年":["岁","春","旧","新","饭","钟","归","愿","雪","轮"],"分秒":["针","急","惜","滴答","短","赶","时","快","金","逝"],"光阴":["流","水","旧","白发","逝","惜","静","落","远","镜"],"岁月":["痕","老","静","尘","远","长","变","旧","河","默"],"刹那":["瞬","光","闪","短","落","惊","逝","一","定"],"须臾":["短","风","过","瞬","空","静","逝","一","微","虚"],"勇敢":["前","心","战","站","不退","光","脊","孤","决","火"],"诚实":["眼","话","镜","心","不遮","信","白","守","真","直"],"宽容":["笑","退","海","谅","风","放","心","度","静","怀"],"谦逊":["低头","让","听","学","敬","退半步","躬","无争","温","空杯"],"坚韧":["咬牙","夜","不弃","石","风","伤","路","信","终","脊"],"机智":["眼","转","笑","话","解","急","巧","光","变","答"],"幽默":["笑","话","眼","松","场","巧","逗","暖","智","声"],"浪漫":["花","月","信","风","诗","你","暖","夜","柔","梦"],"理智":["思","秤","静","退","析","冷","明","控","稳","脑"],"感性":["心","泪","风","诗","你","软","夜","梦","触","暖"],"专注":["静","眼","笔","灯","一","忘","深","定","思","流"],"自律":["早","规","笔","克","守","灯","独","成","稳","己"],"平等":["秤","人","线","无别","光","公","天","则","同","尊"],"公正":["天平","法","正","不偏","光","则","心","立","明","公"],"_meta":"T25 补充：保证「探索」可下钻 9 层以上 + 补全基础词「菠萝」「玻璃」。格式沿用 {词:[子词...]}。本文件字母序最后，覆盖同名 key。","原子核":["质子","中子","核力","放射性","聚变","裂变","夸克","核膜","核子"],"质子":["夸克","电荷","正电","原子核","质量","强相互作用","重子"],"中子":["夸克","电荷中性","原子核","β衰变","质量","重子"],"电子":["电荷","原子","电流","量子","轨道","轻子","能级","负电"],"核力":["强相互作用","夸克","原子核","胶子","束缚"],"夸克":["基本粒子","六种味","强相互作用","希格斯","反物质","夸克禁闭","胶子"],"基本粒子":["费米子","玻色子","夸克","轻子","规范玻色子","自旋"],"费米子":["轻子","夸克","电子","质子","中微子","自旋","半整数自旋"],"轻子":["电子","中微子","μ子","τ子","轻子数","中微子振荡","带电轻子"],"中微子":["弱相互作用","质量极小","中微子振荡","轻子","天体物理","中微子探测"],"玻色子":["光子","胶子","规范玻色子","希格斯","自旋整数","力载体"],"强相互作用":["夸克","胶子","原子核","核力","色荷"],"弱相互作用":["中微子","β衰变","夸克","轻子","W玻色子","Z玻色子"],"细胞核":["染色体","核仁","核膜","DNA","核孔","核质"],"染色体":["DNA","基因","组蛋白","着丝粒","染色单体","核小体"],"突变":["变异","随机","进化","碱基","序列","自然选择"],"碱基":["ATCG","核酸","配对","DNA","氢键","遗传密码"],"核酸":["DNA","RNA","核苷酸","磷酸","碱基","遗传"],"DNA":["碱基","双螺旋","遗传","基因","复制","脱氧核糖"],"双螺旋":["螺旋","结构","氢键","碱基配对","模型","沃森克里克"],"RNA":["核糖","密码子","转录","翻译","mRNA","tRNA"],"菠萝":["水果","热带","黄肉","刺","甜","凤梨","果汁","罐头","菠萝包","维生素C"],"材料":["金属","木材","塑料","玻璃","陶瓷","纤维","橡胶","复合材料","半导体"],"玻璃":["透明","镜片","窗户","玻璃杯","二氧化硅","折射","易碎","制造","工艺","镜子","钢化玻璃"],"金属":["铁","铜","铝","金","银","钢","导电","延展","合金"],"半导体":["硅","锗","二极管","芯片","能带","掺杂","晶体管"],"汽车":["厂家","特斯拉","马斯克","火箭","新能源","代步","跑车","绝版","装饰","车位","环境","绿色","结构","设计师","工程师","蒸汽时代","火车"],"手机":["通讯","屏幕","电池","芯片","应用","微信","iPhone","基站","拍照","充电","口袋","低头族","信号","移动互联网"],"星星":["夜空","恒星","闪烁","银河","星座","遥远","流星","黑暗","光年","牛郎织女","天文","浪漫","陨石","宇宙"],"春天":["花开","温暖","燕子","播种","柳树","清明","复苏","春雨","踏青","绿色","花粉","新年","希望","微风"],"生命":["细胞","DNA","呼吸","生长","死亡","进化","水","繁殖","意识","脆弱","顽强","物种","代谢","奇迹"],"健康":["身体","运动","饮食","睡眠","疾病","医生","免疫","心态","体检","营养","长寿","疲惫","习惯","活力"],"疾病":["病毒","症状","医生","药物","免疫","疼痛","传染","住院","疫苗","康复","诊断","慢性","恐惧","预防"],"睡眠":["夜晚","梦","枕头","床","休息","失眠","打鼾","疲惫","生物钟","被窝","褪黑素","午睡","清醒","安静"],"梦":["睡眠","潜意识","夜","幻境","噩梦","愿望","弗洛伊德","飞翔","醒来","记忆","荒诞","黎明","梦游","预示"],"死亡":["生命","终结","墓地","悲伤","葬礼","遗体","轮回","恐惧","临终","悼念","哲学","遗产","告别","永恒"],"朋友":["友谊","信任","陪伴","聊天","聚会","分享","支持","争吵","同学","知己","背叛","酒","远方","笑声"],"国家":["领土","主权","人民","政府","军队","国旗","边界","历史","民族","法律","外交","认同","税收","独立"],"战争":["军队","武器","士兵","伤亡","和平","战场","将军","炮火","侵略","难民","战略","历史","残酷","胜利"],"和平":["战争","安宁","谈判","条约","橄榄枝","停火","共存","鸽子","稳定","难民","宽容","合作","愿望","守护"],"教育":["学校","老师","学生","知识","考试","课程","成长","素质","家庭","公平","大学","启蒙","终身","改革"],"工作":["职业","办公室","老板","同事","工资","加班","简历","面试","压力","成就","通勤","退休","技能","KPI"],"钱":["货币","钞票","银行","工资","消费","贫穷","财富","贪欲","投资","硬币","钱包","物价","焦虑","交易"],"股市":["股价","涨跌","交易所","散户","K线","分红","牛市","熊市","风险","上市公司","投资","韭菜","账户","波动"],"学习":["知识","老师","课本","考试","记忆","练习","专注","好奇心","学校","笔记","进步","遗忘","方法","成长"],"灵感":["自然","清风","暖阳","折纸","山峦","叠加","穿插","交叠","色彩","冲击","爆裂","梦境","旋律","画笔","烟火","光"],"勇气":["恐惧","一步","心跳","深渊","表白","辞职","逆行","伤口","黎明","火","孩子","沉默","坚持"],"遗憾":["错过","如果","告别","旧信","未说出口","站台","背影","假如","青春","后悔","深夜","沉默","时间","释怀"],"雨伞":["雨天","屋檐","折叠","骨架","布料","两个人","淋湿","忘带","街头","彩色","遮挡","晴天"],"颜色":["红","蓝","黄","色相","明度","饱和度","调色","情绪","彩虹","油漆","黑白","冷暖","象征"],"舞台":["聚光灯","幕布","演员","掌声","后台","台词","谢幕","话剧","麦克风","紧张","化妆","观众","灯光"],"心跳":["心脏","脉搏","紧张","心动","加速","胸口","生命","咚咚","运动","恐惧","耳机","平静","触诊"],"味觉":["舌头","甜","咸","苦","酸","鲜","味蕾","麻辣","回味","失灵","童年","食物","嗅觉"],"疼痛":["伤口","神经","忍耐","药","眼泪","头痛","刺痛","麻木","呻吟","医院","成长","隐忍","缓解"],"钟表":["指针","齿轮","秒针","时间","发条","滴答","表盘","准时","钟楼","机械","走慢","石英","挂钟"],"竹子":["竹节","竹林","熊猫","竹笋","坚韧","空心","毛笔","郑板桥","生长","竹叶","箫","编织","清高"]};

</script>
<script>
/* ============================================================
 * 动感圆笔记 · Web 高保真交互演示
 * 依赖：data.js（万物页分类树），需在 app.js 之前加载。
 * 说明：普通 script（非 ES module），零外部依赖、零 CDN，file:// 双击可直接打开。
 * ============================================================ */
(function () {
  'use strict';

  /* ==========================================================
   * 0. 色卡集中区（唯一颜色出口）
   *    · 真源：style.css 顶部 :root 的 CSS 变量（--c1..--c5 / --l1..--l5 / --ink / --muted）
   *    · 本文件只在启动时读取一次，读不到时用下面的兜底值
   *    · 【后续替换成正式 16 色时，只改 style.css 里的那几个变量即可，本文件无需改动】
   * ========================================================== */

  /* 圆 / 板块填充色：用户色卡 16 色（逐像素取样，顺序=色卡从左到右、上到下） */
  var PALETTE = ['#EC6841', '#F19149', '#F7B551', '#FFF45C',
                 '#B3D465', '#7FC269', '#31B16C', '#12B4B1',
                 '#00B7EE', '#448AC9', '#556FB5', '#5F52A0',
                 '#8957A1', '#AD5DA1', '#EA68A2', '#EB6877'];
  /* 浅色色卡（文字颜色用，由主色卡提亮而来）——新建页文字颜色只从这里取 */
  var LIGHT_PALETTE = ['#BFC7C9', '#C7C7CD', '#CDD1CF', '#CDC7C4', '#D7D2C1'];
  var INK = '#2F2F35';              /* 主文字 / 线条 */
  var MUTED = '#5E616D';            /* 次级文字 / 图标线条 */
  var SURFACE_FALLBACK = '#ECEBE7'; /* 兜底底色（极浅灰） */
  /* 万物页「万物」大圆的填充色：与 App 启动图标同色的主题紫（唯一定义处，避免散落硬编码） */
  var INSPIRE_CORE_COLOR = '#7B5EA7';
  var INSPIRE_CORE_TEXT = '#FFFFFF';/* 紫底配白字，保证对比度 */

  /** 启动时把色卡从 CSS 变量同步进 JS（真源在 style.css） */
  (function syncColorsFromCss() {
    var cs = window.getComputedStyle(document.documentElement);
    function v(name) { return String(cs.getPropertyValue(name) || '').trim(); }
    function readAll(prefix, fallback) {
      var arr = [];
      for (var i = 1; i <= fallback.length; i++) {
        var s = v(prefix + i);
        if (!s) return null;          /* 任一缺失则整组沿用兜底值 */
        arr.push(s);
      }
      return arr;
    }
    var p = readAll('--c', PALETTE);
    if (p) PALETTE = p;
    var l = readAll('--l', LIGHT_PALETTE);
    if (l) LIGHT_PALETTE = l;
    var ink = v('--ink');
    if (ink) INK = ink;
    var muted = v('--muted');
    if (muted) MUTED = muted;
  })();

  /* 文字颜色卡（编辑器「文字颜色」弹窗专用）：以圆填充色卡为底复制一份，
     只把第 12 色 #5F52A0 → 灰 #8A8A8E、第 16 色 #EB6877 → 纯黑 #000000。
     真源是手机端 assets/editor/editor.js 的 PALETTE（U24 新表）；
     圆的填充色（本文件的 PALETTE / style.css 的 --c12/--c16）**保持旧值不动**，
     因为 App 的 Theme.kt NOTEBOOK_COLOR_HEX 也没换，两边必须一致。 */
  var TEXT_PALETTE = PALETTE.slice();
  TEXT_PALETTE[11] = '#8A8A8E';
  TEXT_PALETTE[15] = '#000000';

  /* ==========================================================
   * 1. 配置与常量
   * ========================================================== */
  var CFG = {
    refWidth: 390,       /* 演示屏宽（内部逻辑宽度） */
    minR: 42,            /* 兼容旧逻辑：基础最小半径（已不参与默认尺寸计算） */
    maxR: 50,            /* 基础最大半径 */
    defaultR: 100,       /* 默认圆半径 = 旧规则下「最大的圆」：maxR 50 × 封顶倍率 2.0 = 100 */
    childShrink: 0.52,   /* 作为子圆（被拖进别的圆）后的缩小系数 → 100 × 0.52 ≈ 52 */
    gap: 14,             /* 相邻圆竖直间隙 */
    margin: 18,          /* 左右 / 上下留白 */
    longPressMs: 1200,   /* 长按判定阈值：静态按住 1.2 秒（App HomeScreen.LIFT_LONG_PRESS_MS = 1200L） */
    moveTol: 12,         /* 长按期间允许移动的最大像素（超过则视为滚动） */
    maxChildren: 100,    /* 子圆竖条上限 */
    catLongPressMs: 600, /* 目录页「全部笔记」行长按删除的判定阈值（与圆主页的 longPressMs 无关，不要混用） */
    remainingDays: 13,   /* 可用时间剩余天数（静态演示值） */
    /* 【已废弃】下下层细分圆的数量上限。App 每一屏只显示同一层级，这里不再混入下下层，
       常量保留仅为兼容外部冒烟脚本的读取，不要再使用。 */
    grandChildLimit: 6,

    /* ---- 圆主页「二维画布」参数（比例均以可视区宽度 W 为基准）----
       思路：整张画布上圆竖向错开排列，屏幕只是画布的一个局部视口，
       圆可以只露一部分，用户左右 / 上下滑动视口即可看全。 */
    /* 说明：以下五个比例与安卓端 util/CircleLayout.kt 的常量**逐一对齐**，
       保证 Web 演示与真机看到的圆大小 / 间距 / 两列排布完全一致。 */
    canvasWRatio: 1.56,  /* 画布宽 = 1.56 × 可视区宽（比屏幕宽，才有左右滑动空间） */
    rMinRatio: 0.33,     /* 圆半径下限 = 0.33W（390 屏宽 → 129px）——比原来更大 */
    rMaxRatio: 0.38,     /* 圆半径上限 = 0.38W（390 屏宽 → 148px） */
    colLeftRatio: 0.27,  /* 左列圆心 x = 0.27 × 画布宽（≈0.42W） */
    colDxRatio: 0.74,    /* 右列圆心 x = 左列 + 0.74W（横向最多两列，缝隙收紧但不重叠） */
    pitchRatio: 0.30,    /* 纵向节拍 = 0.30W（≈117px）——竖向错开排列，缝隙更小 */
    jitterXRatio: 0.02,  /* 圆心横向确定性抖动 ±0.02W（不是固定错位） */
    jitterYRatio: 0.015, /* 纵向确定性抖动 ±0.015W */
    firstYRatio: 0.05,   /* 首个圆心 y = r + 0.05W */
    /* 【已废弃】原固定 4px 间隙，已改为按比例 W × 0.02 计算（App CircleLayout.MIN_EDGE_GAP_RATIO = 0.020f，
       gap = containerWidth × ratio）。常量保留仅为兼容外部冒烟脚本的读取，layoutCircles 不再使用。 */
    minGap: 4,
    /* 不重叠松弛：解析解通常 1 轮收敛；下面两个常量只在解析解退化（|dx| ≥ need）时兜底 */
    relaxStepRatio: 0.02, /* 兜底下移步进 = 0.02W */
    relaxMaxPass: 8       /* 最多迭代 8 轮，保证有界 */
  };

  /* 父圆内套的小圆槽位（可配置常量）：
   *   圆内套的小圆越多，里面的小圆越小（半径 0.34R → 0.28R → 0.23R）。
   *   size = 子圆直径占父圆直径的百分比（= 子圆半径 / 父圆半径 × 100），
   *   与 Android util/CircleLayout.kt 的 childSlots **逐一对齐**（两端统一 0.34R / 0.28R / 0.23R）。
   *   pos 为 [left%, top%]（相对父圆外框），最多显示 3 个，超出用「+N」角标。
   *   **y 向下为正**：子圆簇落在父圆**圆心下方**，与 App util/CircleLayout.kt 的
   *   childSlots 偏移（+0.46R / +0.44R / +0.34R / +0.64R，单位 R，向下为正）等价：
   *     top% = (1 + offsetY/R − childRadius/R) / 2 × 100
   *
   *   硬约束（两端共同）：子圆圆心到父圆圆心距离 + 子圆半径 ≤ 0.88·父圆半径，
   *   保证子圆**完整落在父圆内、不被圆形裁剪**。当前各槽位实测余量（1 − edge/R）：
   *     1 个子圆：edge = 0.80R → 余量 20%
   *     2 个子圆：edge = 0.824R → 余量 17.6%
   *     3 个子圆：edge = 0.755R（两侧）/ 0.87R（中间）→ 余量 24.5% / 13%
   *   子圆之间同样不重叠（最近两圆圆心距 ≥ 半径和）。 */
  var INNER_SLOT = {
    1: { size: 34, pos: [[33, 56]] },                          /* 单个：水平居中、圆心下方（0.46R） */
    2: { size: 28, pos: [[52, 58], [20, 58]] },                /* 两个：左右对称、圆心下方（0.44R） */
    3: { size: 23, pos: [[38.5, 70.5], [18.5, 55.5], [58.5, 55.5]] } /* 三个：左右靠上（0.34R）+ 中间靠下（0.64R） */
  };

  /* ---------- 万物页「混合方案」联网开关 ----------
   * 离线为主：默认 false，直接读 data.js 内置分类树，零成本、永不失效。
   * 预留联网接口：将来接 LLM 在线展开时——
   *   1) 把 INSPIRATION_USE_REMOTE 置为 true；
   *   2) 填上 INSPIRATION_API_URL；
   *   3) 在 fetchChildrenRemote() 里实现远程请求（只改这一处）。 */
  var INSPIRATION_USE_REMOTE = false;
  var INSPIRATION_API_URL = ''; /* 例如 'https://api.example.com/inspire/children' */
  var INSPIRE_ROOT_ID = 'wanwu';   /* 万物页分类树根节点 id（见 data.js） */

  /* ==========================================================
   * 2. 状态
   * ========================================================== */
  var state = {
    tab: 'ring',          /* 当前 Tab：catalog | ring | inspire | new */
    notebooks: [],        /* {id, title, content, rich, parentId, color, orderIndex, createdAt, updatedAt} */
    nextId: 1,
    detailStack: [],      /* 详情页路由栈 */
    quickParentId: null,  /* 新建 Tab 当前绑定的父圆 id */
    quickSeq: 0,
    inspPath: [],         /* 万物页当前路径（节点 id 数组），空 = 初始「万物」单圆态 */
    inspCustom: null,     /* 万物页解算出的自定义圆阵 {forId, nodes} */
    inspQuery: '',        /* 万物页搜索栏当前词 */
    /* 万物页当前屏的层级 = 已下钻次数（与 App InspireViewModel 的 `level = stack.size` 等价）。
       决定圆直径档位：0 → lv1（84~108），≥1 → lv2（68~84，透明度 .92）。 */
    inspLevel: 0,
    searchOpen: false,
    trashOpen: false,     /* 回收站覆盖层是否打开（不是第 5 个 tab，不依赖 state.tab） */
    trashSelected: {}     /* 回收站选中集：{id: true}，随 renderTrash 重绘保留 */
  };

  /* ==========================================================
   * 3. 通用工具
   * ========================================================== */

  /** HTML 转义，避免用户输入破坏结构 */
  function esc(s) {
    return String(s === null || s === undefined ? '' : s)
      .replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;').replace(/'/g, '&#39;');
  }

  function $(id) { return document.getElementById(id); }

  /** 确定性伪随机（xorshift32），以 id 为种子 → 同一 id 每次结果恒定，重绘 / 滚动不抖动 */
  function rand01(seed) {
    var x = (seed * 2654435761) >>> 0;
    if (x === 0) x = 1;
    x ^= (x << 13); x >>>= 0;
    x ^= (x >>> 17);
    x ^= (x << 5);  x >>>= 0;
    return x / 4294967296;
  }

  /** 由字符串生成稳定 hash（万物页节点 id 是字符串） */
  function hashStr(s) {
    var h = 2166136261;
    for (var i = 0; i < s.length; i++) {
      h ^= s.charCodeAt(i);
      h = Math.imul(h, 16777619);
    }
    return (h >>> 0) / 4294967296;
  }

  /** 依据背景色明度自动选择文字颜色（16 色色卡专用规则）：
   *   相对亮度 L < 0.20 → 白字；否则 → 近黑 #1A1A1A。
   *   按此规则，#556FB5 / #5F52A0 / #8957A1 / #AD5DA1 四色用白字，其余 12 色用 #1A1A1A。 */
  function textColorFor(hex) {
    var h = String(hex || '').replace('#', '');
    if (h.length === 3) h = h[0] + h[0] + h[1] + h[1] + h[2] + h[2];
    var r = parseInt(h.substr(0, 2), 16) / 255;
    var g = parseInt(h.substr(2, 2), 16) / 255;
    var b = parseInt(h.substr(4, 2), 16) / 255;
    function lin(c) { return c <= 0.03928 ? c / 12.92 : Math.pow((c + 0.055) / 1.055, 2.4); }
    var L = 0.2126 * lin(r) + 0.7152 * lin(g) + 0.0722 * lin(b);
    return L < 0.20 ? '#FFFFFF' : '#1A1A1A';
  }

  /** 时间戳格式化为「YYYY-MM-DD HH:mm」 */
  function fmtTime(ts) {
    var d = new Date(ts);
    function p(n) { return (n < 10 ? '0' : '') + n; }
    return d.getFullYear() + '-' + p(d.getMonth() + 1) + '-' + p(d.getDate()) +
           ' ' + p(d.getHours()) + ':' + p(d.getMinutes());
  }

  /* ==========================================================
   * 4. 数据层
   * ========================================================== */

  /** 按 id 取单条。**故意不过滤 deletedAt**：恢复 / 彻底删除 / 面包屑 / 编辑页写回都要读已删行。 */
  function byId(id) {
    for (var i = 0; i < state.notebooks.length; i++) {
      if (state.notebooks[i].id === id) return state.notebooks[i];
    }
    return null;
  }

  /* 软删除标记（与 App NotebookEntity.deletedAt 同名同义）：
     - 级联删除整棵子树时给所有后代写**同一个** deletedAt（删除批次），
       恢复时按该批次精确还原，不会误恢复更早被单独删掉的子圆；
     - 种子数据 / 旧数据可能没有这个字段 → 一律用真值判断（undefined 当作未删）。 */

  /** 取某父圆的子圆（按 orderIndex 排序；**已删行不出现在任何 UI 列表里**） */
  function childrenOf(parentId) {
    return state.notebooks
      .filter(function (n) { return n.parentId === parentId && !n.deletedAt; })
      .sort(function (a, b) { return a.orderIndex - b.orderIndex; });
  }

  /** 取所有根圆（首页大圆，按 orderIndex 排序；已删行不画在画布上） */
  function roots() {
    return state.notebooks
      .filter(function (n) { return n.parentId === null && !n.deletedAt; })
      .sort(function (a, b) { return a.orderIndex - b.orderIndex; });
  }

  /** 取所有笔记（含子圆），用于目录页；已删行只出现在回收站里 */
  function allNotebooks() {
    return state.notebooks.filter(function (n) { return !n.deletedAt; })
      .sort(function (a, b) {
        return (a.parentId === null ? 0 : 1) - (b.parentId === null ? 0 : 1) ||
               a.orderIndex - b.orderIndex || a.id - b.id;
      });
  }

  /** candidateId 是否为 ancestorId 的子孙（用于阻止把父圆拖进自己的子圆造成环） */
  function isDescendant(candidateId, ancestorId) {
    var cur = byId(candidateId);
    var guard = 0;
    while (cur && cur.parentId !== null && guard++ < 1000) {
      if (cur.parentId === ancestorId) return true;
      cur = byId(cur.parentId);
    }
    return false;
  }

  /** 收集 rootId 及其全部子孙的 id（**含已删行**，软删 / 物理删共用）。
   *  不能用 childrenOf：它已过滤已删行，purge 会漏掉回收站里的后代。 */
  function subtreeIds(rootId) {
    var doomed = {};
    doomed[rootId] = true;
    var queue = [rootId];
    var guard = 0;
    while (queue.length && guard++ < 5000) {
      var cur = queue.shift();
      for (var i = 0; i < state.notebooks.length; i++) {
        var n = state.notebooks[i];
        if (n.parentId === cur && !doomed[n.id]) {
          doomed[n.id] = true;
          queue.push(n.id);
        }
      }
    }
    return doomed;
  }

  /**
   * 软删除整棵子树（圆 + 全部子孙）：只打 deletedAt 标记，**不物理移除**，返回被标记的条数。
   *
   * 与手机端 HomeViewModel.trashNote → NotebookRepository.trashNotebook 对齐：
   * 整棵子树写同一个时间戳（删除批次）；已经是回收站里的后代不再改写，
   * 这样恢复时按批次还原，不会把更早单独删掉的子圆一起复活。
   */
  function trashSubtree(rootId) {
    var root = byId(rootId);
    if (!root || root.deletedAt) return 0;
    var doomed = subtreeIds(rootId);
    var batch = Date.now();
    var marked = 0;
    for (var i = 0; i < state.notebooks.length; i++) {
      var n = state.notebooks[i];
      if (!doomed[n.id] || n.deletedAt) continue;
      n.deletedAt = batch;
      marked++;
    }
    /* 新建页若正绑定在已删子树里的圆上 → 解绑，避免把新笔记写进回收站里的圆 */
    if (state.quickParentId !== null && doomed[state.quickParentId]) state.quickParentId = null;
    return marked;
  }

  /**
   * 回收站条目：只列「被删子树的根」——父圆不存在或父圆未删，
   * 被级联删掉的子圆不重复出现（同 App NotebookDao.observeTrash 的 NOT IN 条件）。
   */
  function trashItems() {
    return state.notebooks
      .filter(function (n) {
        if (!n.deletedAt) return false;
        if (n.parentId === null) return true;
        var p = byId(n.parentId);
        return !p || !p.deletedAt;
      })
      .sort(function (a, b) { return (b.deletedAt || 0) - (a.deletedAt || 0) || b.id - a.id; });
  }

  /** 恢复：只还原**同一删除批次**的整棵子树（同 App NotebookRepository.restore）。返回还原条数。 */
  function restore(id) {
    var root = byId(id);
    if (!root || !root.deletedAt) return 0;
    var batch = root.deletedAt;
    var scope = subtreeIds(id);
    var count = 0;
    for (var i = 0; i < state.notebooks.length; i++) {
      var n = state.notebooks[i];
      if (!scope[n.id] || n.deletedAt !== batch) continue;
      n.deletedAt = null;
      count++;
    }
    /* 孤儿兜底（正常不会发生：purge 也是级联的）→ 父圆不存在就降级成根圆 */
    if (root.parentId !== null && !byId(root.parentId)) {
      root.parentId = null;
      root.orderIndex = roots().length;
    }
    return count;
  }

  /**
   * 物理删除整棵子树（圆 + 全部子孙），返回被移除的条数。
   *
   * 现在**只作为 purge / purgeAll 的内部工具**：拖拽删除已改为软删除（trashSubtree），
   * 否则用户一拖就永久丢数据。
   */
  function deleteSubtree(rootId) {
    if (!byId(rootId)) return 0;
    var doomed = subtreeIds(rootId);
    var before = state.notebooks.length;
    state.notebooks = state.notebooks.filter(function (n) { return !doomed[n.id]; });
    /* 新建页若正绑定在已被删掉的圆上 → 解绑，避免把新笔记写进不存在的父圆 */
    if (state.quickParentId !== null && !byId(state.quickParentId)) state.quickParentId = null;
    return before - state.notebooks.length;
  }

  /** 彻底删除：该行及其全部子孙**物理**移除，返回移除条数。 */
  function purge(id) {
    return deleteSubtree(id);
  }

  /** 清空回收站：只物理删已删行，活笔记零风险（同 App dao.deleteAllTrashed）。返回删除条数。 */
  function purgeAll() {
    var before = state.notebooks.length;
    state.notebooks = state.notebooks.filter(function (n) { return !n.deletedAt; });
    if (state.quickParentId !== null && !byId(state.quickParentId)) state.quickParentId = null;
    return before - state.notebooks.length;
  }

  /** 取该圆到根的路径名，用于详情页面包屑 */
  function pathOf(id) {
    var names = [];
    var cur = byId(id);
    var guard = 0;
    while (cur && guard++ < 1000) {
      names.unshift(cur.title || '无标题');
      cur = cur.parentId === null ? null : byId(cur.parentId);
    }
    return names.join(' / ');
  }

  function nextColor() {
    // 需求 #3/#28：新建圆颜色随机（从 16 色卡随机取一种）
    return PALETTE[Math.floor(Math.random() * PALETTE.length)];
  }

  /** 新建笔记（父圆 or 子圆）。返回新对象。 */
  function createNotebook(title, content, parentId) {
    var nb = {
      id: state.nextId++,
      title: title || '',
      content: content || '',
      rich: '',                                /* 新建页富文本 HTML（content 为其纯文本镜像） */
      parentId: (parentId === undefined ? null : parentId),
      color: nextColor(),
      orderIndex: 0,
      createdAt: Date.now(),                   /* 创建时间戳 */
      updatedAt: Date.now(),
      deletedAt: null                          /* 软删除时间戳（null = 未删；回收站机制见「数据层」） */
    };
    var siblings = nb.parentId === null ? roots() : childrenOf(nb.parentId);
    nb.orderIndex = siblings.length;
    state.notebooks.push(nb);
    return nb;
  }

  /** 建立 / 改变包含关系。
   *    · parentId !== null（被拖进某个圆）→ 清掉「拎出」标记，回到普通大圆档位；
   *    · parentId === null（从小圆拎出成独立根圆）→ 插到**原父圆之后**，
   *      与 App HomeViewModel.detachToRoot / NotebookRepository.detachToRootAfter 一致
   *      （原父圆索引 + 1，而不是排到列表末尾）。 */
  function setParent(id, parentId) {
    var nb = byId(id);
    if (!nb) return false;
    var oldParentId = nb.parentId;
    nb.parentId = parentId;
    nb.updatedAt = Date.now();
    if (parentId !== null) nb.detached = false;   /* 又被包含 → 不再是拎出档 */

    function reorder(list) {
      list.forEach(function (n, i) { n.orderIndex = i; });
    }
    reorder(oldParentId === null ? roots() : childrenOf(oldParentId));
    reorder(parentId === null ? roots() : childrenOf(parentId));

    /* 拎出成本：紧跟在原父圆后面（不是末尾），App 的 detachToRootAfter 语义 */
    if (parentId === null && oldParentId !== null) {
      var list = roots().filter(function (n) { return n.id !== id; });
      var at = list.length;
      for (var i = 0; i < list.length; i++) {
        if (list[i].id === oldParentId) { at = i + 1; break; }
      }
      list.splice(at, 0, nb);
      list.forEach(function (n, i) { n.orderIndex = i; });
    }
    return true;
  }

  /* ==========================================================
   * 5. 圆布局算法
   * ----------------------------------------------------------
   * 新的大小规则（对应用户要求 7）：
   *   1) 默认（独立 / 新建）圆 = 旧规则下「最大的那个圆」→ CFG.defaultR；
   *   2) 被长按拖进别的圆、成为子圆后 → 尺寸缩小（CFG.childShrink 系数）；
   *   3) 不再按子圆数量放大。
   * 纵向：cy_i = cy_{i-1} + r_{i-1} + GAP + r_i → 竖直不重叠；
   * 横向：按 id 确定性随机错位（同一 id 每次位置一致，滚动/重绘不抖动）。
   * ========================================================== */

  /** 某圆在画布上的半径：按 id 确定性落在 [rMinRatio·W, rMaxRatio·W]。
   *   同一 id 恒定（滚动 / 重绘不抖动），不同 id 大小不一；成为子圆后缩小。
   *   被「拎出」成独立圆的（nb.detached）走 **中间档 0.21W ± 0.02W**
   *   （App CircleLayout.DETACH_RADIUS_BASE_RATIO / DETACH_RADIUS_JITTER_RATIO），
   *   正好落在普通大圆 0.33~0.38W 与父圆内子圆 0.076~0.129W 之间。 */
  function radiusOf(nb, containerWidth) {
    var W = containerWidth || CFG.refWidth;
    var id = (nb && nb.id) || 0;
    var r;
    if (nb && nb.detached) {
      r = W * (0.21 + (rand01(id * 31 + 7) * 2 - 1) * 0.02);
    } else {
      r = W * (CFG.rMinRatio + rand01(id * 31 + 7) * (CFG.rMaxRatio - CFG.rMinRatio));
    }
    if (nb && nb.parentId !== null) r *= CFG.childShrink;
    return Math.max(20, r);
  }

  /** 兼容旧签名：由 id 取半径 */
  function radiusFor(id, containerWidth) {
    return radiusOf(byId(id), containerWidth);
  }

  /** 二维画布布局：返回 { canvasW, canvasH, positions }
   *   · 画布比可视区宽（canvasWRatio），屏幕只是画布的一个局部视口；
   *   · 圆竖向错开（左 / 右两列 + 确定性抖动），纵向节拍收紧，圆可以只露一部分；
   *   · 用户左右 / 上下滑动视口即可看全任意圆。 */
  function layoutCircles(ids, containerWidth) {
    var W = containerWidth || CFG.refWidth;
    if (W <= 0) return { canvasW: 0, canvasH: 0, positions: [] };

    var canvasW = Math.round(W * CFG.canvasWRatio);
    var radii = ids.map(function (id) { return radiusFor(id, W); });
    var positions = [];
    var colLeft = canvasW * CFG.colLeftRatio;
    var colRight = colLeft + W * CFG.colDxRatio;

    var cy = (radii.length ? radii[0] : 0) + W * CFG.firstYRatio;
    for (var i = 0; i < ids.length; i++) {
      var r = radii[i];
      /* 左右两列 + 确定性抖动（不是固定错位量） */
      var baseX = (i % 2 === 0) ? colLeft : colRight;
      var jx = (rand01(ids[i] * 977 + 13) - 0.5) * 2 * W * CFG.jitterXRatio;
      var cx = baseX + jx;
      /* 夹在画布内，允许左右各出血一点点 */
      cx = Math.max(r - W * 0.02, Math.min(cx, canvasW - r + W * 0.02));
      positions.push({ id: ids[i], cx: cx, cy: cy, r: r });
      cy += W * CFG.pitchRatio + (rand01(ids[i] * 613 + 29) - 0.5) * 2 * W * CFG.jitterYRatio;
    }

    /* ---- 不变量修复：任意两圆圆心距 >= 半径和 + minGap ----
       与 Android util/CircleLayout.kt 的 relaxAgainstOverlap **行为对齐**：
       **只把后一个圆的圆心往下移，绝不缩小半径** —— 圆要尽量大（用户明确要求），
       旧版「各缩 ×0.97」会让 Web 端比 Android 端小 3%，已废弃。
       下移量取解析解 minY = A.cy + √(need² − dx²)（一次算准，同 Android 公式）；
       退化情形（|dx| ≥ need，理论上不会出现）用 relaxStepRatio·W 的小步进兜底；
       最多迭代 relaxMaxPass 轮。全过程只依赖已确定的 cx / r / 顺序 → 结果确定性。 */
    var minGap = W * 0.02;   /* = App CircleLayout.MIN_EDGE_GAP_RATIO(0.020f) × 容器宽度，随屏宽缩放 */
    var relaxStep = W * CFG.relaxStepRatio;
    for (var pass = 0; pass < CFG.relaxMaxPass; pass++) {
      var moved = false;
      /* b 升序：把 b 下移只会增大它与「上方所有圆」的 dy，因此一轮即收敛 */
      for (var b = 1; b < positions.length; b++) {
        var B = positions[b];
        for (var a = 0; a < b; a++) {
          var A = positions[a];
          var dx = B.cx - A.cx, dy = B.cy - A.cy;
          var need = A.r + B.r + minGap;
          if (dx * dx + dy * dy >= need * need) continue;   /* 已满足，跳过 */
          var needDySq = need * need - dx * dx;
          var minY = needDySq > 0 ? A.cy + Math.sqrt(needDySq) : B.cy + relaxStep;
          if (B.cy < minY) { B.cy = minY + 0.5; moved = true; }
        }
      }
      if (!moved) break;
    }

    var canvasH = 0;
    for (var k = 0; k < positions.length; k++) {
      canvasH = Math.max(canvasH, positions[k].cy + positions[k].r + W * CFG.firstYRatio);
    }
    return { canvasW: canvasW, canvasH: Math.round(canvasH), positions: positions };
  }

  /** 兼容旧调用：只要高度 */
  function contentHeight(positions) {
    if (!positions.length) return CFG.margin * 2;
    var last = positions[positions.length - 1];
    return last.cy + last.r + CFG.margin;
  }

  /* ==========================================================
   * 6. DOM 引用
   * ========================================================== */
  var ringScroll = $('ringScroll');
  var ringFab = $('ringFab');
  var dragLayer = $('dragLayer');
  var detailLayer = $('detailLayer');
  var trashLayer = $('trashLayer');
  var toastEl = $('toastEl');
  var tabbar = $('tabbar');
  var searchPanel = $('searchPanel');
  var viewCatalog = $('view-catalog');
  var viewRing = $('view-ring');
  var viewInspire = $('view-inspire');
  var viewNew = $('view-new');

  /* 模板 Tab 已删除：视图表不再含 templates（见 index.html） */
  var VIEWS = {
    catalog: viewCatalog, ring: viewRing, inspire: viewInspire, 'new': viewNew
  };

  /* ==========================================================
   * 7. Toast
   * ========================================================== */
  var toastTimer = null;
  function toast(msg) {
    toastEl.textContent = msg;
    toastEl.classList.add('show');
    if (toastTimer) clearTimeout(toastTimer);
    toastTimer = setTimeout(function () { toastEl.classList.remove('show'); }, 1600);
  }

  /* ==========================================================
   * 8. 进度条指示器（横向书页进度条 / 竖向滚动指示器，样式同源）
   * ========================================================== */
  function bindHProgress(scrollEl, barEl) {
    if (!scrollEl || !barEl) return;
    var thumb = barEl.querySelector('.hp-thumb');
    function update() {
      var sw = scrollEl.scrollWidth;
      var cw = scrollEl.clientWidth;
      if (sw <= cw) { thumb.style.width = '100%'; thumb.style.left = '0%'; return; }
      var w = Math.max(8, (cw / sw) * 100);
      var l = (scrollEl.scrollLeft / (sw - cw)) * (100 - w);
      thumb.style.width = w + '%';
      thumb.style.left = l + '%';
    }
    scrollEl.addEventListener('scroll', update);
    update();
    return update;
  }

  function bindVProgress(scrollEl, barEl) {
    if (!scrollEl || !barEl) return;
    var thumb = barEl.querySelector('.vp-thumb');
    function update() {
      var sh = scrollEl.scrollHeight;
      var ch = scrollEl.clientHeight;
      if (sh <= ch) { thumb.style.height = '100%'; thumb.style.top = '0%'; return; }
      var h = Math.max(8, (ch / sh) * 100);
      var t = (scrollEl.scrollTop / (sh - ch)) * (100 - h);
      thumb.style.height = h + '%';
      thumb.style.top = t + '%';
    }
    scrollEl.addEventListener('scroll', update);
    update();
    return update;
  }

  /* ==========================================================
   * 9. 圆主页渲染
   * ========================================================== */

  /** 构建父圆内的小圆（最多 3 个槽位 + 「+N」角标；数量越多小圆越小） */
  function innerKidsHtml(nb, kids) {
    var html = '';
    var show = Math.min(3, kids.length);
    var conf = INNER_SLOT[Math.max(1, show)];
    for (var i = 0; i < show; i++) {
      var k = kids[i];
      var pos = conf.pos[i];
      html += '<div class="child-circle" data-child-id="' + k.id + '" data-parent="' + nb.id + '" ' +
              'style="left:' + pos[0] + '%;top:' + pos[1] + '%;width:' + conf.size + '%;height:' + conf.size + '%;' +
              'background:' + k.color + '">' +
              '<div class="cc-title" style="color:' + textColorFor(k.color) + '">' +
              esc(k.title || '无标题') + '</div></div>';
    }
    if (kids.length > 3) {
      html += '<div class="more-badge">+' + (kids.length - 3) + '</div>';
    }
    return html;
  }

  /** 构建单个圆的 DOM */
  function buildCircleEl(nb, pos) {
    var el = document.createElement('div');
    el.className = 'circle';
    el.dataset.id = String(nb.id);
    el.style.left = (pos.cx - pos.r) + 'px';
    el.style.top = (pos.cy - pos.r) + 'px';
    el.style.width = (pos.r * 2) + 'px';
    el.style.height = (pos.r * 2) + 'px';
    el.style.background = nb.color;

    var kids = childrenOf(nb.id);
    if (kids.length) el.classList.add('kids');

    var inkOnBg = textColorFor(nb.color);
    var html = '';

    /* 圆内文本：大标题（加粗）+ 预览小字（可截断）；颜色随背景明度自动切换 */
    var hasText = !!(nb.title || nb.content);
    if (hasText) {
      html += '<div class="circle-text">';
      if (nb.title) html += '<div class="c-title" style="color:' + inkOnBg + '">' + esc(nb.title) + '</div>';
      if (nb.content) {
        var firstLine = String(nb.content).split('\n').filter(function (l) { return l.trim(); })[0] || '';
        html += '<div class="c-preview" style="color:' + inkOnBg + '">' +
                esc(firstLine.replace(/^\s*-\s*/, '')) + '</div>';
      }
      html += '</div>';
    } else {
      html += '<div class="c-plus">＋</div>';
    }

    if (kids.length) html += innerKidsHtml(nb, kids);

    el.innerHTML = html;
    return el;
  }

  function renderRing() {
    var list = roots();
    var width = ringScroll.clientWidth || CFG.refWidth;
    var res = layoutCircles(list.map(function (n) { return n.id; }), width);
    var positions = res.positions;

    var inner = document.createElement('div');
    inner.className = 'ring-inner';
    inner.style.position = 'relative';
    /* 画布比屏幕宽 / 高 —— 屏幕只是画布的一个局部视口，可左右 + 上下滑动看全 */
    inner.style.width = res.canvasW + 'px';
    inner.style.height = Math.max(res.canvasH, ringScroll.clientHeight || 0) + 'px';
    for (var i = 0; i < list.length; i++) {
      inner.appendChild(buildCircleEl(list[i], positions[i]));
    }
    ringScroll.innerHTML = '';
    ringScroll.appendChild(inner);
  }

  /** 右下角灰圆：新建一个默认大小的圆，并带出现动画（缩放淡入 / 轻微弹入） */
  function addCircleByFab() {
    var nb = createNotebook('', '', null);
    renderRing();
    renderCatalog();
    var el = ringInner() ? ringInner().querySelector('.circle[data-id="' + nb.id + '"]') : null;
    if (el) {
      el.classList.add('appear');            /* 触发 CSS 出现动画 */
      /* 把新圆滚到视口中间（画布是二维的，左右 / 上下都要挪） */
      ringScroll.scrollTop = Math.max(0, el.offsetTop + el.offsetHeight / 2 - ringScroll.clientHeight / 2);
      ringScroll.scrollLeft = Math.max(0, el.offsetLeft + el.offsetWidth / 2 - ringScroll.clientWidth / 2);
    }
    toast('已新建一个圆');
    return nb;
  }

  if (ringFab) {
    ringFab.addEventListener('click', function () { addCircleByFab(); });
  }

  /* 「？」深蓝小圆（仅 Web；手机端不同步）：直达「目录 → 产品介绍」引导界面 */
  var ringHelp = $('ringHelp');
  if (ringHelp) {
    ringHelp.addEventListener('click', function () { openGuide(); });
  }

  /* ---------------- 圆主页顶部搜索（对齐 App HomeScreen 的搜索框 + HomeSearchResults） ----------------
   * App 首页顶部只有一条搜索框（没有任何「长按提示」说明行），输入即筛：
   * 标题或正文**包含**关键词即命中（忽略大小写）；空输入收起结果层；
   * 无命中显示「没有找到包含“X”的笔记」；点某行 → 清空输入并进详情页。 */
  var ringSearchEl = $('ringSearch');
  var ringResultsEl = $('ringResults');

  /** 收起并清空结果层 */
  function hideRingResults() {
    if (!ringResultsEl) return;
    ringResultsEl.classList.remove('open');
    ringResultsEl.innerHTML = '';
  }

  /** 全量筛选（只筛未删除的笔记，与 App 的 `all.filter{...}` 一致） */
  function ringSearchHits(q) {
    var kw = String(q || '').trim().toLowerCase();
    if (!kw) return [];
    return allNotebooks().filter(function (n) {
      return String(n.title || '').toLowerCase().indexOf(kw) >= 0 ||
             String(n.content || '').toLowerCase().indexOf(kw) >= 0;
    });
  }

  /** 按输入框当前值重绘结果层 */
  function renderRingResults() {
    if (!ringResultsEl || !ringSearchEl) return;
    var q = ringSearchEl.value;
    if (!String(q || '').trim()) { hideRingResults(); return; }
    var hits = ringSearchHits(q);
    if (!hits.length) {
      ringResultsEl.innerHTML = '<div class="rs-empty">没有找到包含“' + esc(q) + '”的笔记</div>';
      ringResultsEl.classList.add('open');
      return;
    }
    var html = '';
    for (var i = 0; i < hits.length; i++) {
      var n = hits[i];
      /* 副标题 = 正文首行非空行（同 App 的 content.lineSequence().firstOrNull { it.isNotBlank() }） */
      var firstLine = String(n.content || '').split('\n').filter(function (l) { return l.trim(); })[0] || '';
      html += '<div class="rs-row" data-id="' + n.id + '">' +
                '<span class="rs-dot" style="background:' + n.color + '"></span>' +
                '<div class="rs-main">' +
                  '<div class="rs-title">' + esc(n.title) + '</div>' +
                  '<div class="rs-sub">' + esc(firstLine.replace(/^\s*-\s*/, '')) + '</div>' +
                '</div>' +
                '<span class="rs-arrow">›</span>' +
              '</div>';
    }
    ringResultsEl.innerHTML = html;
    ringResultsEl.classList.add('open');
  }

  if (ringSearchEl) ringSearchEl.addEventListener('input', renderRingResults);
  if (ringResultsEl) {
    ringResultsEl.addEventListener('click', function (e) {
      var row = e.target.closest ? e.target.closest('.rs-row') : null;
      if (!row) return;
      var hitId = parseInt(row.dataset.id, 10);
      if (ringSearchEl) ringSearchEl.value = '';   /* 同 App：进详情前清空查询词 */
      hideRingResults();
      openDetail(hitId);
    });
  }

  /* ==========================================================
   * 10. 长按提起 + 拖拽（Pointer Events，鼠标 / 触摸通用）
   *    · 长按大圆 1.2 秒 → 提起，拖到目标圆上松手建立包含（成为子圆后变小）
   *    · 长按大圆内的小圆 1.2 秒 → 单独提起该小圆，
   *      拖到大圆外松手 → 小圆成为独立圆（解除嵌套，落到 0.21W 的中间档）
   * ========================================================== */
  var press = null;   /* {id, el, isChild, parentId, startX, startY, timer, lifted, moved} */
  var drag = null;    /* {id, sourceIsChild, fromParentId, r, grabDX, grabDY, cx, cy, originCX, originCY,
                          dragged, clone, sourceEl, targetEl, delEl, delCX, delCY, delHitR} */

  /* 拖拽删除的「上膛」阈值：拎起后被拖圆心至少移动这么多 px，删除圆才生效。
     原因：删除圆贴在右下角 FAB 上方，而画布最下方的大圆本来就可能与它重叠——
     若不加这个门槛，用户只是长按看看、原地松手就会把笔记删掉。
     必须真的「拖到删除圆上」才算数（需求原文），所以门槛取 6px 这种极小的值即可。 */
  var DELETE_ARM_PX = 6;

  function ringInner() {
    return ringScroll.firstElementChild;
  }

  function onRingPointerDown(e) {
    if (e.button !== undefined && e.button !== 0 && e.pointerType === 'mouse') return;
    /* 防重复启动：若上一次 press 的计时器还在，先清掉，保证长按严格 1.2 秒 */
    if (press && press.timer) { clearTimeout(press.timer); press = null; }

    /* 优先命中大圆内套的小圆（小圆可单独拎出） */
    var childEl = e.target.closest ? e.target.closest('.child-circle') : null;
    var el = e.target.closest ? e.target.closest('.circle') : null;
    var target = null;
    var isChild = false;
    var id = 0;
    var parentId = null;

    if (childEl && ringScroll.contains(childEl)) {
      target = childEl;
      isChild = true;
      id = parseInt(childEl.dataset.childId, 10);
      parentId = parseInt(childEl.dataset.parent, 10);
    } else if (el && ringScroll.contains(el)) {
      target = el;
      id = parseInt(el.dataset.id, 10);
    } else {
      return;
    }

    var p = {
      id: id,
      el: target,
      isChild: isChild,
      parentId: parentId,
      startX: e.clientX,
      startY: e.clientY,
      moved: false,
      lifted: false,
      timer: null
    };
    press = p;
    p.timer = setTimeout(function () {
      if (press !== p) return;   /* press 已被替换 / 清除 → 放弃本次提起 */
      p.lifted = true;
      startLift(p.el, p.id, e.clientX, e.clientY, p.isChild, p.parentId);
    }, CFG.longPressMs);
  }

  /** 进入「提起」态：克隆到拖拽层，scale 1.03 + ±2° 摇摆（CSS 动画）+ 轻微阴影。
   *   注意：clone 的位置只由 left / top 决定 —— transform 全部让给 .lifting 的
   *   liftWobble 关键帧（动画会覆盖 inline transform，混用会让 clone 跟不上手指）。 */
  function startLift(el, id, clientX, clientY, isChild, parentId) {
    /* 防御：上一次拖拽若未正常收尾，先清干净，避免拖拽层残留 clone / 删除圆 */
    if (drag) cleanupDrag();

    var rect = el.getBoundingClientRect();
    var r = rect.width / 2;
    var cx = rect.left + r;
    var cy = rect.top + r;

    var nb = byId(id);
    var bg = nb ? nb.color : SURFACE_FALLBACK;
    var clone = document.createElement('div');
    clone.className = 'circle lifting';
    clone.style.left = '0px';
    clone.style.top = '0px';
    clone.style.width = (r * 2) + 'px';
    clone.style.height = (r * 2) + 'px';
    clone.style.background = bg;
    var inkOnBg = textColorFor(bg);
    var t = nb ? (nb.title || '无标题') : '';
    clone.innerHTML = '<div class="circle-text"><div class="c-title" style="color:' + inkOnBg + ';font-size:' +
                      (r > 30 ? 13 : 10) + 'px">' + esc(t) + '</div></div>';
    dragLayer.appendChild(clone);

    /* 「删除」目标圆：紧贴右下角「新建」FAB 正上方、间隔 10px、水平与 FAB 对齐。
       数量与尺寸全部从 FAB 的真实矩形推出 —— 演示页在窄高屏上会给 .phone-wrap 加
       transform:scale()，只写死 58 会比 FAB 视觉尺寸大一截，所以这里按比例跟 FAB 走。 */
    var fabRect = ringFab ? ringFab.getBoundingClientRect() : null;
    var delD = (fabRect && fabRect.width > 0) ? fabRect.width : 58;
    var delR = delD / 2;
    var delCX = fabRect ? fabRect.left + fabRect.width / 2 : (window.innerWidth - 16 - delR);
    var delCY = fabRect ? fabRect.top - 10 - delR : (window.innerHeight - 16 - delD - 10 - delR);
    var delEl = document.createElement('div');
    delEl.className = 'delete-target';
    delEl.textContent = '删除';
    delEl.style.width = delD + 'px';
    delEl.style.height = delD + 'px';
    /* 字号与直径同比（58px 直径 → 12px），同 .delete-target 的基准值 */
    delEl.style.fontSize = Math.max(10, Math.round(delD * 12 / 58)) + 'px';
    delEl.style.left = (delCX - delR) + 'px';
    delEl.style.top = (delCY - delR) + 'px';
    dragLayer.appendChild(delEl);

    el.classList.add('is-lifted');

    drag = {
      id: id,
      sourceIsChild: !!isChild,
      fromParentId: (parentId === undefined ? null : parentId),
      r: r,
      grabDX: clientX - cx,
      grabDY: clientY - cy,
      cx: cx,
      cy: cy,
      originCX: cx,
      originCY: cy,
      dragged: false,
      clone: clone,
      sourceEl: el,
      targetEl: null,
      delEl: delEl,
      delCX: delCX,
      delCY: delCY,
      /* 命中半径比圆略宽容（+8，与手机端一致） */
      delHitR: delR + 8
    };
    moveClone(clientX, clientY);
  }

  function moveClone(clientX, clientY) {
    if (!drag) return;
    drag.cx = clientX - drag.grabDX;
    drag.cy = clientY - drag.grabDY;
    /* 只用 left / top 定位：缩放 + 摇摆全交给 .lifting 的 liftWobble 动画，
       写 inline transform 会被运行中的动画覆盖，clone 就追不上指针了 */
    drag.clone.style.left = (drag.cx - drag.r) + 'px';
    drag.clone.style.top = (drag.cy - drag.r) + 'px';

    /* 真的拖动过（离开提起位置超过 DELETE_ARM_PX）才允许删除，见 DELETE_ARM_PX 注释 */
    if (!drag.dragged &&
        Math.sqrt(Math.pow(drag.cx - drag.originCX, 2) + Math.pow(drag.cy - drag.originCY, 2)) >
          DELETE_ARM_PX) {
      drag.dragged = true;
    }

    /* 悬停在「删除」圆上时：高亮删除圆，并压制「建立包含」的目标高亮——
       删除优先级高于合并，两种反馈不能同时亮。 */
    var overDel = isOverDelete();
    if (drag.delEl) drag.delEl.classList.toggle('hover', overDel);

    var hit = overDel ? null : findHitTarget();
    if (drag.targetEl && drag.targetEl !== hit) drag.targetEl.classList.remove('is-target');
    if (hit) hit.classList.add('is-target');
    drag.targetEl = hit;
  }

  /** 被拖圆的圆心是否进入「删除」目标圆的命中半径（未拖动过则一律不算命中） */
  function isOverDelete() {
    if (!drag || !drag.delEl || !drag.dragged) return false;
    var d = Math.sqrt(
      Math.pow(drag.cx - drag.delCX, 2) + Math.pow(drag.cy - drag.delCY, 2)
    );
    return d <= drag.delHitR;
  }

  function removeDeleteTarget() {
    if (drag && drag.delEl && drag.delEl.parentNode) {
      drag.delEl.parentNode.removeChild(drag.delEl);
    }
    if (drag) drag.delEl = null;
  }

  /** 命中检测：提起圆中心与目标圆中心距离 < 目标圆半径 */
  function findHitTarget() {
    if (!drag) return null;
    var circles = ringInner() ? ringInner().querySelectorAll('.circle') : [];
    var best = null;
    var bestD = Infinity;
    for (var i = 0; i < circles.length; i++) {
      var el = circles[i];
      var id = parseInt(el.dataset.id, 10);
      if (id === drag.id) continue;                       /* 自身 */
      if (isDescendant(drag.id, id)) continue;            /* 不能包含自己的祖先（避免成环） */
      if (isDescendant(id, drag.id)) continue;            /* 不能包含进自己的子孙（避免成环） */
      var rect = el.getBoundingClientRect();
      if (rect.width === 0) continue;
      var tr = rect.width / 2;
      var tcx = rect.left + tr;
      var tcy = rect.top + tr;
      var d = Math.sqrt(Math.pow(drag.cx - tcx, 2) + Math.pow(drag.cy - tcy, 2));
      if (d < tr && d < bestD) { bestD = d; best = el; }
    }
    return best;
  }

  function onWindowPointerMove(e) {
    if (!press) return;
    var dx = e.clientX - press.startX;
    var dy = e.clientY - press.startY;

    if (!press.lifted) {
      /* 长按未满 1.2s 且移动超过阈值 → 视为滚动，放弃提起 */
      if (Math.sqrt(dx * dx + dy * dy) > CFG.moveTol) {
        clearTimeout(press.timer);
        press.moved = true;
      }
      return;
    }
    if (!drag) return;
    if (e.cancelable) e.preventDefault();
    moveClone(e.clientX, e.clientY);
    autoScroll(e.clientY);
  }

  /** 拖到容器上下边缘时自动滚动，便于在长列表中跨屏嵌套 */
  function autoScroll(clientY) {
    var rect = ringScroll.getBoundingClientRect();
    var edge = 64;
    if (clientY < rect.top + edge) ringScroll.scrollTop -= 10;
    else if (clientY > rect.bottom - edge) ringScroll.scrollTop += 10;
  }

  function onWindowPointerUp() {
    if (!press) return;
    clearTimeout(press.timer);

    if (press.lifted && drag) {
      endDrag();
    } else if (!press.moved) {
      /* 普通点击 → 打开详情页（小圆也有自己的一整页） */
      openDetail(press.id);
    }
    press = null;
  }

  function onWindowPointerCancel() {
    if (!press) return;
    clearTimeout(press.timer);
    if (press.lifted && drag) endDrag();
    press = null;
  }

  /** 松手：命中删除圆 → 删除；命中目标圆 → 建立包含；小圆未命中 → 拎出独立；其余 → 回弹原位 */
  function endDrag() {
    /* ① 删除优先：拖到「删除」圆上松手 → 该圆连同全部子孙**软删除**（放进回收站，可恢复）。
       必须排在 findHitTarget / merge 分支之前，否则右下角附近若同时压到某个圆
       会走「建立包含」而不是删除。 */
    if (isOverDelete()) {
      var deleteId = drag.id;
      var trashed = trashSubtree(deleteId);
      cleanupDrag();
      renderRing();
      renderCatalog();
      if (trashed > 0) toast('已删除 · 可在目录页的回收站里恢复');
      return;
    }

    var target = findHitTarget();
    var src = drag.sourceEl;
    var clone = drag.clone;
    var liftedId = drag.id;
    var sourceIsChild = drag.sourceIsChild;
    var fromParentId = drag.fromParentId;

    if (target) {
      var targetId = parseInt(target.dataset.id, 10);
      if (sourceIsChild && targetId === fromParentId) {
        /* 拖回自己的父圆 → 不变化，静默回弹 */
        reboundClone(src, clone);
        return;
      }
      if (childrenOf(targetId).length >= CFG.maxChildren) {
        toast('已达 ' + CFG.maxChildren + ' 条上限');
      } else if (isDescendant(targetId, liftedId)) {
        toast('不能把父圆包含进它自己的子');
      } else {
        setParent(liftedId, targetId);
      }
      cleanupDrag();
      renderRing();
      var newEl = ringInner() ? ringInner().querySelector('.circle[data-id="' + targetId + '"]') : null;
      if (newEl && byId(liftedId) && byId(liftedId).parentId === targetId) {
        newEl.classList.add('flash');
        toast('已包含');
        setTimeout(function () { newEl.classList.remove('flash'); }, 650);
      }
      return;
    }

    /* 小圆拖到大圆外松手 → 解除嵌套，成为独立圆（走 0.21W 的中间档，不是普通大圆） */
    if (sourceIsChild && fromParentId !== null) {
      setParent(liftedId, null);
      var detachedNb = byId(liftedId);
      if (detachedNb) detachedNb.detached = true;   /* 标记拎出档位，radiusOf 据此取 0.21W */
      cleanupDrag();
      renderRing();
      renderCatalog();
      toast('已拎出，成为独立圆');
      return;
    }

    reboundClone(src, clone);
  }

  /** 未命中 → 回弹原位（同样只动 left / top，transform 回 1） */
  function reboundClone(src, clone) {
    removeDeleteTarget();          /* 「删除」圆随松手立即消失 */
    var rect = src.getBoundingClientRect();
    clone.classList.remove('lifting');   /* 先撤掉摇摆动画，否则 transform 不受控 */
    clone.classList.add('rebound');
    clone.style.left = rect.left + 'px';
    clone.style.top = rect.top + 'px';
    clone.style.transform = 'scale(1)';
    setTimeout(function () {
      if (clone.parentNode) clone.parentNode.removeChild(clone);
      if (src) src.classList.remove('is-lifted');
    }, 240);
    drag = null;
  }

  /** 立即清除拖拽态 */
  function cleanupDrag() {
    if (drag && drag.clone && drag.clone.parentNode) {
      drag.clone.parentNode.removeChild(drag.clone);
    }
    if (drag && drag.sourceEl) drag.sourceEl.classList.remove('is-lifted');
    if (drag && drag.targetEl) drag.targetEl.classList.remove('is-target');
    removeDeleteTarget();
    drag = null;
  }

  ringScroll.addEventListener('pointerdown', onRingPointerDown);
  window.addEventListener('pointermove', onWindowPointerMove, { passive: false });
  window.addEventListener('pointerup', onWindowPointerUp);
  window.addEventListener('pointercancel', onWindowPointerCancel);
  window.addEventListener('dragstart', function (e) { if (drag) e.preventDefault(); });

  /* ==========================================================
   * 11. 新建页 / 详情页共用的编辑器面板
   *     · 详情页与新建页必须是同一套（标题输入 + 富文本区 + 子圆竖条 + 底部工具条），
   *       这样从大圆 → 子圆 → 子子圆一路下钻时，编辑体验完全一致；
   *     · 富文本 HTML 写回 notebook.rich，content 保存纯文本镜像供圆主页 / 目录页预览用。
   * ========================================================== */

  /** 富文本区初始内容：优先 rich（富文本），没有则用 content 的纯文本镜像 */
  function richInitialHtml(nb) {
    if (!nb) return '';
    if (nb.rich) return nb.rich;
    return esc(nb.content || '').replace(/\n/g, '<br>');
  }

  /** 编辑器面板 HTML（新建页 / 详情页共用同一份，保证两处体验一致） */
  function editorPaneHtml(nbId, opts) {
    opts = opts || {};
    var nb = byId(nbId) || quickDraft();
    return '' +
      '<input class="ed-title-input" data-role="title" data-id="' + nbId + '" type="text" ' +
             'value="' + esc(nb.title) + '" placeholder="标题" />' +
      '<div class="rich-editor" data-role="rich" data-id="' + nbId + '" contenteditable="true" ' +
           'data-placeholder="记录点什么…">' + richInitialHtml(nb) + '</div>' +
      stripsHtml(nbId) +
      richToolbarHtml();
  }

  /** 当前正在编辑的富文本区：详情页打开时在详情页，否则在新建页 */
  function currentRichEl() {
    if (detailLayer.classList.contains('open')) {
      return detailLayer.querySelector('[data-role="rich"]');
    }
    return viewNew.querySelector('[data-role="rich"]');
  }

  /** 当前编辑器面板的根节点（用于同步工具条上的颜色图标等 UI） */
  function currentPaneRoot() {
    return detailLayer.classList.contains('open') ? detailLayer : viewNew;
  }

  /** 绑定编辑器面板事件：标题 / 富文本 / 撤回重做 / 工具条 */
  function bindEditorPane(root, nbId) {
    var titleEl = root.querySelector('[data-role="title"]');
    if (titleEl) {
      titleEl.addEventListener('input', function () {
        /* 新建页首次输入时才懒创建父圆，避免空转创建一堆空圆 */
        var id = parseInt(titleEl.dataset.id, 10);
        if (!id || !byId(id)) {
          id = ensureQuickParent();
          titleEl.dataset.id = String(id);
        }
        var nb = byId(id);
        if (!nb) return;
        nb.title = titleEl.value;
        nb.updatedAt = Date.now();
        renderRing();
        renderCatalog();
      });
    }

    var editor = root.querySelector('[data-role="rich"]');
    if (editor) {
      editor.addEventListener('input', function () { saveSelection(); syncRichFromDom(editor); });
      editor.addEventListener('keyup', saveSelection);
      editor.addEventListener('mouseup', saveSelection);
      /* 触屏：手指抬起时补存一次选区，否则点色卡 / 弹窗后可能拿不到光标位置（同 editor.js 的 touchend） */
      editor.addEventListener('touchend', saveSelection);
    }

    var undoBtn = root.querySelector('[data-role="undo"]');
    var redoBtn = root.querySelector('[data-role="redo"]');
    if (undoBtn) undoBtn.addEventListener('click', function () { onToolbarCmd('undo', undoBtn); });
    if (redoBtn) redoBtn.addEventListener('click', function () { onToolbarCmd('redo', redoBtn); });

    /* 顶栏「保存」：正文本来就是随打随存，这里做的是「立即写回数据层 + 成功反馈」（同原生 flushSave + toast）。
       空笔记（标题和正文都空）时不落库：新建页的父圆是「首次输入才懒创建」的，
       无条件 sync 会让用户点一下保存就凭空多出一个「无标题」空圆。 */
    var saveBtn = root.querySelector('[data-role="save"]');
    if (saveBtn) {
      saveBtn.addEventListener('click', function () {
        var ed = currentRichEl();
        var titleEl = currentPaneRoot().querySelector('[data-role="title"]');
        var hasText = !!ed && String(ed.innerText || '').trim() !== '';
        var hasTitle = !!titleEl && String(titleEl.value || '').trim() !== '';
        if (!hasText && !hasTitle) { toast('还没有内容'); return; }
        syncRichFromDom(ed);
        toast('保存成功');
      });
    }

    bindRichToolbar(root);
  }

  /** 工具条绑定：mousedown 阻止默认行为，避免点击按钮时编辑器丢失选区 */
  function bindRichToolbar(root) {
    var toolbar = root.querySelector('[data-role="toolbar"]');
    if (!toolbar) return;
    toolbar.addEventListener('mousedown', function (e) {
      if (e.target.closest && e.target.closest('.rt-btn')) e.preventDefault();
    });
    var btns = toolbar.querySelectorAll('.rt-btn');
    for (var i = 0; i < btns.length; i++) {
      (function (btn) {
        btn.addEventListener('click', function () { onToolbarCmd(btn.dataset.cmd, btn); });
      })(btns[i]);
    }
  }

  /* ==========================================================
   * 12. 竖条（小板块）区
   *     「＋」新建紧跟在已建子圆的后边（与 App 的 renderStrips 同结构：
   *     真实子圆 + 一个淡紫虚线「＋」，没有占位块、没有进度条）。
   *     新建页 / 详情页中该板块始终固定在屏幕下方（见 style.css .pane）。
   * ========================================================== */

  function stripsHtml(parentId) {
    var kids = childrenOf(parentId);
    var full = kids.length >= CFG.maxChildren;

    var html = '<div class="strips-wrap">' +
               '<div class="strips-title">子 ' + kids.length + ' / ' + CFG.maxChildren + '</div>' +
               '<div class="strips" data-parent="' + parentId + '">';

    for (var i = 0; i < kids.length; i++) {
      var k = kids[i];
      var first = String(k.content || '').split('\n').filter(function (l) { return l.trim(); })[0] || '';
      var inkOnBg = textColorFor(k.color);
      html += '<div class="strip" data-id="' + k.id + '" style="background:' + k.color + '">' +
                '<div class="s-title" style="color:' + inkOnBg + '">' + esc(k.title || '无标题') + '</div>' +
                '<div class="s-preview" style="color:' + inkOnBg + '">' + esc(first.replace(/^\s*-\s*/, '')) + '</div>' +
                '<div class="s-idx" style="color:' + inkOnBg + '">#' + (i + 1) + '</div>' +
              '</div>';
    }

    /* 「＋」新建竖条：紧跟在已建子圆后边，永远收到竖条区末尾 */
    html += '<div class="strip-new' + (full ? ' disabled' : '') + '" data-role="add-child" ' +
            'data-parent="' + parentId + '">＋</div>';

    html += '</div></div>';
    return html;
  }

  /** 绑定竖条区事件：＋ 新建子圆、点击进子圆整页 */
  function bindStripsEvents(root) {
    var addBtns = root.querySelectorAll('[data-role="add-child"]');
    for (var i = 0; i < addBtns.length; i++) {
      (function (btn) {
        if (btn.dataset.bound === '1') return;
        btn.dataset.bound = '1';
        btn.addEventListener('click', function () {
          var pid = parseInt(btn.dataset.parent, 10);
          /* 新建页父圆尚未懒创建时（pid=0），先创建父圆再建子圆 */
          if (!pid) pid = ensureQuickParent();
          addChild(pid);
        });
      })(addBtns[i]);
    }

    var strips = root.querySelectorAll('.strip');
    for (var j = 0; j < strips.length; j++) {
      (function (s) {
        if (s.dataset.bound === '1') return;
        s.dataset.bound = '1';
        s.addEventListener('click', function () {
          var id = parseInt(s.dataset.id, 10);
          if (id) openDetail(id);
        });
      })(strips[j]);
    }
  }

  /** 新建子圆（含 100 条上限校验） */
  function addChild(parentId) {
    if (childrenOf(parentId).length >= CFG.maxChildren) {
      toast('已达 ' + CFG.maxChildren + ' 条上限');
      return null;
    }
    var seq = childrenOf(parentId).length + 1;
    var nb = createNotebook('子 ' + seq, '- 待记录', parentId);
    renderAll();
    toast('已新建子（' + childrenOf(parentId).length + '/' + CFG.maxChildren + '）');
    return nb;
  }

  /* ==========================================================
   * 13. 详情页（与新建页共用同一套编辑器，支持三层嵌套：
   *     大圆 → 子圆 → 子子圆，每一层都是同一套编辑面板，都能再建子圆）
   * ========================================================== */

  function openDetail(id) {
    if (!byId(id)) return;
    if (state.detailStack[state.detailStack.length - 1] !== id) {
      state.detailStack.push(id);
    }
    renderDetail();
  }

  function backDetail() {
    state.detailStack.pop();
    renderDetail();
  }

  function renderDetail() {
    if (!state.detailStack.length) {
      detailLayer.classList.remove('open');
      detailLayer.innerHTML = '';
      navSync();
      return;
    }
    var id = state.detailStack[state.detailStack.length - 1];
    var nb = byId(id);
    if (!nb) { state.detailStack.pop(); renderDetail(); return; }

    /* 头部：← 返回 ＋ 新建 ＋ 撤回 ＋ 重做 ＋ 路径面包屑 ＋ 保存；下方是与新建页完全相同的编辑面板。
       顺序与手机端 editor.html 的 .detail-head 一致（撤回/重做在「新建」之后，路径吃掉剩余宽度把「保存」顶到最右）。 */
    var html = '' +
      '<div class="detail-head">' +
        '<button class="dh-btn" type="button" data-role="back" title="返回">' + headIconHtml('back') + '</button>' +
        '<button class="dh-btn dh-add" type="button" data-role="add-child" data-parent="' + nb.id + '" title="新建子">' + headIconHtml('add') + '</button>' +
        undoRedoHtml() +
        '<div class="dh-path">' + esc(pathOf(nb.id)) + '</div>' +
        headSaveHtml() +
      '</div>' +
      '<div class="pane detail-pane">' + editorPaneHtml(nb.id) + '</div>';

    detailLayer.innerHTML = html;
    detailLayer.classList.add('open');

    var backBtn = detailLayer.querySelector('[data-role="back"]');
    if (backBtn) backBtn.addEventListener('click', backDetail);

    bindEditorPane(detailLayer, nb.id);
    bindStripsEvents(detailLayer);
    navSync();
  }

  /* ==========================================================
   * 14. Tab1 目录页
   * ========================================================== */
  function renderCatalog() {
    var all = allNotebooks();

    /* 最近访问：显示全部（已删的不算，只出现在回收站里），横向排列左右滑动（按更新时间倒序） */
    var recent = allNotebooks()
      .sort(function (a, b) { return b.updatedAt - a.updatedAt; });

    var html = '' +
      /* 22sp Bold「目录」大标题（App CatalogScreen.kt:116-121） */
      '<div class="cat-title">目录</div>' +
      '<div class="sec-title">最近访问</div>' +
      '<div class="recent-row" id="recentRow">';

    for (var i = 0; i < recent.length; i++) {
      var rn = recent[i];
      var inkOnBg = textColorFor(rn.color);
      html += '<div class="recent-card" data-id="' + rn.id + '" style="background:' + rn.color + '">' +
                '<div class="rc-title" style="color:' + inkOnBg + '">' + esc(rn.title || '无标题') + '</div>' +
                '<div class="rc-meta" style="color:' + inkOnBg + '">' +
                  (rn.parentId === null ? '圆' : '子') + ' · ' + childrenOf(rn.id).length + ' 子</div>' +
              '</div>';
    }
    html += '</div>' +
            '<div class="h-progress" id="recentProgress"><div class="hp-thumb"></div></div>' +
            '<div class="sec-title sec-all">全部笔记</div>' +
            '<div class="cat-wrap"><div class="cat-list" id="catList">';

    for (var j = 0; j < all.length; j++) {
      var n = all[j];
      /* 大标题下方灰色小字：子圆显示所属父圆的大标题（路径），根圆留空 */
      var sub = '';
      if (n.parentId !== null) {
        var parent = byId(n.parentId);
        sub = parent ? (parent.title || '无标题') : '';
      }
      html += '<div class="cat-row" data-id="' + n.id + '">' +
                '<div class="cat-dot" style="background:' + n.color + '"></div>' +
                '<div>' +
                  '<div class="cat-name">' + esc(n.title || '无标题') + '</div>' +
                  '<div class="cat-preview">' + esc(sub) + '</div>' +
                '</div>' +
                '<div class="cat-arrow">›</div>' +
              '</div>';
    }
    html += '</div><div class="v-progress" id="catVProgress"><div class="vp-thumb"></div></div></div>';

    /* 底部入口行：与 App CatalogScreen.kt:176-217 同序
       USER → SEARCH → SETTINGS → BOOK（产品介绍）→ TRASH（回收站）→ 可用时间 */
    html += '<div class="cat-bottom">' +
              '<div class="avatar" data-role="login" title="用户">' +
                '<svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="3.4"/>' +
                '<path d="M5.5 19c1.2-3.2 3.6-4.8 6.5-4.8s5.3 1.6 6.5 4.8"/></svg>' +
              '</div>' +
              '<div class="icon-btn" data-role="search" title="搜索">' +
                '<svg viewBox="0 0 24 24"><circle cx="10.5" cy="10.5" r="5.5"/><path d="M14.6 14.6 20 20"/></svg>' +
              '</div>' +
              '<div class="icon-btn" data-role="settings" title="设置">' +
                '<svg viewBox="0 0 24 24"><path d="M4 7h16M4 12h16M4 17h16"/>' +
                '<circle cx="9" cy="7" r="1.7"/><circle cx="15" cy="12" r="1.7"/><circle cx="7" cy="17" r="1.7"/></svg>' +
              '</div>' +
              /* 产品介绍（App 的 BOOK 图标）：摊开的书 —— 上沿 + 书脊折角 + 两侧书脊 + 下沿 + 中缝（11 段线，坐标照抄 CatalogScreen.kt:485-497） */
              '<div class="icon-btn" data-role="book" title="产品介绍">' +
                '<svg viewBox="0 0 24 24">' +
                '<path d="M4.5 5.5 10.5 4.5"/><path d="M10.5 4.5 12 6"/><path d="M12 6 13.5 4.5"/><path d="M13.5 4.5 19.5 5.5"/>' +
                '<path d="M4.5 5.5V18.5"/><path d="M19.5 5.5V18.5"/>' +
                '<path d="M4.5 18.5 10.5 17.5"/><path d="M10.5 17.5 12 19"/><path d="M12 19 13.5 17.5"/><path d="M13.5 17.5 19.5 18.5"/>' +
                '<path d="M12 6V19"/></svg>' +
              '</div>' +
              '<div class="icon-btn" data-role="trash" title="回收站">' +
                /* 回收站图标：桶盖横线 + 提手 + 收口桶身 + 两道竖纹（8 段线，坐标照抄 App CatalogScreen） */
                '<svg viewBox="0 0 24 24"><path d="M6 7h12"/><path d="M10 4.5h4"/><path d="M12 4.5v2.5"/>' +
                '<path d="M7.5 7 9 19.5"/><path d="M16.5 7 15 19.5"/><path d="M9 19.5h6"/>' +
                '<path d="M10.5 10 11.3 17"/><path d="M13.5 10 12.7 17"/></svg>' +
              '</div>' +
              '<div class="cat-time">可用时间：无限</div>' +
            '</div>';

    viewCatalog.innerHTML = html;

    /* 进度条联动 */
    bindHProgress($('recentRow'), $('recentProgress'));
    bindVProgress($('catList'), $('catVProgress'));

    /* 最近卡片：短按打开详情 */
    var cards = viewCatalog.querySelectorAll('.recent-card');
    for (var c = 0; c < cards.length; c++) {
      (function (card) {
        card.addEventListener('click', function () {
          openDetail(parseInt(card.dataset.id, 10));
        });
      })(cards[c]);
    }

    /* 全部笔记行：短按打开详情 / 长按 600ms 弹删除二次确认
       （对应 App CatalogRowItem 的 combinedClickable(onClick, onLongClick)） */
    var rows = viewCatalog.querySelectorAll('.cat-row');
    for (var r = 0; r < rows.length; r++) {
      (function (row) {
        var rowId = parseInt(row.dataset.id, 10);
        bindLongPress(row,
          function () { openCatDelete(rowId); },
          function () { openDetail(rowId); });
      })(rows[r]);
    }

    var login = viewCatalog.querySelector('[data-role="login"]');
    if (login) login.addEventListener('click', function () { toast('登录占位：手机号验证码登录（演示未接入）'); });
    var settings = viewCatalog.querySelector('[data-role="settings"]');
    if (settings) settings.addEventListener('click', openSettings);
    var book = viewCatalog.querySelector('[data-role="book"]');
    if (book) book.addEventListener('click', openGuide);
    var trashBtn = viewCatalog.querySelector('[data-role="trash"]');
    if (trashBtn) trashBtn.addEventListener('click', openTrash);
    var search = viewCatalog.querySelector('[data-role="search"]');
    if (search) search.addEventListener('click', openSearch);
  }

  /* ==========================================================
   * 14.2 设置 / 隐私政策 / 产品介绍 / 长按删除确认
   *   · 设置 + 隐私政策：对应 App ui/settings/SettingsDialog.kt
   *   · 产品介绍：对应 App ui/onboarding/OnboardingScreen.kt（5 页）
   *   · 长按删除：对应 App CatalogScreen.kt:234-262 的 AlertDialog
   *   所有中文文案照抄 App 资源文件（strings_account.xml / strings.xml /
   *   strings_home_catalog.xml），不做二次创作。
   * ========================================================== */

  /** 隐私政策全文：与 app/src/main/res/raw/privacy_policy.txt **逐字同源**。
   *  应用商店审核要求「可读的隐私政策」，所以 Web 演示里必须是真正文，
   *  不能只写「详见 App」。App 侧正文更新时请同步这一份。 */
  var PRIVACY_POLICY_TEXT = `万物笔记 隐私政策

生效日期：2026 年 10 月 28 日
最近更新：2026 年 10 月 29 日
版本：1.2

欢迎使用「万物笔记」。我们深知个人信息对您的重要性，并会尽力保护您的隐私。本政策向您说明：在您使用本应用时，我们如何处理您的信息。请您在使用前仔细阅读。

一、我们的基本原则
「万物笔记」是一款以本地存储为核心的笔记工具。我们遵循"最小必要"与"默认不收集"的原则：本应用不向任何服务器上传您的任何内容——所有笔记与「万物」解算结果都只存在于您的设备上。

二、我们收集的信息
1. 您主动创建的内容（笔记标题、正文、子笔记、图片等）：全部默认仅保存在您设备本地的私有存储空间，不会自动上传到我们的服务器或任何第三方。
2. 设备信息：本应用不收集设备标识符、手机号、通讯录、地理位置等个人信息。
3. 我们没有账号系统，也不要求登录或绑定手机号。您无需提供任何身份信息即可使用全部功能。

三、「万物」功能与离线词典
1. 「万物」页面的"解算"能力完全离线：所有结果均来自随安装包一起发布的本地词典，不联网、不向任何服务器发送任何内容。断网、飞行模式下同样可以正常使用。
2. 该页面上的「开启超算模式」按钮目前仅为预留入口：点击后会提示「功能正在升级中」，不会改变任何设置，也不会发起任何网络请求。
3. 本应用不接入任何第三方大模型接口（包括但不限于 DeepSeek 等），不存在将您输入或点击的词语发送给第三方的行为。
4. 若将来开放需要联网的能力，我们会在开启前先更新本隐私政策并明确告知，由您主动选择是否使用；在此之前，您的一切输入都不离开您的设备。
5. 当前版本中，您在「万物」里输入或点击的任何词语都不会离开您的设备。

四、信息的存储与安全
1. 您的笔记数据存储在设备本地数据库（Android DataStore / Room），仅本应用可读写。
2. 本地笔记数据的保存期限：您的笔记将一直保留在您设备本地，直至您主动删除相应笔记或卸载本应用。我们不存在任何服务器端副本。
3. 我们不会将您的笔记内容用于广告投放、用户画像或任何商业分析。
4. 卸载应用会清除设备上的全部本地数据；如需保留，请使用应用内的导出功能提前备份。

五、信息的共享与转让
我们不会出售、出租或与他人交换您的个人信息。除下列情形外，我们不会向第三方提供您的信息：
1. 获得您的明确同意后；
2. 法律法规规定或司法/行政机关依法定程序要求提供；
3. 为保护本应用及用户的重大合法权益所必需。

六、您的权利
您对自己的信息享有充分的控制权：
1. 查阅与修改：所有内容均可在应用内直接编辑。
2. 导出与备份：可通过"分享/导出"功能将笔记导出为图片或文本文件。
3. 删除：删除单条笔记或卸载应用即可清除相应数据。

七、儿童隐私
本应用不面向未满 14 周岁的儿童，我们也不会主动收集儿童的个人信息。若我们发现误收集了儿童信息，将尽快删除。

八、政策的变更
我们可能不时更新本政策。重大变更会通过应用内公告或更新说明告知您；若您继续使用本应用，即视为接受更新后的政策。

九、联系我们
运营者（处理者）名称：万物AI实验室；联系方式：见下方邮箱与官方公众号。
官方公众号：「万物AI实验室」（微信 ID：WanWu_AI_Lab），您也可通过公众号与我们联系。
如您对本政策或隐私保护有任何疑问，可通过应用内「设置」页面查看联系方式，或发送邮件至 1033364069@qq.com 与我们联系。我们将在合理时间内回复。

感谢您使用「万物笔记」。`;

  /** 统一开关一个 .modal-mask（元素在 index.html 里静态存在，事件在启动时绑一次） */
  function showModal(id, on) {
    var mask = $(id);
    if (!mask) return;
    if (on) mask.classList.add('open'); else mask.classList.remove('open');
    navSync();
  }
  function isModalOpen(id) {
    var mask = $(id);
    return !!mask && mask.classList.contains('open');
  }

  /* ---------------- 设置 → 隐私政策（两级弹窗） ---------------- */
  function openSettings() {
    showModal('settingsModal', true);
  }
  function closeSettings() {
    showModal('settingsModal', false);
  }
  function openPrivacy() {
    var body = $('privacyBody');
    if (body) body.textContent = PRIVACY_POLICY_TEXT;   /* 全文内联写入 */
    showModal('settingsModal', false);
    showModal('privacyModal', true);
  }
  /** 关闭二级只回到设置弹窗，不连关两层（App SettingsDialog 里 showPolicy=false 后仍在设置页） */
  function closePrivacy() {
    showModal('privacyModal', false);
    showModal('settingsModal', true);
  }

  /* ---------------- 产品介绍：5 页引导轮播 ----------------
     文案逐字照抄 strings.xml 的 onboard_p1..p5（含第 4 页的换行广告语）。 */
  var GUIDE_PAGES = [
    { title: '这不仅仅是一个简单的笔记。', desc: '让我们来探索吧！' },
    { title: '长按大圆拖拽建立关联', desc: '长按大圆 >1 秒提起，拖到目标圆重合即建立包含关系。' },
    { title: '长按小圆拖拽取消关联', desc: '长按小圆 >1 秒提起，拖到空白处取消包含关系成独立个体。' },
    { title: '解算万物', desc: '世界很大，从一次解算开始：\n找回起源，接住灵感，看见更多。', core: '万物' },
    { title: '限时免费体验', desc: '本app为公测体验版，无需任何费用' }
  ];
  var guideIdx = 0;

  /* ================= 产品介绍 5 页动态插画（对齐 App OnboardingIllustrations.kt） =================
     逻辑画布固定 300×260（≈ 手机端插画 260dp 高度），CSS 等比缩放到弹窗宽度。
     颜色逐条取自 AppPalette（THEME_PRIMARY #7B5EA7 / THEME_DEEP #4A2E6B / THEME_ACCENT #C9A0DC /
     NEW_CIRCLE_GRAY #B9B9B4 + 16 色卡），不改色值。 */
  var GUIDE_WORDS = ['微积分', '星系', '艺术', '语言', '画笔', '人格', '几何体', '演员', '美食', '半导体', '火箭', '能源', '基因'];
  var PAL = {
    themePrimary: '#7B5EA7',
    themeDeep: '#4A2E6B',
    themeAccent: '#C9A0DC',
    newCircleGray: '#B9B9B4',
    white: '#FFFFFF'
  };
  var NOTEBOOK_COLOR_HEX = [
    '#EC6841', '#F19149', '#F7B551', '#FFF45C', '#B3D465', '#7FC269', '#31B16C', '#12B4B1',
    '#00B7EE', '#448AC9', '#556FB5', '#5F52A0', '#8957A1', '#AD5DA1', '#EA68A2', '#EB6877'
  ];

  function gLerp(a, b, f) { return a + (b - a) * f; }
  function gClamp(v, lo, hi) { return v < lo ? lo : (v > hi ? hi : v); }
  /* 一轮末尾淡出：holdEndMs 之后在 fadeMs 内 1→0（与 App tailFade 同义） */
  function gTailFade(t, holdEndMs, fadeMs) {
    if (t <= holdEndMs) return 1;
    if (t >= holdEndMs + fadeMs) return 0;
    return 1 - (t - holdEndMs) / fadeMs;
  }
  /* CubicBezier 缓动求解（Compose FastOutSlowInEasing = cubic-bezier(0.4,0,0.2,1)） */
  function gCubicBezier(x1, y1, x2, y2) {
    function A(a, b) { return 1 - 3 * b + 3 * a; }
    function B(a, b) { return 3 * b - 6 * a; }
    function C(a) { return 3 * a; }
    function calc(a, b, t) { return ((A(a, b) * t + B(a, b)) * t + C(a)) * t; }
    function slope(a, b, t) { return 3 * A(a, b) * t * t + 2 * B(a, b) * t + C(a); }
    return function (x) {
      if (x <= 0) return 0;
      if (x >= 1) return 1;
      var t = x;
      for (var i = 0; i < 8; i++) {
        var xe = calc(x1, x2, t);
        var dx = xe - x;
        if (Math.abs(dx) < 1e-5) break;
        var d = slope(x1, x2, t);
        if (Math.abs(d) < 1e-6) break;
        t -= dx / d;
      }
      return calc(y1, y2, t);
    };
  }
  var gEase = gCubicBezier(0.4, 0, 0.2, 1);

  /* sRGB 相对亮度（WCAG）→ 圆内文字自动反色，阈值 0.20（同 AppPalette.textOnCircle） */
  function gLin(c) { c /= 255; return c <= 0.04045 ? c / 12.92 : Math.pow((c + 0.055) / 1.055, 2.4); }
  function gLum(hex) {
    var n = parseInt(hex.slice(1), 16);
    return 0.2126 * gLin((n >> 16) & 255) + 0.7152 * gLin((n >> 8) & 255) + 0.0722 * gLin(n & 255);
  }
  function gTextOn(hex) { return gLum(hex) < 0.20 ? '#FFFFFF' : '#1A1A1A'; }

  function gRoundRect(ctx, x, y, w, h, r) {
    ctx.beginPath();
    ctx.moveTo(x + r, y);
    ctx.arcTo(x + w, y, x + w, y + h, r);
    ctx.arcTo(x + w, y + h, x, y + h, r);
    ctx.arcTo(x, y + h, x, y, r);
    ctx.arcTo(x, y, x + w, y, r);
    ctx.closePath();
  }

  var GUIDE_DRAW = {
    /* 第 1 页 · 应用图标（紫大圆 + 深紫中圆 + 粉紫小圆）呼吸 */
    0: function (ctx, elapsed) {
      var cx = 150, cy = 130, k = 62 / 30;
      var T = 8000, tt = elapsed % T, outer = 1, inner = 1;
      if (tt < 2000) { outer = gLerp(1, 1.06, gEase(tt / 2000)); }
      else if (tt < 4000) { outer = 1.06; inner = gLerp(1, 0.94, gEase((tt - 2000) / 2000)); }
      else if (tt < 6000) { outer = gLerp(1.06, 1, gEase((tt - 4000) / 2000)); inner = 0.94; }
      else { inner = gLerp(0.94, 1, gEase((tt - 6000) / 2000)); }
      ctx.fillStyle = PAL.themePrimary;
      ctx.beginPath(); ctx.arc(cx, cy, 30 * k * outer, 0, Math.PI * 2); ctx.fill();
      ctx.fillStyle = PAL.themeDeep;
      ctx.beginPath(); ctx.arc(cx - 9 * k * outer, cy + 9 * k * outer, 12.5 * k * outer * inner, 0, Math.PI * 2); ctx.fill();
      ctx.fillStyle = PAL.themeAccent;
      ctx.beginPath(); ctx.arc(cx + 13 * k * outer, cy - 9 * k * outer, 7.5 * k * outer * inner, 0, Math.PI * 2); ctx.fill();
    },
    /* 第 2 页 · 长按大圆拖拽建立关联（小圆拖入后收缩成 0.34 子圆） */
    1: function (ctx, elapsed) {
      var cx = 150, cy = 130;
      var t = elapsed % (3200 + 200);
      var p = gEase(gClamp(t / 1600, 0, 1));
      var fade = gTailFade(t, 1600 + 1000, 300);
      var bigCx = cx - 48, bigCy = cy, bigR = 54;
      var sX = cx + 80, sY = cy - 10, eX = bigCx + 16, eY = bigCy - 12;
      var childR = gLerp(bigR, bigR * 0.34, p);
      var childX = gLerp(sX, eX, p), childY = gLerp(sY, eY, p);
      var ringA = gClamp((p - 0.55) / 0.45, 0, 1) * 0.55 * fade;
      if (ringA > 0) {
        ctx.globalAlpha = ringA; ctx.strokeStyle = PAL.themeAccent; ctx.lineWidth = 3;
        ctx.beginPath(); ctx.arc(bigCx, bigCy, bigR + 7, 0, Math.PI * 2); ctx.stroke(); ctx.globalAlpha = 1;
      }
      ctx.fillStyle = PAL.themePrimary;
      ctx.beginPath(); ctx.arc(bigCx, bigCy, bigR, 0, Math.PI * 2); ctx.fill();
      ctx.globalAlpha = fade; ctx.fillStyle = PAL.themeAccent;
      ctx.beginPath(); ctx.arc(childX, childY, childR, 0, Math.PI * 2); ctx.fill();
      ctx.globalAlpha = fade * 0.9; ctx.strokeStyle = PAL.themeDeep; ctx.lineWidth = 2;
      ctx.beginPath(); ctx.arc(childX, childY, childR, 0, Math.PI * 2); ctx.stroke();
      ctx.globalAlpha = 1;
    },
    /* 第 3 页 · 长按小圆拖拽取消关联（小圆拖出后长成大圆） */
    2: function (ctx, elapsed) {
      var cx = 150, cy = 130;
      var t = elapsed % (3800 + 200);
      var p = gEase(gClamp((t - 600) / 1600, 0, 1));
      var fade = gTailFade(t, 600 + 1600 + 1000, 300);
      var bigCx = cx - 48, bigCy = cy, bigR = 54;
      var sX = bigCx + 16, sY = bigCy - 12, eX = cx + 80, eY = cy - 10;
      var childR = gLerp(bigR * 0.34, bigR, p);
      var childX = gLerp(sX, eX, p), childY = gLerp(sY, eY, p);
      ctx.fillStyle = PAL.themePrimary;
      ctx.beginPath(); ctx.arc(bigCx, bigCy, bigR, 0, Math.PI * 2); ctx.fill();
      ctx.globalAlpha = fade; ctx.fillStyle = PAL.themeAccent;
      ctx.beginPath(); ctx.arc(childX, childY, childR, 0, Math.PI * 2); ctx.fill();
      ctx.globalAlpha = fade * 0.9; ctx.strokeStyle = PAL.themeDeep; ctx.lineWidth = 2;
      ctx.beginPath(); ctx.arc(childX, childY, childR, 0, Math.PI * 2); ctx.stroke();
      ctx.globalAlpha = 1;
    },
    /* 第 4 页 · 「万物」大圆分裂成 13 个灰线相连的小圆 */
    3: function (ctx, elapsed) {
      var cx = 150, cy = 130;
      var t = elapsed % (4800 + 120);
      var fade = gTailFade(t, 4200, 400);
      var count = GUIDE_WORDS.length;
      var splitAll = gClamp((t - 300) / (80 * count + 400), 0, 1);
      var coreRatio = 1 - 0.12 * splitAll;
      var coreR = 62, smallR = 26, ringR = 102;
      function wordP(i) { return gClamp((t - (300 + i * 80)) / 400, 0, 1); }
      function off(i) { var a = (Math.PI * 2 * i / count) - Math.PI / 2; return [ringR * Math.cos(a), ringR * Math.sin(a)]; }
      /* 相邻小圆的灰线（首尾相接） */
      for (var i = 0; i < count; i++) {
        var pi = gEase(wordP(i)), pj = gEase(wordP((i + 1) % count));
        var al = Math.min(pi, pj) * 0.9 * fade;
        if (al > 0) {
          var a1 = off(i), a2 = off((i + 1) % count);
          ctx.globalAlpha = al; ctx.strokeStyle = PAL.newCircleGray; ctx.lineWidth = 1;
          ctx.beginPath(); ctx.moveTo(cx + a1[0], cy + a1[1]); ctx.lineTo(cx + a2[0], cy + a2[1]); ctx.stroke();
        }
      }
      /* 「万物」大圆（随分裂略微收小） */
      ctx.globalAlpha = fade; ctx.fillStyle = PAL.themePrimary;
      ctx.beginPath(); ctx.arc(cx, cy, coreR * coreRatio, 0, Math.PI * 2); ctx.fill();
      /* 13 个小圆：从圆心分裂出去，边移动边长大 */
      for (var j = 0; j < count; j++) {
        var pp = gEase(wordP(j));
        if (pp > 0) {
          var o = off(j);
          ctx.globalAlpha = pp * fade;
          ctx.fillStyle = NOTEBOOK_COLOR_HEX[j % 16];
          ctx.beginPath(); ctx.arc(cx + o[0] * pp, cy + o[1] * pp, smallR * pp, 0, Math.PI * 2); ctx.fill();
        }
      }
      /* 「万物」两个字 */
      ctx.globalAlpha = fade; ctx.fillStyle = PAL.white;
      ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
      ctx.font = 'bold 19px system-ui,-apple-system,"PingFang SC","Microsoft YaHei",sans-serif';
      ctx.save(); ctx.translate(cx, cy); ctx.scale(coreRatio, coreRatio); ctx.fillText('万物', 0, 0); ctx.restore();
      /* 13 个词（位置/字号与小圆同步） */
      ctx.font = '500 9.5px system-ui,-apple-system,"PingFang SC","Microsoft YaHei",sans-serif';
      for (var m = 0; m < count; m++) {
        var p3 = gEase(wordP(m));
        if (p3 > 0) {
          var oo = off(m), hex = NOTEBOOK_COLOR_HEX[m % 16];
          ctx.globalAlpha = p3 * fade; ctx.fillStyle = gTextOn(hex);
          var wx = cx + oo[0] * p3, wy = cy + oo[1] * p3;
          ctx.save(); ctx.translate(wx, wy); ctx.scale(p3, p3); ctx.fillText(GUIDE_WORDS[m], 0, 0); ctx.restore();
        }
      }
      ctx.globalAlpha = 1; ctx.textAlign = 'start'; ctx.textBaseline = 'alphabetic';
    },
    /* 第 5 页 · 圆上下浮动 + 锁随之开合（+ 微光呼吸） */
    4: function (ctx, elapsed) {
      var cx = 150, cy = 130;
      var FLOAT = 2600, GLOW = 2200;
      var ft = elapsed % (FLOAT * 2);
      var floatY = ft < FLOAT ? gLerp(-6, 6, gEase(ft / FLOAT)) : gLerp(6, -6, gEase((ft - FLOAT) / FLOAT));
      var gt = elapsed % (GLOW * 2);
      var glow = gt < GLOW ? gLerp(0.15, 0.45, gEase(gt / GLOW)) : gLerp(0.45, 0.15, gEase((gt - GLOW) / GLOW));
      var open = gClamp((6 - floatY) / 12, 0, 1);
      var ccx = cx, ccy = cy + floatY;
      /* 柔和微光 */
      ctx.globalAlpha = glow; ctx.fillStyle = PAL.themeAccent;
      ctx.beginPath(); ctx.arc(ccx, ccy, 78, 0, Math.PI * 2); ctx.fill();
      /* 主体圆 */
      ctx.globalAlpha = 1; ctx.fillStyle = PAL.themePrimary;
      ctx.beginPath(); ctx.arc(ccx, ccy, 66, 0, Math.PI * 2); ctx.fill();
      /* 锁身 */
      var bw = 44, bh = 34, bt = ccy - 2;
      ctx.fillStyle = PAL.white; gRoundRect(ctx, ccx - bw / 2, bt, bw, bh, 8); ctx.fill();
      /* 锁梁：上半圆，开启时上移 7dp 并以左腿根部为轴旋转 -8° */
      var ss = 30, sLeft = ccx - ss / 2, sTop = bt - ss + 4;
      ctx.save();
      ctx.translate(0, -7 * open);
      var pivotX = sLeft, pivotY = sTop + ss / 2;
      ctx.translate(pivotX, pivotY); ctx.rotate(-8 * open * Math.PI / 180); ctx.translate(-pivotX, -pivotY);
      ctx.strokeStyle = PAL.white; ctx.lineWidth = 5;
      ctx.beginPath(); ctx.arc(sLeft + ss / 2, sTop + ss / 2, ss / 2, Math.PI, Math.PI * 2, false); ctx.stroke();
      ctx.restore();
    }
  };

  var guideArtState = { el: null, ctx: null, page: 0, raf: 0, t0: 0, running: false };

  function ensureGuideCanvas() {
    if (guideArtState.el) return guideArtState.el;
    var cv = document.getElementById('guideArt');
    if (!cv) return null;
    var dpr = Math.min(2, window.devicePixelRatio || 1);
    cv.width = 300 * dpr;
    cv.height = 260 * dpr;
    var ctx = cv.getContext('2d');
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
    guideArtState.el = cv; guideArtState.ctx = ctx;
    return cv;
  }

  function guideArtFrame(now) {
    if (!guideArtState.running) return;
    var ctx = guideArtState.ctx;
    if (ctx) {
      var el = (now || 0) - guideArtState.t0;
      if (el < 0) el = 0;
      ctx.setTransform(guideArtState.el.width / 300, 0, 0, guideArtState.el.height / 260, 0, 0);
      ctx.globalAlpha = 1;
      ctx.clearRect(0, 0, 300, 260);
      var fn = GUIDE_DRAW[guideArtState.page];
      if (fn) { try { fn(ctx, el); } catch (e) { /* 绘制异常不打断循环 */ } }
    }
    guideArtState.raf = requestAnimationFrame(guideArtFrame);
  }

  /** 切页/打开时调用：更新页码并从 0 重启一轮动画（与手机端「页面重新可见即重播」一致） */
  function startGuideArt() {
    if (!ensureGuideCanvas()) return;
    guideArtState.page = guideIdx;
    guideArtState.t0 = (window.performance && performance.now) ? performance.now() : Date.now();
    if (guideArtState.running) return;
    guideArtState.running = true;
    guideArtState.raf = requestAnimationFrame(guideArtFrame);
  }

  function stopGuideArt() {
    guideArtState.running = false;
    if (guideArtState.raf) { cancelAnimationFrame(guideArtState.raf); guideArtState.raf = 0; }
  }

  function renderGuide() {
    var page = GUIDE_PAGES[guideIdx];
    var body = $('guideBody');
    var dots = $('guideDots');
    if (body) {
      /* 插画改由 canvas 动态绘制（含第 4 页「万物」大圆），这里只放文字 */
      body.innerHTML =
        '<div class="guide-title">' + esc(page.title) + '</div>' +
        '<div class="guide-desc">' + esc(page.desc) + '</div>';
    }
    if (dots) {
      var dh = '';
      for (var i = 0; i < GUIDE_PAGES.length; i++) {
        dh += '<span class="guide-dot' + (i === guideIdx ? ' on' : '') + '"></span>';
      }
      dots.innerHTML = dh;
    }
    /* 与手机端对齐：仅末页（LOCK 插画 / 第 5 页）展示「本版完全免费」徽章（onboard_unlock_badge） */
    var badge = $('guideBadge');
    if (badge) badge.classList.toggle('show', guideIdx === GUIDE_PAGES.length - 1);
    var prev = $('guidePrev');
    var next = $('guideNext');
    if (prev) prev.disabled = (guideIdx === 0);
    /* 与手机端对齐：非末页主按钮「探索更多 ›」（onboard_cta_next + onboard_arrow），末页「开始使用」 */
    if (next) next.textContent = (guideIdx === GUIDE_PAGES.length - 1 ? '开始使用' : '探索更多 ›');
    /* 启动/切换该页动态插画（每页进入时从 0 重播一轮，与手机端一致） */
    startGuideArt();
  }
  function openGuide() {
    guideIdx = 0;
    showModal('guideModal', true);
    renderGuide();
  }
  function closeGuide() {
    stopGuideArt();
    showModal('guideModal', false);
  }
  /** 翻页：最后一页的「开始使用」= 关闭（App 的 onboard_cta_start） */
  function guideStep(delta) {
    var next = guideIdx + delta;
    if (next < 0) return;
    if (next >= GUIDE_PAGES.length) { closeGuide(); return; }
    guideIdx = next;
    renderGuide();
  }

  /* ---------------- 「全部笔记」长按删除 + 二次确认 ---------------- */
  var catDeleteCb = null;

  /** 长按（600ms；位移超过 12px 视为滑动，取消长按）+ 短按，用于目录页「全部笔记」行。
   *  长按触发后紧跟的那次 click 会被吞掉，避免「删除确认」后面再弹出详情页。 */
  function bindLongPress(el, onLongPress, onTap) {
    var timer = null, startX = 0, startY = 0, fired = false;
    function cancelTimer() {
      if (timer) { clearTimeout(timer); timer = null; }
    }
    el.addEventListener('pointerdown', function (e) {
      if (typeof e.button === 'number' && e.button !== 0) return;
      fired = false;
      startX = e.clientX;
      startY = e.clientY;
      cancelTimer();
      timer = setTimeout(function () {
        timer = null;
        fired = true;
        onLongPress();
      }, CFG.catLongPressMs);
    });
    el.addEventListener('pointermove', function (e) {
      if (!timer) return;
      if (Math.abs(e.clientX - startX) > 12 || Math.abs(e.clientY - startY) > 12) cancelTimer();
    });
    el.addEventListener('pointerup', cancelTimer);
    el.addEventListener('pointercancel', cancelTimer);
    el.addEventListener('pointerleave', cancelTimer);
    el.addEventListener('click', function (e) {
      if (fired) {                 /* 长按已处理这次交互：只吞掉 click，不再打开详情 */
        fired = false;
        e.preventDefault();
        e.stopPropagation();
        return;
      }
      onTap();
    });
  }

  function openCatDelete(id) {
    var nb = byId(id);
    var mask = $('catDeleteModal');
    if (!nb || !mask) return;
    var text = $('catDeleteText');
    if (text) {
      /* catalog_delete_message：删除「%s」？删除后会放进回收站，可在目录的回收站里恢复。 */
      text.textContent = '删除「' + (nb.title || '无标题') + '」？删除后会放进回收站，可在目录的回收站里恢复。';
    }
    catDeleteCb = function () {
      trashSubtree(id);          /* 软删除进回收站，不写物理删除 */
      renderCatalog();
      renderRing();
      toast('已删除 · 可在目录页的回收站里恢复');
    };
    showModal('catDeleteModal', true);
  }
  function closeCatDelete(keepCb) {
    showModal('catDeleteModal', false);
    if (!keepCb) catDeleteCb = null;
  }

  /** 启动时绑一次目录页四个弹窗的按钮（弹窗 DOM 在 index.html 里是静态的） */
  function bindCatalogDialogs() {
    var s = $('settingsModal');
    if (s) {
      var sPrivacy = s.querySelector('[data-role="privacy"]');
      if (sPrivacy) sPrivacy.addEventListener('click', openPrivacy);
      var sClose = s.querySelector('[data-role="close"]');
      if (sClose) sClose.addEventListener('click', closeSettings);
    }
    var p = $('privacyModal');
    if (p) {
      var pClose = p.querySelector('[data-role="close"]');
      if (pClose) pClose.addEventListener('click', closePrivacy);
    }
    var g = $('guideModal');
    if (g) {
      var gPrev = g.querySelector('[data-role="prev"]');
      if (gPrev) gPrev.addEventListener('click', function () { guideStep(-1); });
      var gNext = g.querySelector('[data-role="next"]');
      if (gNext) gNext.addEventListener('click', function () { guideStep(1); });
      var gSkip = g.querySelector('[data-role="skip"]');
      if (gSkip) gSkip.addEventListener('click', closeGuide);
    }
    var d = $('catDeleteModal');
    if (d) {
      var dCancel = d.querySelector('[data-role="cancel"]');
      if (dCancel) dCancel.addEventListener('click', function () { closeCatDelete(); });
      var dOk = d.querySelector('[data-role="ok"]');
      if (dOk) dOk.addEventListener('click', function () {
        var cb = catDeleteCb;
        closeCatDelete();          /* 先关弹窗再执行（cb 已被取出） */
        if (cb) cb();
      });
    }
  }

  /* ---------------- 返回语义：与 App 系统返回键三级优先级一致 ----------------
   * 关弹窗 → 关搜索/回收站 → 退编辑栈 → 回「圆」Tab，绝不退出页面。
   * 做法：只要有覆盖层打开就压一条 history 守卫（popstate 时才拦得住浏览器返回）；
   * 只在**非 iframe** 里启用，避免污染宿主页面的历史记录。 */
  var navGuard = { pushed: false, enabled: false };

  function navAnyOverlayOpen() {
    /* 除覆盖层外，非「圆」Tab 也算一层：App 的返回键此时回「圆」Tab，而不是退出 */
    return state.tab !== 'ring' ||
           state.detailStack.length > 0 || state.trashOpen || state.searchOpen ||
           isModalOpen('settingsModal') || isModalOpen('privacyModal') ||
           isModalOpen('guideModal') || isModalOpen('catDeleteModal');
  }
  function navSync() {
    if (!navGuard.enabled) return;
    if (navAnyOverlayOpen() && !navGuard.pushed) {
      try {
        history.pushState({ wanwuDemo: 1 }, '');
        navGuard.pushed = true;
      } catch (err) {
        navGuard.enabled = false;
      }
    }
  }
  function navBack() {
    if (isModalOpen('privacyModal')) { closePrivacy(); return true; }
    if (isModalOpen('settingsModal')) { closeSettings(); return true; }
    if (isModalOpen('guideModal')) { closeGuide(); return true; }
    if (isModalOpen('catDeleteModal')) { closeCatDelete(); return true; }
    if (state.searchOpen) { closeSearch(); return true; }
    if (state.trashOpen) { closeTrash(); renderRing(); renderCatalog(); return true; }
    if (state.detailStack.length) { backDetail(); return true; }
    if (state.tab !== 'ring') { setTab('ring'); return true; }
    return false;
  }
  function bindBackGesture() {
    var framed = true;
    try { framed = (window.top !== window.self); } catch (err) { framed = true; }
    if (framed || !window.history || !history.pushState) return;
    navGuard.enabled = true;
    window.addEventListener('popstate', function () {
      navGuard.pushed = false;      /* 守卫被浏览器消耗掉了 */
      if (navBack()) navSync();     /* 还有别的层没关完 → 重新挂一层守卫 */
    });
  }

  /* ==========================================================
   * 14.5 回收站覆盖层（对应 App TrashScreen / TrashViewModel）
   *   · 与 App 一致：**不是第 5 个 Tab** —— 从目录页图标进入的独立覆盖层，
   *     顶条原地切换两态：未选中「返回 / 回收站 / 清空」，已选 N 项「× / 已选 N 项 / 恢复 / 删除」
   *   · 短按与长按都切换选中（App 是 combinedClickable 两个回调都绑 onToggle，
   *     故意这样 —— 长按不灵敏时短按可补救）
   * ========================================================== */

  /** 打开回收站：覆盖层不依赖 state.tab，任何 tab 下都能正常显示 */
  function openTrash() {
    state.trashOpen = true;
    state.trashSelected = {};
    if (!trashLayer) return;
    trashLayer.style.display = 'block';
    renderTrash();
    navSync();
  }

  function closeTrash() {
    state.trashOpen = false;
    state.trashSelected = {};
    if (!trashLayer) return;
    trashLayer.style.display = 'none';
    trashLayer.innerHTML = '';
    navSync();
  }

  /** 选中集 {id: true} → id 数组 */
  function trashSelectedIds() {
    var out = [];
    for (var k in state.trashSelected) {
      if (state.trashSelected[k]) out.push(parseInt(k, 10));
    }
    return out;
  }

  /** 彻底删除二次确认：「删除」与「清空」共用同一个弹窗（不用 window.confirm，风格与 App AlertDialog 一致） */
  var purgeConfirmCb = null;
  function openPurgeConfirm(count, onConfirm) {
    var mask = $('trashModal');
    if (!mask) { if (onConfirm) onConfirm(); return; }
    var text = mask.querySelector('#trashModalText');
    if (text) text.textContent = '共 ' + count + ' 项，删除后无法恢复。';
    purgeConfirmCb = onConfirm;
    mask.querySelector('[data-role="cancel"]').onclick = function () {
      mask.classList.remove('open');
      purgeConfirmCb = null;
    };
    mask.querySelector('[data-role="ok"]').onclick = function () {
      mask.classList.remove('open');
      var cb = purgeConfirmCb;
      purgeConfirmCb = null;
      if (cb) cb();
    };
    mask.classList.add('open');
  }

  function renderTrash() {
    if (!trashLayer) return;
    var items = trashItems();
    var sel = trashSelectedIds();
    var n = sel.length;
    /* 重绘后保持滚动位置（多条目时选中不跳回顶部） */
    var oldBody = trashLayer.querySelector('.trash-body');
    var keepTop = oldBody ? oldBody.scrollTop : 0;

    /* 顶条：两态原地切换，高度固定 56px（见 style.css .trash-topbar / .trash-bar-btn） */
    var bar;
    if (n === 0) {
      bar = '<button class="trash-bar-btn" type="button" data-role="back">返回</button>' +
            '<div class="trash-bar-title">回收站</div>' +
            '<button class="trash-bar-btn" type="button" data-role="clear">清空</button>';
    } else {
      bar = '<button class="trash-bar-btn" type="button" data-role="unselect">×</button>' +
            '<div class="trash-bar-title sel">已选 ' + n + ' 项</div>' +
            '<button class="trash-bar-btn" type="button" data-role="restore">恢复</button>' +
            '<button class="trash-bar-btn danger" type="button" data-role="purge">删除</button>';
    }

    /* 网格：3 列灰圆（正方形占满格子） */
    var body;
    if (!items.length) {
      body = '<div class="trash-empty">回收站是空的</div>';
    } else {
      body = '<div class="trash-grid">';
      for (var i = 0; i < items.length; i++) {
        var it = items[i];
        var on = !!state.trashSelected[it.id];
        body += '<div class="trash-circle' + (on ? ' sel' : '') + '" data-id="' + it.id + '">' +
                  '<div class="tc-title">' + esc(it.title || '无标题') + '</div>' +
                  (on ? '<div class="tc-check">✓</div>' : '') +
                '</div>';
      }
      body += '</div>';
    }

    trashLayer.innerHTML = '<div class="trash-topbar">' + bar + '</div>' +
                           '<div class="trash-body">' + body + '</div>';
    var newBody = trashLayer.querySelector('.trash-body');
    if (newBody) newBody.scrollTop = keepTop;

    var back = trashLayer.querySelector('[data-role="back"]');
    if (back) back.addEventListener('click', function () {
      /* 返回前重绘圆 / 目录：恢复过的数据要立刻回到原位 */
      closeTrash();
      renderRing();
      renderCatalog();
    });

    var unselect = trashLayer.querySelector('[data-role="unselect"]');
    if (unselect) unselect.addEventListener('click', function () {
      state.trashSelected = {};
      renderTrash();
    });

    var clear = trashLayer.querySelector('[data-role="clear"]');
    if (clear) clear.addEventListener('click', function () {
      var count = trashItems().length;      /* N 取回收站条目数（每项可能含整棵子树） */
      if (!count) return;
      openPurgeConfirm(count, function () {
        purgeAll();
        state.trashSelected = {};
        renderTrash();
        renderRing();
        renderCatalog();
        toast('回收站已清空');
      });
    });

    var restoreBtn = trashLayer.querySelector('[data-role="restore"]');
    if (restoreBtn) restoreBtn.addEventListener('click', function () {
      var ids = trashSelectedIds();
      for (var r = 0; r < ids.length; r++) restore(ids[r]);
      state.trashSelected = {};
      renderTrash();
      renderRing();
      renderCatalog();
      toast('已恢复');
    });

    var purgeBtn = trashLayer.querySelector('[data-role="purge"]');
    if (purgeBtn) purgeBtn.addEventListener('click', function () {
      var ids = trashSelectedIds();
      if (!ids.length) return;
      openPurgeConfirm(ids.length, function () {
        for (var p = 0; p < ids.length; p++) purge(ids[p]);
        state.trashSelected = {};
        renderTrash();
        renderRing();
        renderCatalog();
        toast('已彻底删除');
      });
    });

    /* 圆：短按 / 长按都切换选中（App 也是两个回调都绑 onToggle） */
    var cells = trashLayer.querySelectorAll('.trash-circle');
    for (var c = 0; c < cells.length; c++) {
      (function (cell) {
        cell.addEventListener('click', function () {
          var id = parseInt(cell.dataset.id, 10);
          if (state.trashSelected[id]) delete state.trashSelected[id];
          else state.trashSelected[id] = true;
          renderTrash();
        });
      })(cells[c]);
    }
  }

  /* ==========================================================
   * 15. 搜索面板（实时匹配标题 / 内容，点击进入笔记）
   * ========================================================== */
  function openSearch() {
    state.searchOpen = true;
    renderSearch('');
    searchPanel.classList.add('open');
    var input = searchPanel.querySelector('.search-input');
    if (input) setTimeout(function () { input.focus(); }, 60);
    navSync();
  }

  function closeSearch() {
    state.searchOpen = false;
    searchPanel.classList.remove('open');
    navSync();
  }

  function renderSearch(kw) {
    kw = String(kw || '').trim();
    var results = kw ? state.notebooks.filter(function (n) {
      if (n.deletedAt) return false;           /* 回收站里的笔记不参与搜索 */
      return (n.title && n.title.indexOf(kw) >= 0) ||
             (n.content && n.content.indexOf(kw) >= 0);
    }) : [];

    var html = '' +
      '<div class="search-head">' +
        '<input class="search-input" type="text" placeholder="搜索笔记标题或内容…" value="' + esc(kw) + '" />' +
        '<button class="dh-btn" type="button" data-role="search-close">×</button>' +
      '</div>' +
      '<div class="search-results">';

    if (!kw) {
      html += '<div class="search-empty">输入关键字，实时列出包含它的笔记</div>';
    } else if (!results.length) {
      html += '<div class="search-empty">没有找到包含「' + esc(kw) + '」的笔记</div>';
    } else {
      for (var i = 0; i < results.length; i++) {
        var n = results[i];
        var first = String(n.content || '').split('\n').filter(function (l) { return l.trim(); })[0] || '';
        html += '<div class="cat-row" data-id="' + n.id + '">' +
                  '<div class="cat-dot" style="background:' + n.color + '"></div>' +
                  '<div>' +
                    '<div class="cat-name">' + esc(n.title || '无标题') + '</div>' +
                    '<div class="cat-preview">' + esc(first.replace(/^\s*-\s*/, '')) + '</div>' +
                  '</div>' +
                  '<div class="cat-arrow">›</div>' +
                '</div>';
      }
    }
    html += '</div>';
    searchPanel.innerHTML = html;

    var input = searchPanel.querySelector('.search-input');
    input.addEventListener('input', function () {
      /* 保留光标位置：仅刷新结果区 */
      var kwNow = input.value;
      var resultsHtml = renderSearchResultsOnly(kwNow);
      searchPanel.querySelector('.search-results').innerHTML = resultsHtml;
      bindSearchResults();
    });

    var closeBtn = searchPanel.querySelector('[data-role="search-close"]');
    closeBtn.addEventListener('click', closeSearch);

    bindSearchResults();
  }

  function renderSearchResultsOnly(kw) {
    kw = String(kw || '').trim();
    var results = kw ? state.notebooks.filter(function (n) {
      if (n.deletedAt) return false;           /* 回收站里的笔记不参与搜索 */
      return (n.title && n.title.indexOf(kw) >= 0) ||
             (n.content && n.content.indexOf(kw) >= 0);
    }) : [];
    if (!kw) return '<div class="search-empty">输入关键字，实时列出包含它的笔记</div>';
    if (!results.length) return '<div class="search-empty">没有找到包含「' + esc(kw) + '」的笔记</div>';
    var html = '';
    for (var i = 0; i < results.length; i++) {
      var n = results[i];
      var first = String(n.content || '').split('\n').filter(function (l) { return l.trim(); })[0] || '';
      html += '<div class="cat-row" data-id="' + n.id + '">' +
                '<div class="cat-dot" style="background:' + n.color + '"></div>' +
                '<div>' +
                  '<div class="cat-name">' + esc(n.title || '无标题') + '</div>' +
                  '<div class="cat-preview">' + esc(first.replace(/^\s*-\s*/, '')) + '</div>' +
                '</div>' +
                '<div class="cat-arrow">›</div>' +
              '</div>';
    }
    return html;
  }

  function bindSearchResults() {
    var rows = searchPanel.querySelectorAll('.cat-row');
    for (var i = 0; i < rows.length; i++) {
      (function (row) {
        row.addEventListener('click', function () {
          closeSearch();
          openDetail(parseInt(row.dataset.id, 10));
        });
      })(rows[i]);
    }
  }

  /* ==========================================================
   * 16. 新建页 / 详情页共用的富文本工具条（模板 Tab 已删除）
   * 布局（两处一致）：
   *   顶部行  ：返回（左） + 撤回 / 重做（右，与返回同一行）
   *   中间    ：标题输入 + 富文本编辑区（自适应填充剩余高度）
   *   底部    ：子圆板块（始终固定在屏幕下方） + 其下方的图标工具条
   * ========================================================== */

  /** 确保新建页有一个父圆笔记（懒创建） */
  function ensureQuickParent() {
    if (state.quickParentId && byId(state.quickParentId)) return state.quickParentId;
    state.quickSeq++;
    var nb = createNotebook('', '', null);
    state.quickParentId = nb.id;
    renderRing();
    renderCatalog();
    return nb.id;
  }

  function quickDraft() {
    return { id: 0, title: '', content: '', rich: '', color: PALETTE[2], parentId: null, createdAt: 0 };
  }

  /** 字号循环档位 */
  var FONT_SIZES = [13, 16, 20];
  var fontSizeIdx = 0;

  /** 当前文字颜色（默认取用户色卡第一色，由「颜色」图标弹窗的 16 色色卡切换） */
  var currentTextColor = PALETTE[0];

  /** 保存 / 恢复富文本选区（弹窗操作后仍需插到原光标处） */
  var savedRange = null;
  function saveSelection() {
    var editor = currentRichEl();
    var sel = window.getSelection();
    if (editor && sel.rangeCount && editor.contains(sel.anchorNode)) {
      savedRange = sel.getRangeAt(0).cloneRange();
    }
  }
  /**
   * 恢复选区（与手机端 editor.js 的 restoreSelection 同语义：**不调用 focus()**）。
   *
   * 旧实现第一行就是 editor.focus()：在移动浏览器里对 contenteditable 调 focus() 会弹起键盘，
   * 于是「点一下工具条就蹦键盘 / 键盘被顶掉」——手机端 U25 已按同样思路修掉，这里对齐：
   *   · 有 savedRange 且仍落在当前编辑区内 → 直接 addRange；
   *   · 没有（用户未聚焦过就点工具条）→ 构造「正文末尾」折叠 Range 作为命令落点。
   * execCommand 在存在合法 selection 时即便元素不是 activeElement 也能生效。
   */
  function restoreSelection(editor) {
    editor = editor || currentRichEl();
    if (!editor) return;
    var sel = window.getSelection();
    if (!sel) return;
    if (savedRange && editor.contains(savedRange.startContainer)) {
      sel.removeAllRanges();
      sel.addRange(savedRange);
      return;
    }
    var end = document.createRange();
    end.selectNodeContents(editor);
    end.collapse(false);
    savedRange = end;
    sel.removeAllRanges();
    sel.addRange(end);
  }

  /**
   * 工具条 / 顶栏按钮不抢焦点（手机端 U25 第 1 层）。
   *
   * 只在「编辑区本来就持有焦点」时 preventDefault()：
   *   · 有焦点 → 阻止按钮抢焦点 → 编辑区不失焦 → 键盘保持展开；
   *   · 无焦点 → 不拦，click 照常派发（本来就没键盘，也不该弹）。
   * 用 pointerdown（不是 touchstart）：cancel pointerdown 只取消 mousedown 等兼容事件，
   * click 仍会派发；cancel touchstart 会把整条触摸序列（含合成 click）吞掉，工具条会失灵。
   */
  function isToolTarget(t) {
    if (!t || !t.closest) return false;
    /* 表格行列的 number input 是唯一「真的需要键盘」的输入框，必须保持默认行为 */
    if (t.closest('.modal-row')) return false;
    return !!t.closest('.rt-btn') ||
           !!t.closest('.dh-btn') ||
           !!t.closest('.color-cell') ||
           !!t.closest('.strip') ||
           !!t.closest('.strip-new');
  }

  function bindNoFocusGuard() {
    document.addEventListener('pointerdown', function (e) {
      if (!isToolTarget(e.target)) return;
      var editor = currentRichEl();
      if (editor && document.activeElement === editor) e.preventDefault();
    }, { capture: true, passive: false });
  }

  /** 执行富文本命令后同步回数据层（写 rich + content 纯文本镜像） */
  function syncRichFromDom(editor) {
    if (!editor) return;
    var id = parseInt(editor.dataset.id, 10) || 0;
    var nb = id ? byId(id) : null;
    /* 新建页首次输入时才懒创建父圆，并把编辑器绑到新圆上 */
    if (!nb) {
      var pid = ensureQuickParent();
      nb = byId(pid);
      editor.dataset.id = String(pid);
      var titleEl = currentPaneRoot().querySelector('[data-role="title"]');
      if (titleEl) titleEl.dataset.id = String(pid);
    }
    if (!nb) return;
    nb.rich = editor.innerHTML;
    nb.content = editor.innerText.replace(/\n+$/g, '');
    nb.updatedAt = Date.now();
    renderRing();
    renderCatalog();
  }

  function execRich(cmd, value) {
    var editor = currentRichEl();
    if (!editor) return;
    restoreSelection(editor);
    document.execCommand(cmd, false, value === undefined ? null : value);
    saveSelection();
    syncRichFromDom(editor);
  }

  /** 顶部行：撤回 / 重做。
   *  位置与手机端 `assets/editor/editor.html` 严格同序：紧跟在「返回」「新建」之后、左对齐，
   *  不再像旧版那样用 .nh-spacer 顶到最右（否则两端视觉不一致）。
   *  图标为矢量（24 视口 / 1.8 描边 / 圆头），SVG 路径与 editor.html:25、:28 逐字一致。 */
  function undoRedoHtml() {
    return '<button class="dh-btn dh-hist" type="button" data-role="undo" title="撤回">' +
             '<svg viewBox="0 0 24 24"><path d="M4 9h9.6a5.4 5.4 0 1 1 0 10.8H8"/><path d="M8.5 5 4 9l4.5 4"/></svg>' +
           '</button>' +
           '<button class="dh-btn dh-hist" type="button" data-role="redo" title="重做">' +
             '<svg viewBox="0 0 24 24"><path d="M20 9h-9.6a5.4 5.4 0 1 0 0 10.8H16"/><path d="M15.5 5 20 9l-4.5 4"/></svg>' +
           '</button>';
  }

  /* 顶栏图标片段（与 editor.html 的 .dh-btn 内 svg 逐字同源，避免三处各写一份漂移） */
  function headIconHtml(name) {
    if (name === 'back') {
      return '<svg viewBox="0 0 24 24"><path d="M19 12H6"/><path d="M11 6l-6 6 6 6"/></svg>';
    }
    if (name === 'add') {
      return '<svg viewBox="0 0 24 24"><path d="M12 5v14M5 12h14"/></svg>';
    }
    return '';
  }

  /** 顶栏右侧「保存」：胶囊按钮，与 editor.html:33 同一个 data-role，两个页面共用同一段绑定 */
  function headSaveHtml() {
    return '<button class="dh-btn dh-save" type="button" data-role="save" title="保存">保存</button>';
  }

  /** 底部图标工具条（位于子圆板块下方）：
   *  字号 / 粗体 / 斜体 / 下划线 / 文字颜色（单图标 → 弹 16 色色卡）/ 表格 / 图片 / 绘图。
   *  与手机端 editor.html 保持一致：撤回/重做已上移到顶部行（U22 同时删掉了
   *  圈一/圈二/①② 三个按钮），两端不允许出现差异。 */
  function richToolbarHtml() {
    return '<div class="rich-toolbar bottom" data-role="toolbar">' +
      '<button class="rt-btn" type="button" data-cmd="fontsize" title="字号">A</button>' +
      '<button class="rt-btn" type="button" data-cmd="bold" title="粗体"><b>B</b></button>' +
      '<button class="rt-btn" type="button" data-cmd="italic" title="斜体"><i>I</i></button>' +
      '<button class="rt-btn" type="button" data-cmd="underline" title="下划线"><u>U</u></button>' +
      /* 文字颜色：只有一个色块图标（默认深灰 --ink，同手机端），点击后弹 16 色色卡小窗口 */
      '<button class="rt-btn rt-color" type="button" data-cmd="color" title="文字颜色">' +
        '<span class="rt-color-bar" data-role="color-bar" style="background:var(--ink)"></span>' +
      '</button>' +
      '<span class="rt-sep"></span>' +
      '<button class="rt-btn" type="button" data-cmd="table" title="插入表格">' +
        '<svg viewBox="0 0 24 24"><rect x="3" y="4" width="18" height="16" rx="2"/><path d="M3 10h18M3 15h18M12 4v16"/></svg>' +
      '</button>' +
      '<button class="rt-btn" type="button" data-cmd="image" title="插入图片">' +
        '<svg viewBox="0 0 24 24"><rect x="3" y="4" width="18" height="16" rx="2"/><circle cx="9" cy="10" r="1.6"/><path d="M4.5 18l4.5-5 3.5 3.5 3-3 4 4.5"/></svg>' +
      '</button>' +
      '<button class="rt-btn" type="button" data-cmd="draw" title="绘图">' +
        '<svg viewBox="0 0 24 24"><path d="M4 20l1-4L16.5 4.5a2.1 2.1 0 0 1 3 3L8 19l-4 1z"/></svg>' +
      '</button>' +
    '</div>';
  }

  function renderNew() {
    var pid = (state.quickParentId && byId(state.quickParentId)) ? state.quickParentId : 0;

    /* 顶栏与详情页严格同一套：返回 / 新建子 / 撤回 / 重做 / 路径 / 保存（顺序同 editor.html 的 .detail-head）。
       新建页的路径在尚未懒建父圆（pid=0）时为空串，靠 .dh-path 的 flex:1 把「保存」顶到最右。 */
    var html = '' +
      '<div class="pane new-pane">' +
        '<div class="detail-head">' +
          '<button class="dh-btn" type="button" data-role="back-ring" title="返回圆主页">' + headIconHtml('back') + '</button>' +
          '<button class="dh-btn dh-add" type="button" data-role="add-child" data-parent="' + pid + '" title="新建子">' + headIconHtml('add') + '</button>' +
          undoRedoHtml() +
          '<div class="dh-path">' + esc(pid ? pathOf(pid) : '') + '</div>' +
          headSaveHtml() +
        '</div>' +
        editorPaneHtml(pid) +
      '</div>';

    viewNew.innerHTML = html;

    /* 返回按钮 → 跳回「圆」主页（仅新建页有，详情页走自己的 ←） */
    var backBtn = viewNew.querySelector('[data-role="back-ring"]');
    if (backBtn) {
      backBtn.addEventListener('click', function () {
        if (state.detailStack.length) { state.detailStack = []; renderDetail(); }
        setTab('ring');
      });
    }

    bindEditorPane(viewNew, pid);
    bindStripsEvents(viewNew);
  }

  /** 设置文字颜色（16 色用户色卡；同步工具条上的圆形色块图标） */
  function setTextColor(color) {
    currentTextColor = color;
    var bar = currentPaneRoot().querySelector('[data-role="color-bar"]');
    if (bar) bar.style.background = color;
    execRich('foreColor', color);
  }

  /* ---------- 文字颜色弹窗：16 色色卡（4×4 网格） ---------- */
  function openColorModal() {
    var mask = $('colorModal');
    var grid = $('colorGrid');
    var cellHtml = '';
    /* 用 TEXT_PALETTE（第 12 色灰 #8A8A8E / 第 16 色纯黑 #000000），
       不能用 PALETTE —— 那是圆的填充色，App 侧仍是旧的两色。 */
    for (var i = 0; i < TEXT_PALETTE.length; i++) {
      cellHtml += '<button class="color-cell" type="button" data-color="' + TEXT_PALETTE[i] + '" ' +
                  'title="' + TEXT_PALETTE[i] + '" style="background:' + TEXT_PALETTE[i] + '"></button>';
    }
    grid.innerHTML = cellHtml;
    mask.classList.add('open');

    grid.onclick = function (e) {
      var cell = e.target.closest ? e.target.closest('.color-cell') : null;
      if (!cell) return;
      mask.classList.remove('open');
      setTextColor(cell.dataset.color);   /* 关闭后再落色，保证选区还在编辑器里 */
    };
    mask.querySelector('[data-role="cancel"]').onclick = function () { mask.classList.remove('open'); };
  }

  /** 工具条命令分发 */
  function onToolbarCmd(cmd, btn) {
    var editor = currentRichEl();
    if (!editor) return;
    switch (cmd) {
      case 'undo':
        restoreSelection(editor);
        document.execCommand('undo');
        syncRichFromDom(editor);
        break;
      case 'redo':
        restoreSelection(editor);
        document.execCommand('redo');
        syncRichFromDom(editor);
        break;
      case 'fontsize':
        fontSizeIdx = (fontSizeIdx + 1) % FONT_SIZES.length;
        editor.style.fontSize = FONT_SIZES[fontSizeIdx] + 'px';
        btn.classList.toggle('on', fontSizeIdx !== 0);
        toast('字号 ' + FONT_SIZES[fontSizeIdx] + 'px');
        break;
      case 'bold':
        execRich('bold');
        btn.classList.toggle('on', document.queryCommandState('bold'));
        break;
      case 'italic':
        execRich('italic');
        btn.classList.toggle('on', document.queryCommandState('italic'));
        break;
      case 'underline':
        execRich('underline');
        btn.classList.toggle('on', document.queryCommandState('underline'));
        break;
      case 'color':
        /* 单个颜色图标 → 弹 16 色色卡小窗口，选完直接落色 */
        saveSelection();
        openColorModal();
        break;
      case 'table':
        saveSelection();
        openTableModal();
        break;
      case 'image':
        saveSelection();
        openImageModal();
        break;
      case 'draw':
        saveSelection();
        openDrawOverlay();
        break;
      /* U22：圈一 / 圈二 / ①② 三个按钮已从工具条删除，对应分支一并移除（与手机端一致） */
    }
  }

  /* ---------- 插入表格弹窗 ---------- */
  function openTableModal() {
    var mask = $('tableModal');
    mask.classList.add('open');
    var rowsInput = mask.querySelector('[data-role="rows"]');
    var colsInput = mask.querySelector('[data-role="cols"]');
    var okBtn = mask.querySelector('[data-role="ok"]');
    var cancelBtn = mask.querySelector('[data-role="cancel"]');

    function close() { mask.classList.remove('open'); }
    okBtn.onclick = function () {
      var rows = Math.max(1, Math.min(12, parseInt(rowsInput.value, 10) || 2));
      var cols = Math.max(1, Math.min(6, parseInt(colsInput.value, 10) || 2));
      var h = '<table class="nb-table"><tbody>';
      for (var r = 0; r < rows; r++) {
        h += '<tr>';
        for (var c = 0; c < cols; c++) h += '<td>&nbsp;</td>';
        h += '</tr>';
      }
      h += '</tbody></table><p><br></p>';
      close();
      execRich('insertHTML', h);
      toast('已插入 ' + rows + '×' + cols + ' 表格');
    };
    cancelBtn.onclick = close;
  }

  /* ---------- 插入图片（拍照 / 相册） ---------- */
  function openImageModal() {
    var mask = $('imageModal');
    mask.classList.add('open');
    var camBtn = mask.querySelector('[data-role="camera"]');
    var albumBtn = mask.querySelector('[data-role="album"]');
    var cancelBtn = mask.querySelector('[data-role="cancel"]');
    var camInput = $('imgCameraInput');
    var albumInput = $('imgAlbumInput');

    function close() { mask.classList.remove('open'); }
    function onFile(input) {
      return function () {
        var file = input.files && input.files[0];
        input.value = '';
        if (!file) return;
        var reader = new FileReader();
        reader.onload = function () {
          execRich('insertHTML', '<img class="nb-img" src="' + reader.result + '" alt="插图" /><p><br></p>');
          toast('已插入图片');
        };
        reader.readAsDataURL(file);
      };
    }
    camInput.onchange = onFile(camInput);
    albumInput.onchange = onFile(albumInput);
    camBtn.onclick = function () { close(); camInput.click(); };
    albumBtn.onclick = function () { close(); albumInput.click(); };
    cancelBtn.onclick = close;
  }

  /* ==========================================================
   * 17. 绘图（canvas 手写）—— 坐标校准修复
   * ----------------------------------------------------------
   * 旧实现的 bug：canvas.width/height 直接取 CSS 尺寸，且只用
   *   (clientX - rect.left) 当作画布内部坐标 → 一旦 CSS 尺寸 ≠ 内部像素尺寸、
   *   或页面有 CSS transform 缩放、或屏幕是高清屏（devicePixelRatio≠1），
   *   画出来的线就会整体偏离指针。
   * 修复方案（唯一换算入口 canvasPointOf）：
   *   1) 画布内部尺寸 = CSS 布局尺寸 × devicePixelRatio（高清屏不糊、不偏）；
   *   2) 用 getBoundingClientRect() 拿到「视觉矩形」——它天然包含所有祖先
   *      CSS transform 的缩放结果，因此 rect.width 与 canvas.width 的比值
   *      一次性把「CSS 缩放 + DPR + 内部/显示尺寸差」全部吃掉；
   *   3) 指针 clientX/clientY 减去 rect.left/top 得到视觉偏移，再乘以上述比值，
   *      即为画布内部像素坐标。绘制全部使用内部像素坐标（lineWidth 乘 DPR）。
   * ========================================================== */

  /** 绘图坐标换算的调试 / 自测数据（供冒烟测试断言，不影响页面逻辑） */
  var drawDebug = {
    dpr: 1, canvasW: 0, canvasH: 0, cssW: 0, cssH: 0,
    rectW: 0, rectH: 0, rectLeft: 0, rectTop: 0,
    px: 0, py: 0, strokes: 0, scaleX: 1, scaleY: 1
  };

  /**
   * 指针事件坐标 → canvas 内部像素坐标（唯一换算入口）。
   * @param {HTMLCanvasElement} canvas 目标画布
   * @param {number} clientX PointerEvent.clientX
   * @param {number} clientY PointerEvent.clientY
   * @returns {{px:number, py:number, rect:ClientRect, scaleX:number, scaleY:number}}
   */
  function canvasPointOf(canvas, clientX, clientY) {
    var rect = canvas.getBoundingClientRect();
    /* rect 为「视觉矩形」：已包含所有祖先 CSS transform 的缩放 */
    var scaleX = rect.width ? canvas.width / rect.width : 1;
    var scaleY = rect.height ? canvas.height / rect.height : 1;
    return {
      px: (clientX - rect.left) * scaleX,
      py: (clientY - rect.top) * scaleY,
      rect: rect,
      scaleX: scaleX,
      scaleY: scaleY
    };
  }

  /** 按容器布局尺寸 + devicePixelRatio 重建画布（尺寸变化时不丢失已画内容） */
  function resizeDrawCanvas(canvas, wrap) {
    var dpr = window.devicePixelRatio || 1;
    var cssW = Math.max(1, Math.round(wrap.clientWidth));
    var cssH = Math.max(1, Math.round(wrap.clientHeight));
    var needW = Math.max(1, Math.round(cssW * dpr));
    var needH = Math.max(1, Math.round(cssH * dpr));
    if (canvas.width === needW && canvas.height === needH) {
      drawDebug.dpr = dpr;
      drawDebug.cssW = cssW;
      drawDebug.cssH = cssH;
      return canvas.getContext('2d');
    }

    /* 保存旧内容（画布尺寸一变就会被清空） */
    var old = null;
    if (canvas.width > 0 && canvas.height > 0) {
      old = document.createElement('canvas');
      old.width = canvas.width;
      old.height = canvas.height;
      old.getContext('2d').drawImage(canvas, 0, 0);
    }

    canvas.width = needW;
    canvas.height = needH;
    canvas.style.width = cssW + 'px';
    canvas.style.height = cssH + 'px';

    var ctx = canvas.getContext('2d');
    ctx.setTransform(1, 0, 0, 1, 0, 0);   /* 不做额外缩放：所有绘制都用内部像素坐标 */
    ctx.lineWidth = 2.5 * dpr;            /* 线宽随 DPR 放大，视觉粗细一致 */
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    ctx.strokeStyle = INK;
    if (old) ctx.drawImage(old, 0, 0);

    drawDebug.dpr = dpr;
    drawDebug.cssW = cssW;
    drawDebug.cssH = cssH;
    drawDebug.canvasW = needW;
    drawDebug.canvasH = needH;
    return ctx;
  }

  var drawResizeHandler = null;

  function openDrawOverlay() {
    var overlay = $('drawOverlay');
    overlay.classList.add('open');
    var canvas = overlay.querySelector('canvas');
    var wrap = overlay.querySelector('.draw-canvas-wrap');
    var doneBtn = overlay.querySelector('[data-role="done"]');
    var clearBtn = overlay.querySelector('[data-role="clear"]');
    var cancelBtn = overlay.querySelector('[data-role="cancel"]');

    var ctx = null;
    var drawing = false;
    var lastX = 0, lastY = 0;

    /** 记录一次落点（同时写入调试数据，便于自动化验证） */
    function markPoint(p) {
      drawDebug.px = p.px;
      drawDebug.py = p.py;
      drawDebug.rectW = p.rect.width;
      drawDebug.rectH = p.rect.height;
      drawDebug.rectLeft = p.rect.left;
      drawDebug.rectTop = p.rect.top;
      drawDebug.scaleX = p.scaleX;
      drawDebug.scaleY = p.scaleY;
      drawDebug.canvasW = canvas.width;
      drawDebug.canvasH = canvas.height;
      drawDebug.cssW = parseFloat(canvas.style.width) || p.rect.width;
      drawDebug.cssH = parseFloat(canvas.style.height) || p.rect.height;
    }

    function beginStroke(e) {
      ctx = resizeDrawCanvas(canvas, wrap);
      var p = canvasPointOf(canvas, e.clientX, e.clientY);
      markPoint(p);
      drawing = true;
      lastX = p.px;
      lastY = p.py;
      /* 起笔点也画一个圆点，保证「点一下」也有反馈 */
      ctx.beginPath();
      ctx.arc(p.px, p.py, ctx.lineWidth / 2, 0, Math.PI * 2);
      ctx.fillStyle = INK;
      ctx.fill();
      try { canvas.setPointerCapture(e.pointerId); } catch (err) { /* 合成事件无真实指针，忽略 */ }
    }

    function moveStroke(e) {
      if (!drawing) return;
      var p = canvasPointOf(canvas, e.clientX, e.clientY);
      markPoint(p);
      ctx.beginPath();
      ctx.moveTo(lastX, lastY);
      ctx.lineTo(p.px, p.py);
      ctx.stroke();
      drawDebug.strokes++;
      lastX = p.px;
      lastY = p.py;
    }

    function endStroke() { drawing = false; }

    /* 等覆盖层显示后再量尺寸（此时容器已有实际布局尺寸） */
    setTimeout(function () {
      ctx = resizeDrawCanvas(canvas, wrap);
      canvas.onpointerdown = beginStroke;
      canvas.onpointermove = moveStroke;
      canvas.onpointerup = endStroke;
      canvas.onpointercancel = endStroke;
    }, 30);

    /* 覆盖层打开期间，容器尺寸 / DPR 变化时重建画布并保留笔迹 */
    if (drawResizeHandler) window.removeEventListener('resize', drawResizeHandler);
    drawResizeHandler = function () {
      if (!overlay.classList.contains('open')) return;
      ctx = resizeDrawCanvas(canvas, wrap);
    };
    window.addEventListener('resize', drawResizeHandler);

    function close() {
      overlay.classList.remove('open');
      canvas.onpointerdown = null;
      canvas.onpointermove = null;
      canvas.onpointerup = null;
      canvas.onpointercancel = null;
      if (drawResizeHandler) {
        window.removeEventListener('resize', drawResizeHandler);
        drawResizeHandler = null;
      }
    }
    clearBtn.onclick = function () {
      ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height);
    };
    cancelBtn.onclick = close;
    doneBtn.onclick = function () {
      var dataUrl = canvas.toDataURL('image/png');
      close();
      execRich('insertHTML', '<img class="nb-img" src="' + dataUrl + '" alt="手写笔记" /><p><br></p>');
      toast('已插入手写笔记');
    };
  }

  /* ==========================================================
   * 18. Tab「万物」页（原「无限」页，Tab 图标为压扁的黑体「万」字）
   * ----------------------------------------------------------
   * 页面从上到下：万物大圆（居中、主题紫）→ 搜索栏（输入词语 + 「解算」）→
   *   「探索」按钮（恒等于「解算万物」，忽略输入框）→「开启超算模式」（只改名，不联网）→
   *   结果区（细分圆自然排列，可纵向滚动）。
   *   点击结果圆 = 用该圆词语再解算一次，继续往下钻；长按结果圆 = 弹词条简介。
   *   **本页完全离线**：只读 data.js 分类树 + wanwu-dict.js 离线词典，不发任何请求。
   * ========================================================== */

  /* 预建 id → 节点索引，避免每次递归查找 */
  var inspIndex = {};
  (function buildInspIndex() {
    function walk(node) {
      inspIndex[node.id] = node;
      (node.children || []).forEach(walk);
    }
    if (window.INSPIRATION_TREE) walk(window.INSPIRATION_TREE);
  })();

  /* 「万物」词条 id 前缀：`w:苹果` 表示这个节点不在分类树里，而是临时从 App 离线词典
     （demo/wanwu-dict.js，与手机端 assets/wanwu 同一份数据）包装出来的。
     这样手机端能解算的词，网页端一定也能解算，不用再维护第二份小词表。 */
  var WANWU_ID_PREFIX = 'w:';

  /** 把词典里的一个词包装成与分类树节点同形的结构：{id, title, children} */
  function wanwuNode(word) {
    var dict = window.WANWU_DICT;
    var kids = dict && Object.prototype.hasOwnProperty.call(dict, word) ? dict[word] : null;
    if (!kids || !kids.length) return null;
    return {
      id: WANWU_ID_PREFIX + word,
      title: word,
      children: kids.map(function (c) {
        return { id: WANWU_ID_PREFIX + c, title: c };
      })
    };
  }

  function findInspNode(id) {
    if (typeof id === 'string' && id.indexOf(WANWU_ID_PREFIX) === 0) {
      return wanwuNode(id.slice(WANWU_ID_PREFIX.length));
    }
    return inspIndex[id] || null;
  }

  /** 数据层抽象：异步取某节点的子节点。
   *  默认读本地 data.js 分类树；接 LLM 时打开开关并改 fetchChildrenRemote 即可。 */
  async function fetchChildren(nodeId) {
    if (INSPIRATION_USE_REMOTE) return fetchChildrenRemote(nodeId);
    var node = findInspNode(nodeId);
    return node ? (node.children || []) : [];
  }

  /** 预留联网接口（默认关闭）：
   *  实现示例（取消注释并配置 INSPIRATION_API_URL 即可）：
   *    var resp = await fetch(INSPIRATION_API_URL + '?node=' + encodeURIComponent(nodeId));
   *    var data = await resp.json();
   *    return data.children; // [{id, title}, ...]
   *  当前未配置远程服务时回落本地数据，保证演示不中断。 */
  async function fetchChildrenRemote(nodeId) {
    var node = findInspNode(nodeId);
    return node ? (node.children || []) : [];
  }

  /** 取根节点到某节点的 id 路径（供返回面包屑使用） */
  function findInspPath(id) {
    function dfs(node, path) {
      var p = path.concat([node.id]);
      if (node.id === id) return p;
      var kids = node.children || [];
      for (var i = 0; i < kids.length; i++) {
        var r = dfs(kids[i], p);
        if (r) return r;
      }
      return null;
    }
    if (!window.INSPIRATION_TREE) return [];
    return dfs(window.INSPIRATION_TREE, []) || [];
  }

  function inspPathTitles() {
    return state.inspPath.map(function (id) {
      var n = findInspNode(id);
      return n ? n.title : id;
    });
  }

  /** 在分类树中搜索：精确匹配优先，其次包含匹配，同级取最浅层级 */
  function searchInspNode(kw) {
    kw = String(kw || '').trim();
    if (!kw || !window.INSPIRATION_TREE) return null;
    var flat = [];
    (function walk(node, depth) {
      if (depth > 0) flat.push({ node: node, depth: depth });
      (node.children || []).forEach(function (c) { walk(c, depth + 1); });
    })(window.INSPIRATION_TREE, 0);

    var exact = [], contains = [];
    flat.forEach(function (it) {
      var t = it.node.title || '';
      if (t === kw) exact.push(it);
      else if (t.indexOf(kw) >= 0) contains.push(it);
    });
    var pool = exact.length ? exact : contains;
    if (!pool.length) return searchWanwuNode(kw);
    pool.sort(function (a, b) { return a.depth - b.depth; });
    return pool[0].node;
  }

  /** 分类树里没有的词，回落到 App 离线词典（1100+ 词）：先精确、再包含匹配 */
  function searchWanwuNode(kw) {
    var dict = window.WANWU_DICT;
    if (!dict) return null;
    if (Object.prototype.hasOwnProperty.call(dict, kw)) return wanwuNode(kw);
    var keys = Object.keys(dict);
    for (var i = 0; i < keys.length; i++) {
      if (keys[i].indexOf(kw) >= 0) return wanwuNode(keys[i]);
    }
    return null;
  }

  /** 由命中节点解算出的细分圆：**只取下一层**（一屏一层）。
   *
   *  以前这里会把「下下层」也混进同一屏（标 level 1），但 App 每一屏只显示同一层级的圆
   *  （`InspireViewModel.buildItems(word, words, level)`，level 即栈深），两端观感不一致，
   *  用户会看到网页端一屏更碎、数量更多 → 去掉下下层，层级只影响直径档位（见 paintInspNodes）。 */
  function fissionFrom(node) {
    var kids = node.children || [];
    return kids.map(function (c) { return { id: c.id, title: c.title }; });
  }

  /* 【已停用】旧的环形（辐射）布局：结果区改为 naturalLayout() 的自然排列后不再使用；
     仅保留函数本体与 __demo 导出键，避免外部冒烟脚本直接调用时报错。 */
  function radialLayout(items, W, H) {
    var cx = W / 2;
    var cy = H / 2;
    var base = Math.min(W, H);
    var ringDefs = [
      { r: base * 0.30, cap: 5 },
      { r: base * 0.50, cap: 9 },
      { r: base * 0.66, cap: 14 }
    ];
    var out = [];
    var placed = 0;

    for (var ri = 0; ri < ringDefs.length && placed < items.length; ri++) {
      var def = ringDefs[ri];
      var n = Math.min(def.cap, items.length - placed);
      for (var j = 0; j < n; j++) {
        var it = items[placed];
        var jitter = (hashStr(it.id + '#r' + ri) - 0.5) * 0.24;   /* 确定性抖动，避免死板 */
        var ang = -Math.PI / 2 + (j + 0.5) * (2 * Math.PI / n) + jitter;
        out.push({ item: it, x: cx + Math.cos(ang) * def.r, y: cy + Math.sin(ang) * def.r });
        placed++;
      }
    }
    /* 兜底：超出所有环容量的圆继续放到最外圈 */
    while (placed < items.length) {
      var extra = items[placed];
      var ang2 = hashStr(extra.id + '#x') * Math.PI * 2;
      out.push({ item: extra, x: cx + Math.cos(ang2) * base * 0.72, y: cy + Math.sin(ang2) * base * 0.72 });
      placed++;
    }
    /* 夹在容器内，保证不出血 */
    var pad = 8;
    out.forEach(function (p) {
      var r = p.item.size / 2;
      p.x = Math.min(Math.max(p.x, pad + r), Math.max(pad + r, W - pad - r));
      p.y = Math.min(Math.max(p.y, pad + r), Math.max(pad + r, H - pad - r));
    });
    return out;
  }

  /**
   * 细分圆「自然排列」：按行排布，每行最多 3 个（容器窄时按宽度自适应列数），
   * 行内水平居中、行内垂直居中，行与行之间留 gap ——
   * 圆与圆之间必有相对间隙，绝不叠加、绝不出血；结果多时往下堆，由容器纵向滚动。
   * @param {Array<{id:string,size:number}>} items 待布局圆（size = 直径）
   * @param {number} W 结果区宽
   * @returns {{positions:Array<{item:Object,size:number,x:number,y:number}>, height:number}}
   *          x / y 为圆心坐标，height 为内容总高（供容器撑开滚动用）
   */
  function naturalLayout(items, W) {
    /* 与 App InspireComponents.kt 的常量一一对应（用户真机反馈「每个都太小」后放大的）：
       LAYOUT_MARGIN_DP=18 / LAYOUT_GAP_DP=20 / LAYOUT_MAX_ITEM_DP=112 */
    var margin = 18;      /* 左右 / 上下留白 */
    var gap = 20;         /* 圆与圆之间的最小间隙 */
    var maxItem = 112;    /* 单个圆的最大直径：用于折算这一行最多能放几个 */
    /* 列数：**放得下才用 3 列**（3×112 + 2×20 = 376 的可用宽），否则 2 列 + 滚动。
       刻意不用 floor(可用宽 /(maxItem+gap))：那个式子把行末间隙也算了进去，
       等价于要求 396，会把 412dp 这类刚够 3 列的屏误判成 2 列。见 App columnCount()。
       也绝不为凑 3 列去缩圆或收紧间距 —— 圆的大小是用户的明确诉求。 */
    var cols = ((W - 2 * margin) >= 3 * maxItem + 2 * gap) ? 3 : 2;
    var positions = [];
    var y = margin;
    var i, k;

    for (i = 0; i < items.length; i += cols) {
      var row = items.slice(i, i + cols);
      var rowW = 0, rowH = 0;
      for (k = 0; k < row.length; k++) {
        rowW += row[k].size + (k ? gap : 0);
        if (row[k].size > rowH) rowH = row[k].size;
      }
      /* 行内水平居中；兜底不小于 margin，保证不出血 */
      var x = Math.max(margin, (W - rowW) / 2);
      for (k = 0; k < row.length; k++) {
        positions.push({ item: row[k], size: row[k].size, x: x + row[k].size / 2, y: y + rowH / 2 });
        x += row[k].size + gap;
      }
      y += rowH + gap;
    }
    return { positions: positions, height: Math.max(0, y - gap + margin) };
  }

  /** 搜索栏 HTML：挂在万物大圆下方（输入框 + 「解算」按钮） */
  function inspSearchHtml() {
    return '<div class="insp-search below-circle">' +
             '<input class="insp-input" type="text" data-role="insp-input" ' +
                    'placeholder="输入词语，解算万物" value="' + esc(state.inspQuery) + '" />' +
             '<button class="insp-go" type="button" data-role="insp-go">解算</button>' +
           '</div>';
  }

  /** 万物大圆（展开态缩为 compact）。**只放大圆**，控件已拆到 inspControlsHtml */
  function inspTopHtml(compact) {
    return '<div class="insp-top">' +
             '<div class="insp-core' + (compact ? ' compact' : '') + '" data-role="insp-root" ' +
                  'style="background:' + INSPIRE_CORE_COLOR + ';color:' + INSPIRE_CORE_TEXT + '">万物</div>' +
           '</div>';
  }

  /**
   * 控件块：搜索栏 + 「探索」按钮 + 「开启超算模式」按钮（顺序与 App InspireControls 一致）。
   * 根态由 `.insp.is-root .insp-bottom { margin-top:auto }` 推到底部，**大圆位置固定不动**；
   * 展开态紧跟小圆下方（无 margin-top:auto，正常文档流）。
   */
  function inspControlsHtml() {
    return '<div class="insp-bottom">' +
             inspSearchHtml() +
             '<div class="insp-explore-wrap">' +
               '<button class="insp-explore" type="button" data-role="insp-explore">探索</button>' +
             '</div>' +
             /* 万物页彻底离线：这个按钮只剩名字，点击只提示「功能正在升级中」，不接任何网络 */
             '<div class="insp-net-wrap">' +
               '<button class="insp-net" type="button" data-role="insp-net">开启超算模式</button>' +
             '</div>' +
           '</div>';
  }

  /** 绑定搜索栏 + 探索按钮 + 超算按钮 + 万物圆：回车 / 点击「解算」都触发一次解算 */
  function bindInspControls() {
    var input = viewInspire.querySelector('[data-role="insp-input"]');
    var go = viewInspire.querySelector('[data-role="insp-go"]');
    var explore = viewInspire.querySelector('[data-role="insp-explore"]');
    var net = viewInspire.querySelector('[data-role="insp-net"]');
    if (go) go.addEventListener('click', function () {
      state.inspQuery = input ? input.value : '';
      doInspSearch(state.inspQuery);
    });
    /* 「探索」恒等于「解算万物」，**不看输入框**（App：`explore() = doResolve(WANWU)`）。
       以前有输入时把探索降级成"解算当前词"，与 App 语义不一致，已去掉。 */
    if (explore) explore.addEventListener('click', function () {
      doInspExplore();
    });
    /* 「开启超算模式」：万物完全离线，点了只弹「功能正在升级中」，不改任何状态、不发请求 */
    if (net) net.addEventListener('click', function () {
      toast('功能正在升级中');
    });
    if (input) {
      input.addEventListener('keydown', function (e) {
        if (e.key === 'Enter' || e.keyCode === 13) {
          e.preventDefault();
          state.inspQuery = input.value;
          doInspSearch(input.value);
        }
      });
    }

    /* 万物大圆：纹裂动画后解算出「万物」的第一层分类 */
    var big = viewInspire.querySelector('[data-role="insp-root"]');
    if (big) {
      big.addEventListener('click', function () {
        if (big.classList.contains('crack')) return;
        big.classList.add('crack');
        setTimeout(function () {
          state.inspQuery = '';
          doInspExplore();
        }, 430);
      });
    }
  }

  /** 执行搜索解算：命中节点 → 展开它的下一层细分圆（自然排列）。
   *  文案与 App strings_inspire.xml **逐字对齐**（万物已彻底离线，不再出现"大模型/末级"这类措辞）。 */
  function doInspSearch(kw) {
    kw = String(kw || '').trim();
    state.inspQuery = kw;
    if (!kw) { toast('请输入要解算的词语'); return; }
    var node = searchInspNode(kw);
    if (!node) { toast('解算失败「' + kw + '」'); return; }
    var nodes = fissionFrom(node);
    /* 词在词典里但没有下一层 → 与 App 同一条兜底提示（inspire_toast_no_result） */
    if (!nodes.length) { toast('解算失败「' + node.title + '」'); return; }
    /* 搜索出的词是「第一屏」，层级归零（App：doResolve 时 level = stack.size = 0）。
       路径只放这一个词 —— App 的栈在搜索时是 [该词]，**不含**「万物」这一层，
       所以面包屑是「万物 / 该词」而不是「万物 / 万物 / 该词」。 */
    state.inspLevel = 0;
    state.inspPath = [node.id];
    state.inspCustom = { forId: node.id, nodes: nodes };
    renderInspire();
    toast('已解算「' + node.title + '」');
  }

  /** 「探索」= **恒等于**「解算万物」，忽略输入框内容（App：`explore() = doResolve(WANWU)`） */
  function doInspExplore() {
    fetchChildren(INSPIRE_ROOT_ID).then(function (children) {
      if (!children || !children.length) { toast('解算失败「万物」'); return; }
      state.inspLevel = 0;
      state.inspPath = [INSPIRE_ROOT_ID];
      state.inspCustom = null;
      renderInspire();
    });
  }

  /**
   * 初始态：万物大圆（位置固定，圆心 50%）+ 被推到底部的「搜索栏 + 探索」+ 空结果区。
   * 版式与 App 一致：大圆不动，控件整块下移。
   */
  function renderInspireRoot() {
    viewInspire.innerHTML =
      '<div class="insp is-root">' +
        inspTopHtml(false) +
        inspControlsHtml() +
        '<div class="insp-stage" data-role="insp-stage"></div>' +
      '</div>';
    bindInspControls();
    requestAnimationFrame(syncInspCoreGeometry);
  }

/**
 * 让网页大圆与手机端**完全同步**（同一套比例，任何屏幕都对齐）：
 *   直径 = 可用高 × 36%（封顶 232）、圆心落在可用高的 50%（居中，不再右偏、不再缩小）。
 * 与 App 常量一一对应：InspireScreen.kt 的
 *   ROOT_CORE_HEIGHT_RATIO=0.36 / ROOT_CORE_MAX_DP=232 / ROOT_CENTER_RATIO=0.50 / ROOT_CORE_HORIZONTAL_BIAS_RATIO=0.00（不再右偏）/ TOP_AREA_RESERVED_DP=190。
 */
  function syncInspCoreGeometry() {
    var insp = viewInspire.querySelector('.insp.is-root');
    if (!insp) return;
    var H = insp.clientHeight;
    if (!H) return;
    var W = insp.clientWidth || H;
    var reserved = 190;                                 // ≈ App TOP_AREA_RESERVED_DP：搜索栏 56 + 20/10/8 + 探索 38 + 超算按钮 34 + 底部呼吸 12
    var size = Math.min(H * 0.36, 232);                 // 直径（回归原状，与 App 一致）
    size = Math.min(size, Math.max(H - reserved - 24, 132));
    var top = H * 0.50 - size / 2;                       // 圆心 50%（居中）
    top = Math.max(24, Math.min(top, Math.max(H - size - reserved, 24)));
    var topEl = insp.querySelector('.insp-top');
    var core = insp.querySelector('.insp-core');
    if (topEl) topEl.style.paddingTop = top + 'px';
    if (core && !core.classList.contains('compact')) {
      core.style.width = size + 'px';
      core.style.height = size + 'px';
      /* 回归居中后不再水平右偏（与 App ROOT_CORE_HORIZONTAL_BIAS_RATIO = 0 一致） */
      /* 字号 = 直径 × 0.15，并夹在 26~36px（App ROOT_FONT_MIN_SP / ROOT_FONT_MAX_SP），
         否则矮屏上会缩成 20px 左右，「万物」两个字撑不满圆 */
      core.style.fontSize = Math.min(36, Math.max(26, size * 0.15)) + 'px';
    }
  }

  /* 手机框高度会被 max-height 压缩，窗口尺寸变化时按比例重新摆放大圆 */
  window.addEventListener('resize', function () { syncInspCoreGeometry(); });

  /** 展开态：面包屑 + 万物小圆 + 搜索栏 + 探索按钮 + 结果区（细分圆自然排列） */
  function renderInspireLevel(nodes, currentId) {
    var titles = inspPathTitles();
    viewInspire.innerHTML =
      '<div class="insp">' +
        '<div class="insp-head">' +
          '<button class="dh-btn" type="button" data-role="insp-back">←</button>' +
          '<div class="insp-path">万物 / ' + esc(titles.join(' / ')) + '</div>' +
        '</div>' +
        inspTopHtml(true) +
        inspControlsHtml() +
        '<div class="insp-stage" data-role="insp-stage"><div class="search-empty">解算中…</div></div>' +
      '</div>';
    bindInspControls();

    var backBtn = viewInspire.querySelector('[data-role="insp-back"]');
    if (backBtn) {
      backBtn.addEventListener('click', function () {
        state.inspPath.pop();
        state.inspCustom = null;
        /* 退一层，直径档位也要跟着回上一层（App 的 stack 里每层自带 level） */
        state.inspLevel = Math.max(0, state.inspLevel - 1);
        renderInspire();
      });
    }

    if (nodes) { paintInspNodes(nodes, state.inspLevel); return; }
    fetchChildren(currentId).then(function (children) {
      var stage = viewInspire.querySelector('[data-role="insp-stage"]');
      if (!stage) return;
      if (!children.length) {
        stage.innerHTML = '<div class="search-empty">这个词还没有细分，换个词试试</div>';
        return;
      }
      paintInspNodes(children.map(function (c) { return { id: c.id, title: c.title }; }), state.inspLevel);
    });
  }

  /**
   * 把细分圆按「自然排列」画到结果区（按行排布，最宽 3 个一行，行内居中）。
   *
   * @param list  [{id, title}, ...]
   * @param level 当前屏层级 = 已下钻次数（0 → lv1 大圆 84~108；≥1 → lv2 小一档 68~84 且透明度 .92）
   */
  function paintInspNodes(list, level) {
    var stage = viewInspire.querySelector('[data-role="insp-stage"]');
    if (!stage) return;
    /* App STAGE_MIN_VISIBLE_DP=60：视口比这还矮就干脆不画，否则圆会溢出压到底部控件 / Tab 上 */
    if (stage.clientHeight && stage.clientHeight < 60) { stage.innerHTML = ''; return; }
    var W = stage.clientWidth || CFG.refWidth;
    var lvl = level || 0;

    var items = list.map(function (n, i) {
      /* 直径按 id hash 确定性微调：同一 id 恒定，重绘 / 滚动不抖动。
         区间与 App nodeSizeDp() 一致：lv1 84~108（原 68~88），lv2 68~84（原 56~68）。 */
      var size = lvl === 0 ? 84 + Math.round(hashStr(n.id) * 24)    /* 下一层：84 ~ 108px */
                           : 68 + Math.round(hashStr(n.id) * 16);   /* 更深层：68 ~ 84px */
      return { id: n.id, title: n.title, level: lvl, size: size, color: PALETTE[i % PALETTE.length] };
    });

    var res = naturalLayout(items, W);
    var h = '<div class="insp-nodes" style="height:' + res.height + 'px">';
    for (var i = 0; i < res.positions.length; i++) {
      var p = res.positions[i];
      var it = p.item;
      var r = p.size / 2;
      /* 字号三档随直径走（App NODE_FONT_LARGE/MID/SMALL）；
         原来只有 `size > 70 ? 12 : 10.5` 两档，圆放大到 84~108 后 12px 会被圆吞掉 */
      var fs = p.size >= 100 ? 15 : (p.size >= 84 ? 13.5 : 12);
      h += '<div class="insp-node' + (it.level > 0 ? ' lv2' : '') + '" data-node="' + esc(it.id) + '" ' +
           'style="left:' + (p.x - r) + 'px;top:' + (p.y - r) + 'px;' +
           'width:' + p.size + 'px;height:' + p.size + 'px;' +
           'background:' + it.color + ';color:' + textColorFor(it.color) + ';' +
           'font-size:' + fs + 'px;' +
           'animation-delay:' + (i * 40) + 'ms">' +
           '<span class="insp-node-title">' + esc(it.title) + '</span></div>';
    }
    h += '</div>';
    stage.innerHTML = h;

    var nodes = stage.querySelectorAll('.insp-node');
    for (var k = 0; k < nodes.length; k++) {
      (function (nd) {
        /* 长按 500ms（Compose detectTapGestures 的默认 longPressTimeout）→ 弹词条简介；
           位移超 12px 或提前松手都取消，避免滚屏时误触发；长按触发后吞掉随后的 click。 */
        var pressTimer = null, fired = false, startX = 0, startY = 0;
        function cancelPress() { if (pressTimer) { clearTimeout(pressTimer); pressTimer = null; } }
        nd.addEventListener('pointerdown', function (e) {
          fired = false;
          startX = e.clientX; startY = e.clientY;
          pressTimer = setTimeout(function () {
            fired = true;
            var t = nd.querySelector('.insp-node-title');
            showInspBrief(t ? t.textContent : '');
          }, 500);
        });
        nd.addEventListener('pointermove', function (e) {
          if (pressTimer && (Math.abs(e.clientX - startX) > 12 || Math.abs(e.clientY - startY) > 12)) cancelPress();
        });
        nd.addEventListener('pointerup', cancelPress);
        nd.addEventListener('pointercancel', cancelPress);
        nd.addEventListener('pointerleave', cancelPress);

        nd.addEventListener('click', function () {
          if (fired) { fired = false; return; }   /* 长按已弹过简介，不再下钻 */
          var nodeId = nd.dataset.node;
          fetchChildren(nodeId).then(function (kids) {
            if (kids && kids.length) {
              state.inspCustom = null;
              state.inspPath.push(nodeId);
              state.inspLevel += 1;                /* 下钻一层 → 直径档位降一档（与 App level=stack.size 等价） */
              renderInspire();                     /* 用该圆词语再解算一次，继续往下钻 */
              var _n = findInspNode(nodeId);
              toast('已解算「' + (_n ? _n.title : nodeId) + '」');
            } else {
              var n = findInspNode(nodeId);
              toast('解算失败「' + (n ? n.title : nodeId) + '」');
            }
          });
        });
      })(nodes[k]);
    }
  }

  /**
   * 长按细分圆弹出的「词条简介」弹窗（App `WordBriefDialog`）。
   * 万物已彻底离线，简介走不到网络，正文固定为「离线模式下暂不提供词条简介」。
   */
  function showInspBrief(word) {
    var screen = document.querySelector('.screen');
    if (!screen) return;
    var mask = screen.querySelector('.insp-brief-mask');
    if (!mask) {
      mask = document.createElement('div');
      mask.className = 'insp-brief-mask';
      mask.innerHTML =
        '<div class="insp-brief-card">' +
          '<div class="insp-brief-title"></div>' +
          '<div class="insp-brief-body">离线模式下暂不提供词条简介</div>' +
          '<div class="insp-brief-foot"><button class="insp-brief-ok" type="button">知道了</button></div>' +
        '</div>';
      screen.appendChild(mask);
      mask.addEventListener('click', function (e) {
        if (e.target === mask || e.target.classList.contains('insp-brief-ok')) {
          mask.classList.remove('open');
        }
      });
    }
    mask.querySelector('.insp-brief-title').textContent = word || '';
    mask.classList.add('open');
  }

  /** 万物页总渲染入口 */
  function renderInspire() {
    if (!state.inspPath.length) {
      state.inspCustom = null;
      state.inspLevel = 0;          /* 回到根态 = 未下钻，直径档位归零 */
      renderInspireRoot();
      return;
    }
    var currentId = state.inspPath[state.inspPath.length - 1];
    if (state.inspCustom && state.inspCustom.forId === currentId) {
      renderInspireLevel(state.inspCustom.nodes, currentId);
      return;
    }
    renderInspireLevel(null, currentId);
  }

  /* ==========================================================
   * 19. Tab 切换与总渲染
   * ========================================================== */
  function setTab(tab) {
    state.tab = tab;
    var tabs = tabbar.querySelectorAll('.tab');
    for (var i = 0; i < tabs.length; i++) {
      tabs[i].classList.toggle('active', tabs[i].dataset.tab === tab);
    }
    Object.keys(VIEWS).forEach(function (k) {
      VIEWS[k].classList.toggle('active', k === tab);
    });
    if (tab === 'ring') renderRing();
    if (tab === 'catalog') renderCatalog();
    if (tab === 'inspire') renderInspire();
    if (tab === 'new') renderNew();
    navSync();
  }

  function renderAll() {
    if (state.tab === 'ring') renderRing();
    if (state.tab === 'catalog') renderCatalog();
    if (state.tab === 'inspire') renderInspire();
    if (state.tab === 'new') renderNew();
    if (state.detailStack.length) renderDetail();
  }

  tabbar.addEventListener('click', function (e) {
    var btn = e.target.closest ? e.target.closest('.tab') : null;
    if (!btn) return;
    if (state.detailStack.length) { state.detailStack = []; renderDetail(); }
    setTab(btn.dataset.tab);
  });

  /* ==========================================================
   * 20. 初始演示数据（镜像手机端 HomeViewModel.ensureSeed：4 个空白根圆，色卡 0/1/2/13）
   * ========================================================== */
  function seed() {
    state.notebooks = [];
    state.nextId = 1;
    state.detailStack = [];
    state.quickParentId = null;
    state.quickSeq = 0;
    state.inspPath = [];
    state.inspCustom = null;
    state.inspQuery = '';

    /* 手机端 HomeViewModel.ensureSeed() 真源：库为空时只建 4 个空白根圆
       （色卡 index 0/1/2/13，其中 13 为紫色 #8957A1），圆本身无标题。
       Web 镜像保持一致：不再预置任何带标题的演示笔记。 */
    var r1 = createNotebook('', '', null); r1.color = PALETTE[0];
    var r2 = createNotebook('', '', null); r2.color = PALETTE[1];

    var r3 = createNotebook('', '', null); r3.color = PALETTE[2];

    var r4 = createNotebook('', '', null); r4.color = PALETTE[13];

    r1.updatedAt = Date.now() - 1000 * 60 * 4;
    r2.updatedAt = Date.now() - 1000 * 60 * 3;
    r3.updatedAt = Date.now() - 1000 * 60 * 2;
    r4.updatedAt = Date.now() - 1000 * 60 * 1;
  }

  /* ==========================================================
   * 21. 启动
   * ========================================================== */
  var resetBtnEl = document.getElementById('resetBtn');
  if (resetBtnEl) resetBtnEl.addEventListener('click', function () {
    seed();
    state.detailStack = [];
    closeSearch();
    closeTrash();          /* 回收站条目随数据一起重置，避免开出空壳覆盖层 */
    renderDetail();
    renderRing();
    renderCatalog();
    renderInspire();
    setTab(state.tab);
    toast('演示数据已重置');
  });

  seed();
  renderRing();
  renderCatalog();
  renderInspire();
  setTab('ring');

  /* 工具条 / 顶栏点击不抢编辑区焦点（U25 第 1 层），必须早于任何交互装好 */
  bindNoFocusGuard();

  /* 目录页四个弹窗（设置 / 隐私政策 / 产品介绍 / 长按删除确认）的按钮：静态 DOM，启动绑一次 */
  bindCatalogDialogs();

  /* 浏览器返回 ≈ App 系统返回键（三级优先级，绝不退出页面） */
  bindBackGesture();

  var resizeTimer = null;
  window.addEventListener('resize', function () {
    if (resizeTimer) clearTimeout(resizeTimer);
    resizeTimer = setTimeout(function () {
      renderRing();
      renderInspire();
    }, 120);
  });

  /* 供调试与冒烟测试：暴露状态与关键函数（不影响页面逻辑） */
  window.__demoState = state;
  window.__demo = {
    state: state,
    CFG: CFG,
    getPalette: function () { return PALETTE; },
    getLightPalette: function () { return LIGHT_PALETTE; },
    getInk: function () { return INK; },
    PALETTE: PALETTE,
    byId: byId,
    childrenOf: childrenOf,
    roots: roots,
    allNotebooks: allNotebooks,
    trashSubtree: trashSubtree,
    trashItems: trashItems,
    restore: restore,
    purge: purge,
    purgeAll: purgeAll,
    openTrash: openTrash,
    renderTrash: renderTrash,
    openDetail: openDetail,
    setTab: setTab,
    setParent: setParent,
    fetchChildren: fetchChildren,
    textColorFor: textColorFor,
    radiusFor: radiusFor,
    radiusOf: radiusOf,
    addCircleByFab: addCircleByFab,
    doInspSearch: doInspSearch,
    searchInspNode: searchInspNode,
    naturalLayout: naturalLayout,
    radialLayout: radialLayout,        /* 已停用：仅为兼容旧调用保留，结果渲染改用 naturalLayout */
    canvasPointOf: canvasPointOf,
    drawDebug: drawDebug
  };

  /* ---------------- 手机外框自适应缩放（匹配用户手机 3168×1440 竖屏） ----------------
     基准逻辑尺寸 390×858（竖屏比 2.2:1 与手机一致），按视口整体等比放大，
     让 Web 演示在屏幕上足够大，且内容比例与手机端完全一致（圆/栏宽随 clientHeight 等比变化）。 */
  (function fitPhoneFrame() {
    var BASE_W = 450, BASE_H = 875;   /* 1440×2800 档（比例 1.94:1）；逻辑宽 450 使万物结果区达到 3 列，与手机端同步 */
    function fit() {
      var phone = document.querySelector('.phone');
      if (!phone) return;
      var availH = (window.innerHeight || document.documentElement.clientHeight || 800) - 130;
      var availW = (window.innerWidth || document.documentElement.clientWidth || 400) - 24;
      var s = Math.min(availW / BASE_W, availH / BASE_H);
      s = Math.max(0.6, Math.min(s, 2.6));   // 合理上下限，避免过小 / 过大
      phone.style.transform = 'scale(' + s + ')';
    }
    window.addEventListener('resize', fit);
    window.addEventListener('orientationchange', fit);
    fit();
    setTimeout(fit, 60);   // 等首帧字体 / 布局稳定后再校准一次
  })();
})();

</script>
</body>
</html>
