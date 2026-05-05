---
title: "Git sync & workspace forks: your entire workspace, version controlled"
url: "https://www.windmill.dev/blog/launch-week-git-sync"
date: "Thu, 02 Apr 2026 00:00:00 GMT"
author: ""
feed_url: "https://www.windmill.dev/blog/rss.xml"
---
<p><img alt="Git sync &amp;amp; workspace forks" class="img_ev3q" height="630" src="https://www.windmill.dev/assets/images/cover-09577abf8d3639ff80251cdb3c2bbb84.webp" width="1200" /></p>
<p><strong>Day 4 of <a href="https://www.windmill.dev/launch-week-march-2026">Windmill launch week</a>.</strong> We have reworked Git sync and introduced workspace forks, giving you a full staging-to-production workflow inside Windmill.</p>
<!-- -->
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="the-problem">The problem<a class="hash-link" href="https://www.windmill.dev/blog/launch-week-git-sync#the-problem" title="Direct link to The problem">​</a></h2>
<p>Windmill workspaces are live environments. You deploy a script, and it runs in production immediately. That works for small teams iterating fast, but as your team grows you need review, staging, and rollback.</p>
<p>Teams had to build custom CI/CD pipelines around Windmill's CLI sync, managing branches manually, and writing their own merge logic. The deploy-to-prod path was functional but required too much glue.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="git-sync-automatic-bidirectional">Git sync: automatic, bidirectional<a class="hash-link" href="https://www.windmill.dev/blog/launch-week-git-sync#git-sync-automatic-bidirectional" title="Direct link to Git sync: automatic, bidirectional">​</a></h2>
<p>Every time you deploy an item in Windmill, it automatically commits and pushes to your configured Git repository. Pull from Git to update your workspace. Supports GitHub, GitLab, Bitbucket, and Azure DevOps.</p>
<p><img alt="Git sync settings" class="img_ev3q" height="1293" src="https://www.windmill.dev/assets/images/git_sync_settings-c83b33cdf7941765b2265ff6272ac15b.png" width="1520" /></p>
<p>Configure Git sync from workspace settings. Path filters let you sync only what matters. Type filters control which resource types are included.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="workspace-forks-branch-your-workspace">Workspace forks: branch your workspace<a class="hash-link" href="https://www.windmill.dev/blog/launch-week-git-sync#workspace-forks-branch-your-workspace" title="Direct link to Workspace forks: branch your workspace">​</a></h2>
<p>Workspace forks let you create an independent copy of a workspace for feature development. Changes in the fork do not affect the parent until you merge them back.</p>
<p>When Git sync is enabled, creating a fork automatically creates a corresponding Git branch (<code>wm-fork/&lt;parent-branch&gt;/&lt;fork-name&gt;</code>). You get parallel development with full version control.</p>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="merge-workflow">Merge workflow<a class="hash-link" href="https://www.windmill.dev/blog/launch-week-git-sync#merge-workflow" title="Direct link to Merge workflow">​</a></h3>
<p>Three ways to bring changes back:</p>
<ol>
<li><strong>Deploy UI</strong>: deploy individual items from the fork to the parent workspace directly from the Windmill UI.</li>
</ol>
<p><img alt="Deploy UI" class="img_ev3q" height="1293" src="https://www.windmill.dev/assets/images/deploy_ui-ffb57623e14d96612a157a60bdd8424c.png" width="1986" /></p>
<ol start="2">
<li><strong>Merge UI</strong>: merge all changes at once with conflict detection, no Git sync required.</li>
</ol>
<p><img alt="Merge UI" class="img_ev3q" height="1168" src="https://www.windmill.dev/assets/images/merge_ui-5e5b6dfc407e7a756adf8d8d20e11e87.png" width="2669" /></p>
<ol start="3">
<li><strong>Git merge</strong>: use your preferred Git workflow (PRs, code review) to merge the fork branch.</li>
</ol>
<p><img alt="Workspace Forks and Git Sync" class="img_ev3q" height="1173" src="https://www.windmill.dev/assets/images/workspace_forks-1137ca96897c946d2b38f6eddf050270.png" width="2583" /></p>
<h3 class="anchor anchorWithStickyNavbar_LWe7" id="coming-soon-data-table-forking">Coming soon: data table forking<a class="hash-link" href="https://www.windmill.dev/blog/launch-week-git-sync#coming-soon-data-table-forking" title="Direct link to Coming soon: data table forking">​</a></h3>
<p>Workspace forks will soon support <a href="https://www.windmill.dev/docs/core_concepts/persistent_storage/data_tables">data tables</a> as well. When forking a workspace, you will be able to clone a data table's schema only or its schema and data, letting you develop against a separate database without affecting upstream workspaces like production.</p>
<p>On merge, Windmill queries the full data table schema, detects differences, and generates SQL migrations automatically.</p>
<p><img alt="Data table fork migration" class="img_ev3q" height="996" src="https://www.windmill.dev/assets/images/datatable_fork_migration-8924fae3349bb72bb52b1d94c10f0b97.png" width="1402" /></p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="why-we-built-it-this-way">Why we built it this way<a class="hash-link" href="https://www.windmill.dev/blog/launch-week-git-sync#why-we-built-it-this-way" title="Direct link to Why we built it this way">​</a></h2>
<p>Three design choices drove the architecture:</p>
<p><strong>Workspace-level branching.</strong> Forks operate at the workspace level, not the file level. When you fork, you get a complete copy of all scripts, flows, apps, resources, and variables. This means you can test changes end-to-end in an isolated environment before merging.</p>
<p><strong>Git as the source of truth.</strong> If you need to roll back, reset the branch. If you need to audit, read the commit history. Windmill does not replace your Git workflow; it plugs into it.</p>
<p><strong>Multiple deployment paths.</strong> Not every team needs the same workflow. Small teams can use the deploy UI. Growing teams can use workspace forks. Enterprise teams can use the full Git promotion workflow with separate dev and prod workspaces, CI/CD, and cross-instance deployment. See <a href="https://www.windmill.dev/docs/advanced/canonical_deployment_setups">Canonical deployment setups</a> for step-by-step guides on each approach.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="deployment-options">Deployment options<a class="hash-link" href="https://www.windmill.dev/blog/launch-week-git-sync#deployment-options" title="Direct link to Deployment options">​</a></h2>
<table><thead><tr><th>Workflow</th><th>Setup</th><th>Best for</th></tr></thead><tbody><tr><td><strong>Draft and deploy</strong></td><td>Single workspace</td><td>Small teams, fast iteration</td></tr><tr><td><strong>Workspace forks</strong></td><td>Fork + merge</td><td>Teams that need staging</td></tr><tr><td><strong>Git promotion</strong></td><td>Git sync + CI/CD + PRs</td><td>Enterprise, cross-instance</td></tr><tr><td><strong>Deploy to prod UI</strong></td><td>Multi-workspace</td><td>Cloud/EE, quick deployments</td></tr></tbody></table>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="getting-started">Getting started<a class="hash-link" href="https://www.windmill.dev/blog/launch-week-git-sync#getting-started" title="Direct link to Getting started">​</a></h2>
<p><strong>Git sync:</strong></p>
<ol>
<li>Create a Git repository.</li>
<li>Go to workspace settings, then Git sync.</li>
<li>Configure authentication (GitHub App, PAT, or GitHub Enterprise App).</li>
<li>Deploy an item and check your repo for the commit.</li>
</ol>
<p><strong>Workspace forks:</strong></p>
<ol>
<li>Enable Git sync (recommended but optional).</li>
<li>Create a fork from the workspace settings.</li>
<li>Make changes in the fork.</li>
<li>Merge back using the deploy UI, merge UI, or Git.</li>
</ol>
<div class="grid grid-cols-2 gap-6 mb-4"><a class="rounded-md p-8 border border-gray-100 dark:border-gray-800 shadow-sm transition-all cursor-pointer flex flex-col gap-2 !no-underline overflow-hidden hover:bg-blue-100 hover:dark:bg-blue-900/50 hover:border-blue-500 dark:hover:border-blue-600" href="https://www.windmill.dev/docs/advanced/git_sync"><div class="flex flex-row gap-4 items-center"><div class="text-lg font-semibold text-gray-800 dark:text-gray-100">Git sync</div></div><div class="text-sm text-gray-500 dark:text-gray-200">Sync your workspace to a Git repository.</div></a><a class="rounded-md p-8 border border-gray-100 dark:border-gray-800 shadow-sm transition-all cursor-pointer flex flex-col gap-2 !no-underline overflow-hidden hover:bg-blue-100 hover:dark:bg-blue-900/50 hover:border-blue-500 dark:hover:border-blue-600" href="https://www.windmill.dev/docs/advanced/workspace_forks"><div class="flex flex-row gap-4 items-center"><div class="text-lg font-semibold text-gray-800 dark:text-gray-100">Workspace forks</div></div><div class="text-sm text-gray-500 dark:text-gray-200">Fork workspaces for feature development.</div></a></div>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="whats-next">What's next<a class="hash-link" href="https://www.windmill.dev/blog/launch-week-git-sync#whats-next" title="Direct link to What's next">​</a></h2>
<p>Tomorrow is Day 5: <strong>Workflow-as-code</strong>. Define complex workflows entirely in code with the next generation of our SDK. <a href="https://www.windmill.dev/launch-week-march-2026">Follow along</a>.</p><div class="flex items-start bg-blue-50 rounded-md mt-12 p-4"><img alt="Windmill Logo" class="mt-1 mr-2 w-10 h-10" height="48" src="https://www.windmill.dev/img/windmill.svg" width="48" /><div class="text-gray-600 font-medium"><a href="https://www.windmill.dev/">Windmill</a> is an<!-- --> <a href="https://github.com/windmill-labs/windmill">open-source</a> and<!-- --> <a href="https://www.windmill.dev/docs/advanced/self_host/">self-hostable</a> serverless runtime and platform combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.<br /><br />You can <a href="https://www.windmill.dev/docs/advanced/self_host/">self-host</a> Windmill using a<!-- --> <code>docker compose up</code>, or go with the<!-- --> <a href="https://app.windmill.dev/user/login" rel="nofollow">cloud app</a>.</div></div>
