# AI Engineer Roadmap Platform — Master Build Prompt (Production SaaS Edition)

> **What this document is:** a single, self-contained prompt for an AI coding agent (Claude Code or equivalent) to execute the complete production build of the AI Engineer Roadmap Platform as a multi-tenant SaaS product. It was assembled by reading all four of your source documents end-to-end, reconciling two real conflicts found between them, patching two gaps found in the database schema, and verifying the canonica[...Truncated text #1 +141 lines...] resets naturally and never needs cleanup.

**RBAC extension:** the existing `users.role` (`learner` by default) gains a `platform_admin` value for internal support/ops tooling. Organization-scoped roles (`owner`, `admin`, `member`) live on a separate `organization_members` row so a platform-level role and an org-level role never collide. See Section 4 for the exact tables.

--- strict rule read each and every file in this directory don't miss a single nuance . use plugins for better efficiency.
