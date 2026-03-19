# Automated Employee Offboarding & Access Revocation

**Category:** IT Operations & ITSM
**Author:** Christopher Ayodeji Ojo — [Trublshut](https://whop.com/trublshut)
**Tools:** n8n · Microsoft Power Automate · Microsoft Entra ID · Jira · Microsoft 365

---

## 🔴 The Problem

When employees leave, manual offboarding leaves security gaps — accounts left active, licences unpaid, access ungoverned. IT teams juggle 10+ manual steps across multiple systems under time pressure, and a single missed step can mean an ex-employee retaining access to sensitive systems.

This is one of the highest-risk manual processes in any IT operation. The cost of getting it wrong is not just operational — it is a security and compliance liability.

---

## ✅ The Solution

A triggered offboarding pipeline that executes every deprovisioning step automatically — disabling accounts, revoking access, recovering licences, and notifying all relevant teams — the moment an exit is confirmed in HR.

Nothing is missed. Everything is timestamped. Every action is logged.

---

## ⚙️ How It Works

```
HR submits offboarding request / exit confirmed
        ↓
n8n workflow triggered via webhook
        ↓
Disable Entra ID account immediately (block sign-in)
        ↓
Revoke all active sessions (token invalidation)
        ↓
Remove from all security groups + Teams channels
        ↓
Unassign Microsoft 365 licence (recover cost)
        ↓
Transfer mailbox / OneDrive access to line manager
        ↓
Create Jira ticket for device collection
        ↓
Notify IT, HR, Finance, and manager via Teams
        ↓
Log all actions with timestamps to audit record (SharePoint)
```

---

## 🛠️ Tech Stack

| Tool | Role |
|------|------|
| n8n | Workflow orchestration + webhook trigger |
| Microsoft Entra ID API | Account disable + session revocation + group removal |
| Microsoft 365 Admin API | Licence recovery + mailbox transfer |
| Jira REST API | Device collection ticket creation |
| Power Automate | Multi-team Teams notifications |
| SharePoint | Full deprovisioning audit log |

---

 Key Logic

- **Immediate account block** — first action in the workflow is disabling sign-in, before anything else
- **Session revocation** — calls the Entra ID `revokeSignInSessions` API to invalidate all active tokens, even if the user is currently logged in
- **Group removal loop** — iterates through all group memberships and removes each one
- **Licence recovery** — unassigns all assigned M365 licences so they return to the pool immediately
- **Mailbox transfer** — grants full access to manager's account, sets auto-reply, and hides from GAL
- **Device ticket** — pre-populated Jira ticket with employee name, last working day, device details, and collection instructions
- **Parallel notifications** — Teams cards sent simultaneously to IT, HR, Finance, and manager

---

Outcomes

- ✅ Account disabled within minutes of exit confirmation — not days
- ✅ No orphaned accounts or active sessions left open
- ✅ Licence recovery reduces SaaS spend immediately
- ✅ Security and compliance posture significantly improved
- ✅ Full deprovisioning audit trail for every leaver — timestamped, logged, and reportable

---

How to Use This

1. Create an n8n webhook triggered by HR form submission (Microsoft Forms / HRIS)
2. First node: call Entra ID API to block sign-in (`accountEnabled: false`)
3. Second node: call `revokeSignInSessions` on the user object
4. Loop node: iterate group memberships and call `removeMember` for each group
5. Call M365 Admin API to remove licence assignments
6. Call Exchange API to grant manager full mailbox access + set auto-reply
7. Create Jira device collection ticket via Jira REST API
8. Send Teams notifications to IT, HR, Finance, manager (parallel branches)
9. Write a completion record to SharePoint with all action timestamps

---

## 🔗 Related Use Cases

- [Use Case 02 — Automated Employee Onboarding Pipeline](./UC-02-Automated-Employee-Onboarding-Pipeline.md)
- [Use Case 04 — Device Provisioning Automation (Jira → Intune)](./UC-04-Device-Provisioning-Jira-Intune.md)

---

*© 2026 Christopher Ayodeji Ojo · Trublshut AI Automation · christopherayodeji131@gmail.com*
