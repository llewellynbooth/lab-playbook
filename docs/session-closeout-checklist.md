# Session Close-Out Checklist

Complete this before ending every session. In order. No exceptions.

---

## 1. Lesson Objectives Review
- [ ] Review original lesson objectives
- [ ] Mark each as complete, deferred, or carried forward
- [ ] Document any scope changes as ADR entries

---

## 2. Verification
- [ ] All planned changes tested and confirmed working
- [ ] AD replication healthy — repadmin /showrepl
- [ ] pfSense gateway online — Status → Gateways
- [ ] Internet connectivity confirmed from at least one VM
- [ ] Time sync correct on all servers — w32tm /query /status
- [ ] WAC dashboard — all servers connected

---

## 3. Incident Log
- [ ] Any unplanned failures documented in incident-log/
- [ ] Incident register README updated
- [ ] Root cause and prevention documented for each

---

## 4. Workbook Entry
- [ ] Week workbook entry completed before closing terminal
- [ ] All sections filled — Problem Statement, Success Criteria, Failures, Reflections
- [ ] ADR entries added for any design decisions
- [ ] Risk Register updated for any accepted risks
- [ ] Open items listed and carried forward

---

## 5. Open Items
- [ ] All open items documented
- [ ] Each has an owner and a target week
- [ ] Nothing left undocumented

---

## 6. GitHub Commit
- [ ] weekly-workbooks/week-XX.md committed
- [ ] incident-log/ files committed
- [ ] incident-log/README.md updated
- [ ] README.md updated if lab status changed
- [ ] git push confirmed — no local uncommitted changes

---

## 7. Lab State
- [ ] All VMs in correct state — running or shut down intentionally
- [ ] pfSense automatic stop action set to Shutdown
- [ ] No VMs left in Paused or Saved state unintentionally
- [ ] Passwords and credentials stored securely offline

---

## Scope Change Protocol

If anything comes up during the session that is outside the lesson objectives:

1. Stop — flag it explicitly
2. Assess — is this blocking the lesson or just interesting?
3. Decide — defer to future week OR accept as addition to this session
4. Document — if accepted, add to workbook as unplanned work
5. Continue — return to lesson objectives

---

## Session Structure — Every Week

| Phase | Action |
|---|---|
| Start | Review lesson objectives on screen |
| During | Flag any drift — defer or accept explicitly |
| Before close | Run this checklist top to bottom |
| Close | Commit to GitHub — workbook entry done first |
