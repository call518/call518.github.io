---
title: call518.github.io
---

# call518.github.io (JungJungIn)

GitHub: [https://github.com/call518](https://github.com/call518)

## List of Repositories (excluding forked)

- 🌐 = GitHub Pages site  
- 📦 = GitHub repository  

<div id="pages-list">Loading…</div>

<style>
  /* 3 lines per repository: title / description / update info */
  ul.repo-list { list-style: none; padding: 0; margin: 16px 0; }
  ul.repo-list li { padding: 14px 8px; border-bottom: 1px solid #e5e7eb; }
  ul.repo-list li:last-child { border-bottom: none; }
  .repo-title { font-weight: 700; font-size: 1.05rem; line-height: 1.4; }
  .repo-title a { text-decoration: none; }
  .repo-title a:hover { text-decoration: underline; }
  .repo-desc { font-size: .95rem; color: #374151; margin-top: 4px; white-space: normal; word-break: break-word; }
  .repo-meta { font-size: .88rem; color: #6b7280; margin-top: 4px; }
  .tag { font-size: .78rem; padding: 2px 6px; border: 1px solid #e5e7eb; border-radius: 9999px; margin-left: 6px; }
</style>

<script>
(async function () {
  const username = "call518";
  const target = document.getElementById("pages-list");

  try {
    const resp = await fetch(`https://api.github.com/users/${username}/repos?per_page=100`, {
      headers: { "Accept": "application/vnd.github+json" }
    });
    if (!resp.ok) throw new Error(`GitHub API error: ${resp.status}`);

    const repos = await resp.json();

    // Condition: exclude forked + exclude call518.github.io itself
    const myRepos = repos
      .filter(r => !r.fork)
      .filter(r => r.name !== `${username}.github.io`)
      .sort((a, b) => new Date(b.pushed_at) - new Date(a.pushed_at));

    if (myRepos.length === 0) {
      target.textContent = "No repositories to display.";
      return;
    }

    const ul = document.createElement("ul");
    ul.className = "repo-list";

    for (const r of myRepos) {
      // Decide link: Pages URL if available, otherwise repo URL
      let url;
      if (r.has_pages) {
        url = `https://${username}.github.io/${r.name}`;
      } else {
        url = r.html_url; // https://github.com/<user>/<repo>
      }

      const emoji = r.has_pages ? "🌐" : "📦";
      const lastPush = new Date(r.pushed_at).toLocaleString('en-US', {
        year: 'numeric', month: '2-digit', day: '2-digit',
        hour: '2-digit', minute: '2-digit'
      });

      const li = document.createElement("li");
      li.innerHTML = `
        <div class="repo-title">
          ${emoji} <a href="${url}" target="_blank" rel="noopener">${r.name}</a>
          ${r.archived ? '<span class="tag">archived</span>' : ''}
        </div>
        <div class="repo-desc">${r.description ? r.description : "No description"}</div>
        <div class="repo-meta">Last updated: ${lastPush}</div>
      `;
      ul.appendChild(li);
    }

    target.innerHTML = "";
    target.appendChild(ul);
  } catch (e) {
    console.error(e);
    target.textContent = "Error loading repository list. Please try again later.";
  }
})();
</script>

---
