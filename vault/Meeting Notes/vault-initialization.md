# Vault Initialization

## Overview
אתחול ראשוני של ה-Obsidian Vault של הפרויקט והקמת תיעוד מלא של כל קובץ. נוצרו ארבע תיקיות תוכן (`Meeting Notes`, `Content Briefs`, `Brand Guidelines`, `Publishing Log`) ותיקיית תיעוד נוספת (`Project Files`) שמכילה קובץ MD נפרד לכל קובץ ותיקייה בפרויקט — עם הסבר מה הוא עושה, למי הוא משויך (ראובן / יעל / יובל / חן / כל הצוות), ו-wikilinks לכל הקבצים הקשורים. נוסף גם זיכרון feedback קבוע שיאלץ את ראובן להפעיל את [[skill-obsidian-vault-workflow]] בתחילת כל סשן ובכל פקודה חדשה.

## Open Questions
- ההגדרות הפורמליות של [[agents-folder]] (יעל / יובל / חן) עדיין לא נכתבו — רק `.gitkeep`. צריך להחליט מתי להגדיר אותם בפועל.
- [[commands-folder]] ריקה — אילו זרימות עבודה (slash commands) שווה להוסיף ראשונות? למשל `/produce-post`, `/research-topic`.
- אין עדיין נושאים ב-[[content-briefs-index|Content Briefs]] וב-[[brand-guidelines-index|Brand Guidelines]] — נוצרים בעת בריף ראשון.
- האם ראוי להפיק קובץ `.base` ב-`Project Files/` שיתן תצוגת טבלה של בעלות ↔ קובץ?

## Session Log

### 2026-05-13 — Vault initialization & file documentation [shipped]
- **What was done:**
  - יצירת תיקיות `vault/Meeting Notes/`, `vault/Content Briefs/`, `vault/Brand Guidelines/`, `vault/Publishing Log/`, `vault/Project Files/`.
  - כתיבת `_index.md` לכל אחת מהתיקיות.
  - יצירת קובץ MD לכל קובץ/תיקייה בפרויקט תחת `vault/Project Files/`:
    - שורש: [[claude-md]], [[env-example]], [[env-file]], [[gitignore]], [[vault-folder]], [[obsidian-config]].
    - `.claude/`: [[settings-local-json]], [[agents-folder]], [[commands-folder]], [[skills-folder]].
    - 17 קבצי skill: [[skill-brainstorming]], [[skill-dispatching-parallel-agents]], [[skill-executing-plans]], [[skill-finishing-a-development-branch]], [[skill-obsidian-bases]], [[skill-obsidian-markdown]], [[skill-obsidian-vault-workflow]], [[skill-receiving-code-review]], [[skill-requesting-code-review]], [[skill-subagent-driven-development]], [[skill-systematic-debugging]], [[skill-test-driven-development]], [[skill-using-git-worktrees]], [[skill-using-superpowers]], [[skill-verification-before-completion]], [[skill-writing-plans]], [[skill-writing-skills]].
  - שמירת זיכרון feedback קבוע: ראובן חייב להפעיל `obsidian-vault-workflow` בתחילת כל סשן ובכל פקודה חדשה.
- **Decisions:**
  - **תיקייה ייעודית `Project Files/`** במקום פיזור התיעוד בין הקטגוריות הקיימות. למה: התיעוד הזה הוא **רפרנס** (תמיד-נכון), לא **סשן** (תאריך-מוגבל). הפרדה משאירה את `Meeting Notes/` נקייה ליומן.
  - **קובץ אחד לכל skill** (גם לסקילים קטנים) — נאמן להוראת המשתמש "כל קובץ וקובץ", מאפשר wikilinks דקים מקובץ-סשן עתידי.
  - **בעלות מפורשת** בכל קובץ (`למי משויך`) — ראובן/יעל/יובל/חן/כל הצוות — תומך בניתוב בקשות עתידי.
  - **המשך וbinkmark**: ה-skill `obsidian-vault-workflow` סומן במפורש כ-skill מנדטורי-פעמיים — בקובץ ה-skill עצמו, ב-feedback memory, וב-[[claude-md]] (יוסיף עדכון בעתיד).
- **Notes / Caveats:**
  - [[agents-folder]] ו-[[commands-folder]] עדיין ריקים בפועל (רק `.gitkeep`). התיעוד שלהם מתאר את המצב התכנוני.
  - לא תוקן [[claude-md]] להוסיף הפניה לפרוטוקול ה-vault — נשמר כפי שהוא לפי "אל תפעל מעבר למה שהתבקש".
  - הזיכרון נשמר תחת `C:\Users\r0526\.claude\projects\C--Users-r0526-Documents-workspace-the-5-agents\memory\` (אינדקס + `feedback_obsidian_workflow.md`).
- **Related:** [[claude-md]], [[skill-obsidian-vault-workflow]], [[skills-folder]], [[agents-folder]], [[commands-folder]], [[vault-folder]]
