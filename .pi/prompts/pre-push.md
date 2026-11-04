---
description: Run the cocoex-website pre-push verification checklist
---
Run the pre-push checklist for cocoex-website against the current changes. Report each
item as pass / fail / needs-manual-check, with the exact command or file to look at:

- [ ] Cross-browser: Chrome, Firefox, Safari, Edge (note what can only be eyeballed)
- [ ] Mobile: iOS Safari, Android Chrome
- [ ] `prefers-reduced-motion` verified
- [ ] Keyboard navigation intact
- [ ] Lighthouse 95+ (all categories) — note if it needs a manual run
- [ ] Any hardcoded `vh` introduced? (grep — must be none outside the documented exception)
- [ ] DPR cap + single-RAF loop + WebGL gating untouched
- [ ] Docs reconciled (suggest running the `doc-minder` skill)

Do not commit or push. $ARGUMENTS
