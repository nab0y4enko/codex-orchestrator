# Example prompt

Once the template files are installed, normal prompts should focus on product requirements and constraints. Persistent orchestration policy handles model routing, decomposition, focused test planning, escalation, review, and final acceptance.

```text
Add passwordless login using email magic links.

Requirements:
- A user can request a magic link for an existing account.
- The link expires after 15 minutes and can be used only once.
- A valid link creates the same authenticated session as password login.
- Invalid, expired, and already-used links return the existing generic authentication error.

Constraints:
- Keep the current email provider and delivery pipeline.
- Preserve the existing password login behavior.
- Do not reveal whether an email address has an account.
- Do not change unrelated authentication or account-recovery flows.

Acceptance criteria:
- Requesting a link does not disclose account existence.
- A valid link authenticates once within the expiration window.
- Expired, invalid, and reused links are rejected.
- Existing password login continues to work.
- Relevant documentation and configuration are updated.

Validation:
- Add or update focused tests for link creation, expiration, one-time use,
  invalid tokens, successful session creation, account-enumeration protection,
  and password-login regression where those behaviors are affected.
- Run only the focused tests and checks related to the changed authentication
  surface.
- Do not run the full test suite unless I explicitly request it. If shared
  authentication infrastructure makes a full run necessary, explain why and
  ask me first.
```
