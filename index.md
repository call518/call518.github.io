---
title: call518.github.io
---

# call518.github.io (JungJungIn)

[내 저장소](https://github.com/call518)들 중 **GitHub Pages**가 켜진 사이트 목록

<div id="pages-list">로딩 중…</div>

<style>
  .grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(260px,1fr)); gap:16px; margin: 16px 0; }
  .card { border:1px solid #e5e7eb; border-radius:12px; padding:14px; }
  .card h3 { margin:0 0 8px 0; font-size:1.05rem; }
  .meta { font-size:.9rem; color:#6b7280; }
</style>

<script>
(async function () {
  const username = "call518";
  const target = document.getElementById("pages-list");

  try {
    const resp = await fetch(`https://api.github.com/users/${username}/repos?per_page=100`, {
      headers: { "Accept": "application/vnd.github+json" }
    });
    if (!resp.ok) throw new Error(`GitHub API 오류: ${resp.status}`);

    const repos = await resp.json();

    // has_pages=true 이고, 사용자 페이지 저장소(call518.github.io)는 제외
    const pagesRepos = repos
      .filter(r => r.has_pages && r.name !== `${username}.github.io`)
      .filter(r => !r.archived) // 보통 아카이브 제외
      .sort((a, b) => new Date(b.pushed_at) - new Date(a.pushed_at)); // 최근 업데이트 순

    if (pagesRepos.length === 0) {
      target.textContent = "표시할 GitHub Pages 사이트가 없습니다.";
      return;
    }

    const grid = document.createElement("div");
    grid.className = "grid";

    for (const r of pagesRepos) {
      // 기본 URL 규칙: https://<user>.github.io/<repo>
      // (홈페이지 필드가 있으면 그걸 우선 사용)
      const defaultUrl = `https://${username}.github.io/${r.name}`;
      const url = r.homepage && r.homepage.trim() ? r.homepage : defaultUrl;

      const card = document.createElement("div");
      card.className = "card";
      card.innerHTML = `
        <h3><a href="${url}">${r.name}</a></h3>
        <div class="meta">${r.description ? r.description : "No description"}</div>
        <div class="meta">Last push: ${new Date(r.pushed_at).toLocaleString()}</div>
        <div class="meta">Default branch: ${r.default_branch}</div>
      `;
      grid.appendChild(card);
    }

    target.innerHTML = "";
    target.appendChild(grid);
  } catch (e) {
    console.error(e);
    target.textContent = "목록 로딩 중 오류가 발생했습니다. 잠시 후 다시 시도하세요.";
  }
})();
</script>

---
