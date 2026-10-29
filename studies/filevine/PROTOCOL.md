================ PROTOCOL: functional study of a legal practice-management web app with REA ================

You run on Mohamad's machine as Claude Code with Claude in Chrome. A cloud session (session_01XuPCUvK3LnzXMVREoqtpec) reviews your pushes every 30 minutes and leaves notes in REVIEW.md in your study repository.

1. GOAL
Produce a precise, evidence-backed account of how the application handles matters (projects), contacts, tasks, calendar and deadlines, time entries and billability, bills and invoices, accounts (operating and trust), documents, communications, and reports: the data model, the workflows, the rules and defaults, and the HTTP API the UI actually calls. The audience is the Qadi product and engineering team. They will use it to design Qadi's own modules.

2. THREE SOURCES, IN THIS ORDER
(1) Public documentation and public API reference. Read it first. Do not reverse engineer what is documented. Cite the page.
(2) The live application in Mohamad's trial account, already open in his browser. Use Claude in Chrome to drive it: open every screen in scope, fill forms, create records, run the workflow end to end, read validation messages and defaults.
(3) REA observation of that same session. REA attaches to a Chrome you start with a debugging port and records what the UI requests, the page structure, the scripts and bundles, and route and endpoint candidates. REA output is Evidence with provenance; keep it as the citation for every API claim.
Label every statement in your findings as one of: documented, observed in the UI, observed on the wire (REA), inferred, unknown.

3. REA SETUP (no `rea setup`)
You are inside the Qadi-AI/rea checkout.
  nvm use            # Node 24.22.1 from .nvmrc; engines ^22.23.4 || ^24.22.1 || >=26.12.1
  npm ci
  npm run build:cached
  node scripts/rea.mjs --help
  alias rea="node $(pwd)/scripts/rea.mjs"
Do not run `npx rea-agents setup` and do not register REA as an MCP server; the CLI is enough and leaves agent configuration untouched. Read docs/cli.md, docs/browser-observation.md, docs/browser-scenario-contract.md, docs/web-network-captures.md, docs/website-script-export.md, docs/website-module-trace.md and docs/agent-environments.md before use.

4. BROWSER ATTACHMENT
Chrome 136 and later ignore --remote-debugging-port on the default profile. Start a second Chrome with its own profile and a loopback debugging port, and ask Mohamad to sign in to the application there once:
  "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --remote-debugging-port=9222 --user-data-dir="$HOME/rea-browser-profile" --no-first-run
Keep the port on loopback only. REA does not launch, own or close this browser. The Clio agent shares this browser; whoever starts it first owns it, do not start a second one on the same port. Then:
  rea list-browser-targets --cdp-endpoint http://127.0.0.1:9222 --json
  rea inspect-web-page <target> --json
  rea observe-web-session <target> --duration-ms 120000 --json
  rea analyze-web-bundle <exported scripts> --json
  rea capture-browser-scenario scenario.json --json
Exact flags are in docs/cli.md; `rea <command> --help` prints them. Where REA offers detail "summary" or a view selection, use it and page the retained Evidence; do not dump whole captures into the chat.
For a complete request and response record of one workflow, also export a HAR from the dedicated profile's DevTools (Network panel, Export HAR), then:
  rea inspect-web-network-capture /abs/path/workflow.har har --json > evidence/<workflow>.capture.json
  rea environment-build /abs/path/workflow.har ./env/<workflow> --json

5. PRIVACY AND SAFETY RULES
- Raw HAR files, cookies, tokens, session storage dumps and screenshots that show credentials stay in local/ which is in .gitignore. Never commit them. REA's network Evidence strips cookies and authorization headers and records JSON bodies as value-free shapes; that is what you commit.
- Do not write the tenant hostname, account identifiers or record ids into committed files; refer to "the trial tenant" and use placeholders.
- Use only Mohamad's trial account, only the records you create, and only actions a normal user takes in the UI. Do not probe other tenants, do not fuzz endpoints, do not change account or billing settings that cost money, do not invite users, do not subscribe.
- Do not send anything to the vendor's support or feedback channels.
- The vendor's terms likely restrict reverse engineering and competitive analysis. Mohamad owns that decision. Keep the record factual and limited to observation of his own session.

6. STUDY REPOSITORY AND REPORTING LOOP
The study lives in its own private repository, not in the rea checkout. Create it with `gh repo create Qadi-AI/<name> --private --clone` in a sibling folder. Push at least every 30 minutes, even when a section is half done. Each push updates STATUS.md.
  README.md, STATUS.md, findings/00-overview.md, findings/<area>.md, findings/api-inventory.md, findings/glossary.md, evidence/, env/, screenshots/<area>/, local/ (ignored), REVIEW.md (written by the reviewing session; read it at the start of each push cycle and answer its questions in STATUS.md).
Findings file shape, per area: (1) what the user can do, with screenshot names; (2) objects and fields: name, type, required, default, validation, where set; (3) workflows step by step with what the UI requested on the wire for each step, linking the evidence file and record ordinal; (4) rules observed: calculations, rounding, status transitions, permissions; (5) defaults and settings that change behaviour; (6) what Qadi should copy, do differently, or skip, three short lists; (7) unknowns and what would resolve them.
If you have a send_message tool from the claude-code-remote MCP server, also send a five-line summary to session_01XuPCUvK3LnzXMVREoqtpec after each push. If not, the push is the report.

7. DONE CRITERIA
Every area in the brief has a findings file with sections 1 to 7 filled or marked unknown with a reason. findings/api-inventory.md lists every endpoint REA observed for the workflows you ran, with evidence links. At least one environment bundle built from a recorded workflow, or a note on why environment-build refused the capture. STATUS.md says "complete" and lists the gaps.

8. TIME
If stuck on tooling for more than 20 minutes, push, write the blocker in STATUS.md, and move to the next area.
