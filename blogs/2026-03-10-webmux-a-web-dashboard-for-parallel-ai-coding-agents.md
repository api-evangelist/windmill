---
title: "Webmux: a web dashboard for parallel AI coding agents"
url: "https://www.windmill.dev/blog/webmux"
date: "Tue, 10 Mar 2026 00:00:00 GMT"
author: ""
feed_url: "https://www.windmill.dev/blog/rss.xml"
---
<p>Every team running AI coding agents in parallel hits the same wall. You have 5 agents going, each in its own git worktree, each with dev servers, each producing PRs. You're alt-tabbing between terminal windows, GitHub tabs, and CI dashboards trying to keep track.</p>
<p>The tooling to manage this is evolving fast. <a href="https://workmux.raine.dev/" rel="noopener noreferrer" target="_blank">workmux</a> uses the one-worktree-one-tmux-one-agent model. <a href="https://cmux.com/" rel="noopener noreferrer" target="_blank">Cmux</a> brings Ghostty-based terminal management with notification rings. Others have built internal solutions around tmux scripts, custom CLIs, or IDE extensions. We tried several of these approaches at Windmill, and none fully solved our problem.</p>
<p>On the other hand, writing code has become cheap, so building our own tooling would not take much time. The advantage is complete control over the experience: we implement exactly the features we need, and we iterate fast.</p>
<p>So we built <a href="https://webmux.dev/" rel="noopener noreferrer" target="_blank"><strong>Webmux</strong></a>, an open-source web dashboard and CLI for creating, monitoring, and managing parallel AI agents. It combines embedded terminals (xterm.js rendering real Claude/Codex sessions), GitHub PR and CI tracking, service health monitoring, and Docker sandboxing in one tool.</p>
<p>Webmux is open-source and MIT licensed. Check it out on <a href="https://github.com/windmill-labs/webmux" rel="noopener noreferrer" target="_blank">GitHub</a> or visit <a href="https://webmux.dev/" rel="noopener noreferrer" target="_blank">webmux.dev</a> to get started.</p>
<p><img alt="Webmux dashboard" class="img_ev3q" height="1204" src="https://www.windmill.dev/assets/images/header-f526fae036e3d08cab017f5e6bbecba3.png" width="2132" /></p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="why-a-web-ui-around-a-terminal">Why a web UI around a terminal<a class="hash-link" href="https://www.windmill.dev/blog/webmux#why-a-web-ui-around-a-terminal" title="Direct link to Why a web UI around a terminal">​</a></h2>
<p>We started with <a href="https://workmux.raine.dev/" rel="noopener noreferrer" target="_blank">workmux</a>, which nails the core abstraction: one worktree, one tmux session, one agent per task. It's a great tool if you live in the terminal.</p>
<p>But as the number of parallel agents grew, we kept running into friction that no terminal-only tool could fully solve:</p>
<ul>
<li><strong>Switching context</strong> between tmux windows to check agent progress</li>
<li><strong>Opening GitHub</strong> in a separate tab to see if a PR was opened, if CI passed, if there were review comments</li>
<li><strong>Checking service health</strong> manually (did the dev server in worktree #3 crash again?)</li>
<li><strong>Losing track</strong> of which agent was doing what across 5+ concurrent worktrees</li>
</ul>
<p>The other extreme, building a custom UI per agent using the Claude or Codex SDK, would solve the aggregation problem but create a new one: every upstream release would require us to update our own renderer.</p>
<p>So we went with the hybrid approach: a web dashboard that renders real terminal sessions via xterm.js. You get everything in one browser tab (live terminals, PR status, CI results, service health, notifications) while the terminal stays the source of truth. Anthropic and OpenAI updates show up automatically, and plugging in another CLI like the <a href="https://github.com/google-gemini/gemini-cli" rel="noopener noreferrer" target="_blank">Gemini CLI</a> or <a href="https://opencode.ai/" rel="noopener noreferrer" target="_blank">OpenCode</a> is just configuration.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="how-it-works">How it works<a class="hash-link" href="https://www.windmill.dev/blog/webmux#how-it-works" title="Direct link to How it works">​</a></h2>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="one-click-worktree-creation">One-click worktree creation<a class="hash-link" href="https://www.windmill.dev/blog/webmux#one-click-worktree-creation" title="Direct link to One-click worktree creation">​</a></h3>
<p>Pick a profile, name a branch, write a prompt. Webmux handles the rest:</p>
<ol>
<li>Creates a <a href="https://git-scm.com/docs/git-worktree" rel="noopener noreferrer" target="_blank">git worktree</a></li>
<li>Allocates ports for dev servers (backend, frontend, etc.)</li>
<li>Spins up a tmux session with the configured pane layout</li>
<li>Starts the AI agent (Claude, Codex, or others) with the prompt and environment variables</li>
<li>Runs any <code>postCreate</code> lifecycle hook (install dependencies, seed data, etc.)</li>
</ol>
<p>All of this is driven by a single <code>.webmux.yaml</code> config file at the root of your project.</p>
<video class="w-full rounded-lg border-2 dark:border-gray-800 my-4" controls="" loop=""><source src="/assets/medias/create-3d89ed87a6044efe67865db75f8a15cf.mp4" /><p>Your browser does not support the video tag.
<a href="https://www.windmill.dev/assets/medias/create-3d89ed87a6044efe67865db75f8a15cf.mp4">Download the video</a>.</p></video>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="cli">CLI<a class="hash-link" href="https://www.windmill.dev/blog/webmux#cli" title="Direct link to CLI">​</a></h3>
<p>Everything you can do from the web UI is also available from the command line:</p>
<ul>
<li><code>webmux create</code>: create a worktree with a profile and prompt</li>
<li><code>webmux list</code>: list active worktrees with their status</li>
<li><code>webmux remove</code>: tear down a worktree and run cleanup hooks</li>
<li><code>webmux attach</code>: attach to a worktree's tmux session</li>
</ul>
<p>Since tmux is the source of truth, changes from the CLI and the web UI are always in sync. This also lets the agent spawn new worktrees for subtasks while working on a parent task.</p>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="pr-and-ci-monitoring">PR and CI monitoring<a class="hash-link" href="https://www.windmill.dev/blog/webmux#pr-and-ci-monitoring" title="Direct link to PR and CI monitoring">​</a></h3>
<p>Webmux polls GitHub to track PRs for each worktree's branch. When an agent opens a PR, you see it immediately in the dashboard with:</p>
<ul>
<li>PR state (open, merged, closed)</li>
<li>CI check status and details: click through to see failed test logs</li>
<li>Review comments displayed inline, so you can read code review feedback without leaving the dashboard</li>
</ul>
<video class="w-full rounded-lg border-2 dark:border-gray-800 my-4" controls="" loop=""><source src="/assets/medias/ci-7538e962ea1502761493e5e6c039df2e.mp4" /><p>Your browser does not support the video tag.
<a href="https://www.windmill.dev/assets/medias/ci-7538e962ea1502761493e5e6c039df2e.mp4">Download the video</a>.</p></video>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="service-health">Service health<a class="hash-link" href="https://www.windmill.dev/blog/webmux#service-health" title="Direct link to Service health">​</a></h3>
<p>Each worktree can define services with allocated ports. Webmux periodically checks these ports and displays badges: green if the dev server is up, red if it crashed. At a glance, you know which worktrees have healthy environments and which need attention.</p>
<video class="w-full rounded-lg border-2 dark:border-gray-800 my-4" controls="" loop=""><source src="/assets/medias/health-cb42aad44a6c194917d0c497d80a0c36.mp4" /><p>Your browser does not support the video tag.
<a href="https://www.windmill.dev/assets/medias/health-cb42aad44a6c194917d0c497d80a0c36.mp4">Download the video</a>.</p></video>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="sandboxed-agents">Sandboxed agents<a class="hash-link" href="https://www.windmill.dev/blog/webmux#sandboxed-agents" title="Direct link to Sandboxed agents">​</a></h3>
<p>The <code>sandbox</code> profile runs agents inside Docker containers instead of on the host. Set <code>runtime: docker</code> in your profile and Webmux handles the rest:</p>
<ul>
<li>Mounts git credentials (<code>~/.gitconfig</code>, <code>~/.ssh</code>, <code>~/.config/gh</code>) read-only so the agent can push and open PRs</li>
<li>Mounts AI credentials (<code>~/.claude</code>, <code>~/.claude.json</code>, <code>~/.codex</code>) so Claude Code and Codex work out of the box</li>
<li>Forwards service ports on loopback (<code>127.0.0.1</code>) so dev servers are accessible from the browser without being exposed externally</li>
<li>Runs as your host UID/GID so file ownership stays consistent across host and container</li>
<li>Passes environment variables listed in <code>envPassthrough</code> (API keys, cloud credentials, etc.)</li>
</ul>
<p>This gives agents full autonomy (install packages, run servers, execute tests) without risking your host environment.</p>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="lifecycle-hooks">Lifecycle hooks<a class="hash-link" href="https://www.windmill.dev/blog/webmux#lifecycle-hooks" title="Direct link to Lifecycle hooks">​</a></h3>
<p>Each worktree can run scripts at key moments in its lifecycle via <code>postCreate</code> and <code>preRemove</code> hooks defined in <code>.webmux.yaml</code>. This is where you wire up environment-specific setup and teardown: provision a database, install dependencies, seed test data, or clean up resources when a worktree is removed.</p>
<p>For example, at Windmill each worktree needs its own isolated Postgres database so agents never collide. The <code>postCreate</code> hook provisions a fresh database and runs migrations automatically. Combined with profiles (one that clones an already-seeded database, another that starts from a blank slate), each engineer picks the exact starting point they need through the profile selector, without any manual setup.</p>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="linear-integration">Linear integration<a class="hash-link" href="https://www.windmill.dev/blog/webmux#linear-integration" title="Direct link to Linear integration">​</a></h3>
<p>We use <a href="https://linear.app/" rel="noopener noreferrer" target="_blank">Linear</a> for issue tracking at Windmill, and Webmux brings it right into the dashboard. You can browse your backlog, search by title, preview the full issue details, and hit <strong>Implement</strong> to spin up a worktree for it in one click.</p>
<video class="w-full rounded-lg border-2 dark:border-gray-800 my-4" controls="" loop=""><source src="/assets/medias/linear-73a76660e87224dff8951c72e5284c0d.mp4" /><p>Your browser does not support the video tag.
<a href="https://www.windmill.dev/assets/medias/linear-73a76660e87224dff8951c72e5284c0d.mp4">Download the video</a>.</p></video>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="mobile-friendly">Mobile friendly<a class="hash-link" href="https://www.windmill.dev/blog/webmux#mobile-friendly" title="Direct link to Mobile friendly">​</a></h3>
<p>The web UI is fully responsive. On a phone you can check which agents are running, read PR comments and CI results, monitor service health, and create or remove worktrees. Combined with a Tailscale setup on a remote machine, you can supervise your agents from anywhere without needing a laptop.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="architecture">Architecture<a class="hash-link" href="https://www.windmill.dev/blog/webmux#architecture" title="Direct link to Architecture">​</a></h2>
<p>The stack is intentionally simple:</p>
<ul>
<li><strong>Backend</strong>: A single <a href="https://bun.sh/" rel="noopener noreferrer" target="_blank">Bun</a> server that orchestrates git, tmux, Docker, and the GitHub CLI. The codebase follows adapter/service/domain layering: adapters handle I/O (git commands, tmux sessions, Docker containers), services implement business logic (lifecycle management, PR monitoring, reconciliation), and the domain layer holds pure types and policies</li>
<li><strong>Frontend</strong>: <a href="https://svelte.dev/" rel="noopener noreferrer" target="_blank">Svelte 5</a> with xterm.js for terminal rendering</li>
</ul>
<p>No database. The only external services are GitHub (for PRs and CI) and optionally Linear (for issue tracking). All state is derived from git worktrees and tmux sessions.</p>
<p>Everything is configured through a single <code>.webmux.yaml</code> at the root of your project. Key fields include: <code>services</code> to declare dev servers with auto-allocated ports, <code>profiles</code> to define pane layouts and choose between <code>runtime: host</code> or <code>runtime: docker</code>, and <code>lifecycleHooks</code> (<code>postCreate</code>, <code>preRemove</code>) to run setup and teardown scripts per worktree.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="how-we-use-it-at-windmill">How we use it at Windmill<a class="hash-link" href="https://www.windmill.dev/blog/webmux#how-we-use-it-at-windmill" title="Direct link to How we use it at Windmill">​</a></h2>
<p>Webmux works fine on a local laptop, but it really shines on a powerful remote machine. Windmill is a large Rust codebase, and each agent compiling in its own worktree is CPU- and memory-intensive. We run Webmux as an always-on service on a beefy remote server behind <a href="https://tailscale.com/" rel="noopener noreferrer" target="_blank">Tailscale</a>, so it's accessible from anywhere.</p>
<video class="w-full rounded-lg border-2 dark:border-gray-800 my-4" controls="" loop=""><source src="/assets/medias/demo-70e6479e1cd41749dbbe5042d255b08b.mp4" /><p>Your browser does not support the video tag.
<a href="https://www.windmill.dev/assets/medias/demo-70e6479e1cd41749dbbe5042d255b08b.mp4">Download the video</a>.</p></video>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="impact-on-our-velocity">Impact on our velocity<a class="hash-link" href="https://www.windmill.dev/blog/webmux#impact-on-our-velocity" title="Direct link to Impact on our velocity">​</a></h2>
<p>This is not a controlled experiment, and plenty of other factors are at play, but the trend is hard to ignore. Between mid-January and late March 2026, our weekly <a href="https://github.com/windmill-labs/windmill/pulls?q=is%3Apr+is%3Aclosed" rel="noopener noreferrer" target="_blank">merged PRs</a> went from 36 to 96, an all-time high for the team.</p>
<p><img alt="Weekly PR summary growth, January to March 2026" class="img_ev3q" height="753" src="https://www.windmill.dev/assets/images/graph-880fcb168eebfeac6de909b2967d542f.png" width="1656" /></p>
<p>Going from "how many things can one person do" to "how many agents can one person supervise" is a real shift, and Webmux is what makes the supervising part practical.</p><div class="flex items-start bg-blue-50 rounded-md mt-12 p-4"><img alt="Windmill Logo" class="mt-1 mr-2 w-10 h-10" height="48" src="https://www.windmill.dev/img/windmill.svg" width="48" /><div class="text-gray-600 font-medium"><a href="https://www.windmill.dev/">Windmill</a> is an<!-- --> <a href="https://github.com/windmill-labs/windmill">open-source</a> and<!-- --> <a href="https://www.windmill.dev/docs/advanced/self_host/">self-hostable</a> serverless runtime and platform combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.<br /><br />You can <a href="https://www.windmill.dev/docs/advanced/self_host/">self-host</a> Windmill using a<!-- --> <code>docker compose up</code>, or go with the<!-- --> <a href="https://app.windmill.dev/user/login" rel="nofollow">cloud app</a>.</div></div>
