# Interview-Practice


**JS Code Snippet - Add more Issues:**

// seed-issues.js
// Usage:
//   mac/linux (bash):  export GH_TOKEN=your_token && node seed-issues.js
//   windows (PowerShell):  $env:GH_TOKEN="your_token"; node seed-issues.js

  
    // Get token from env var (safer than hardcoding)
    const GITHUB_TOKEN = process.env.GH_TOKEN;
    if (!GITHUB_TOKEN) {
      console.error("❌ Missing GH_TOKEN env var. Set it before running.");
      process.exit(1);
    }
  
    // Ensure we have a fetch function (Node 18+ has global fetch)
    const fetchFn = (typeof fetch === "function")
      ? fetch
      : (await import("node-fetch")).default;
  
    const issues = [
      { title: "Bug: login button not clickable", body: "Mobile Safari login button unresponsive. Steps: 1) Open app on iOS 15 2) Tap login. Expected: dashboard." },
      { title: "Feature Request: Dark Mode", body: "Add dark theme to improve accessibility and late-night usage." },
      { title: "Bug: Pagination missing on /tasks API", body: "Only 50 items returned; no Link header documented." },
      { title: "Docs: Clarify rate limit headers", body: "Explain X-RateLimit-Reset vs Retry-After with examples." },
      { title: "Question: OAuth2 refresh tokens", body: "Are refresh tokens long-lived or do users re-consent every 30 days?" },
      { title: "Bug: 500 updating profile picture", body: "Uploading 5MB JPEG yields 500; smaller files succeed." },
      { title: "Feature: Webhook retries policy", body: "Document retry count and intervals for webhook deliveries." },
      { title: "Bug: CORS preflight fails", body: "OPTIONS request returns 403. Need proper CORS headers." },
      { title: "Docs: Minimal curl for POST /issues", body: "Include headers and compact JSON example." },
      { title: "Feature: Bulk create contacts", body: "POST /contacts should accept an array for batch creation." },
      { title: "Bug: 403 on POST /messages", body: "Even with write scope, returns 403. Scope mismatch?" },
      { title: "Docs: Webhook signature algorithm", body: "Confirm HMAC-SHA256 and provide verification example." },
      { title: "Bug: Slow search endpoint", body: "GET /search takes ~12s for >1k records since v2.3." },
      { title: "Feature: Filter on list orders", body: "Support ?status=open to reduce client-side filtering." },
      { title: "Bug: Retry-After missing on 429", body: "Provide Retry-After so clients can back off correctly." },
      { title: "Question: Webhook idempotency", body: "Should clients de-dup events or is at-least-once guaranteed?" },
      { title: "Feature: Sandbox environment", body: "Provide test data separate from production." },
      { title: "Bug: 404 creating issues on private repo", body: "Likely permission masking; document expected behavior." },
      { title: "Docs: Versioning policy header", body: "Document X-API-Version and deprecation timelines." },
      { title: "Bug: Duplicate webhook deliveries", body: "Seeing occasional duplicates; confirm retry/backoff logic." }
    ];
  
    async function createIssue(issue) {
      const url = `https://api.github.com/repos/${OWNER}/${REPO}/issues`;
      const res = await fetchFn(url, {
        method: "POST",
        headers: {
          "Authorization": `Bearer ${GITHUB_TOKEN}`,
          "Accept": "application/vnd.github+json",
          "Content-Type": "application/json"
        },
        body: JSON.stringify(issue)
      });
  
      if (!res.ok) {
        const text = await res.text();
        throw new Error(`Failed to create issue: ${res.status} ${res.statusText}\n${text}`);
      }
  
      const data = await res.json();
      console.log(`✅ Created issue #${data.number}: ${data.title}`);
    }
  
    try {
      for (const issue of issues) {
        await createIssue(issue);
      }
      console.log("🎉 Done seeding 20 issues.");
    } catch (err) {
      console.error("❌ Error while seeding:", err.message);
      process.exit(1);
    }
  })();
  
