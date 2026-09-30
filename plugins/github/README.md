# GitHub

Issues, pull requests and code on GitHub through GitHub's remote MCP server
(`https://api.githubcopilot.com/mcp/`, toolsets `context, repos, issues, pull_requests, users`).

## What it does

- **Apps:** one remote MCP server, `github`. Myndr signs in with your GitHub account through its GitHub connection;
  the package holds no token. Organizations on Copilot Business or Enterprise must allow MCP servers in their
  Copilot policy.
- **Skills:**
  - `github:triage-issues` — sort the backlog and propose next actions
  - `github:review-pr` — review a pull request, and post only on your word
  - `github:release-notes` — draft notes from merged pull requests
- **Automations:** with this plugin installed, Myndr's GitHub automation nodes are available.

Tools that change repositories (comments, issue edits, reviews, merges, file writes) ask for approval by default in
Myndr.

## Skill provenance

All three skills are written by Myndr for this plugin (MIT). `triage-issues` moved here from the retired
`repo-triage` plugin, with its tool steps rewritten for the GitHub MCP server. They name only tools the GitHub server
lists in the toolsets above.

## Logo and trademark

`assets/icon.svg` (light backgrounds) and `assets/icon-dark.svg` (dark backgrounds) are the GitHub mark from
Primer Octicons `mark-github-24` (`@primer/octicons` 19.38.0, https://primer.style/octicons/icon/mark-github-24/,
MIT), with one fill added for use in an image: `#1F2328` and `#F0F6FC`, GitHub's Primer foreground colours. See
GitHub's logo guidelines at https://brand.github.com/foundations/logo.

Trademark: GitHub and the GitHub logo are trademarks of GitHub, Inc. They are used here only to identify the
service this plugin connects to; this plugin is not made, sponsored or endorsed by GitHub. The logo files are not
covered by this repository's MIT license.
