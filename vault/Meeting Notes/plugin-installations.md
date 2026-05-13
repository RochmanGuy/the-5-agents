# Plugin Installations

## Overview
תיעוד התקנות פלאגינים של Claude Code בפרויקט. כל הפלאגינים מותקנים ב-**project scope** כדי שיהיו עקביים בין מכונות שעובדות על אותו ה-repo (התצורה נשמרת ב-`.claude/settings.json` תחת `enabledPlugins`). ה-marketplace הרשמי `claude-plugins-official` רשום בפרופיל המשתמש, ולכן `--scope project` משפיע רק על קובץ ההגדרות המקומי של הפרויקט ולא על המרקטפלייסים.

## Open Questions
- האם להתקין את שאר ה-skills של `anthropic/skills` (pdf, pptx, xlsx, docx, skill-creator) ברמת ה-project כדי שכל שיתופי הצוות יראו אותם זהים, או שמוטב להשאיר אותם ברמת user?
- האם להוסיף תיעוד דומה גם להתקנות marketplace (לא רק plugins)?

## Session Log

### 2026-05-13 — Install skill-creator at project scope [shipped]
- **What was done:**
  - הותקן הפלאגין `skill-creator@claude-plugins-official` ב-scope = project דרך `claude plugin install skill-creator@claude-plugins-official --scope project`.
  - אומת ב-`claude plugin list`: scope = project, enabled = true.
  - נוצר אוטומטית `.claude/settings.json` עם `"enabledPlugins": { "skill-creator@claude-plugins-official": true }` — ייעודי לפרויקט הזה, נכנס ל-git כדי שאותו פלאגין יופעל גם אצל משתפים אחרים של ה-repo.
- **Decisions:**
  - **Project scope ולא user scope** — הסיבה: הפלאגין מותאם לזרימת העבודה של הצוות (יצירת skills חדשים לצוות), ולא להעדפה אישית של המשתמש. הצמדה לפרויקט מבטיחה שכל מי שמקליל את ה-repo בעתיד מקבל אותו setup.
  - **לא צריך לרשום marketplace ידנית** — ה-marketplace `claude-plugins-official` כבר היה רשום ברמת המשתמש, אז הפקודה הראשונה הצליחה מיד והשלבים 2–3 בהוראה לא נדרשו.
- **Notes / Caveats:**
  - ה-CLI של Claude Code לא היה ב-`PATH` של PowerShell כשנקרא בשם `claude`. נדרש להפעיל אותו דרך מסלול מלא: `C:\Users\r0526\AppData\Roaming\Claude\claude-code\2.1.138\claude.exe`. אם בעתיד מריצים פקודות `claude plugin …` מסקריפט, יש להשתמש במסלול המלא או להוסיף אותו ל-PATH.
  - הגרסה ב-`claude plugin list` מוצגת כ-`unknown` — נראה רגיל לפלאגינים מתוך marketplace רשמי.
- **Related:** [[settings-local-json]], [[skills-folder]], [[skill-writing-skills]], [[claude-md]]
