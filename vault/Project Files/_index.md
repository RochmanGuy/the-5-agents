# Project Files — Index

מילון התיעוד של כל קובץ ותיקייה בפרויקט: מה הוא עושה, למי הוא משויך, ועם איזה קבצים אחרים הוא מקושר. נקודת התחלה לכל מי שצריך להתמצא במבנה הפרויקט.

## שורש הפרויקט (Root)

- [[claude-md]] — הגדרת ראובן (המנכ"ל) ותיאור הצוות והמבנה — נקודת הכניסה של Claude Code לפרויקט
- [[env-example]] — תבנית למשתני סביבה (Anthropic / OpenAI / Tavily / מודל ברירת מחדל)
- [[env-file]] — הקובץ הסודי בפועל (לא ב-git)
- [[gitignore]] — קבצים שאסור לדחוף ל-git
- [[vault-folder]] — תיקיית ה-Obsidian Vault — הזיכרון ארוך הטווח של הצוות
- [[obsidian-config]] — תצורת Obsidian (תחת `vault/.obsidian/`)
- [[yael-folder]] — תיקיית העבודה של יעל (style-guide + reference)
- [[yuval-folder]] — תיקיית העבודה של יובל (reference + outputs)
- [[content-folder]] — תיקיית הקלט של יעל (מאמרי גלם)
- [[output-folder]] — תיקיית הפלט של יעל (קבצי MD + HTML)

## תיקיית `.claude/` — תצורת Claude Code

- [[settings-local-json]] — הרשאות מקומיות של Claude Code (מותר/אסור להריץ)
- [[agents-folder]] — תיקיית הגדרות הסוכנים — מכילה את [[yael-agent]] ו-[[yuval-agent]]
- [[yael-agent]] — קובץ ההגדרה של יעל, כותבת התוכן
- [[yuval-agent]] — קובץ ההגדרה של יובל, מעצב התמונות
- [[commands-folder]] — תיקיית פקודות מותאמות לזרימות עבודה — כרגע ריקה
- [[skills-folder]] — תיקיית היכולות (Skills) הזמינות לצוות

## Skills

- [[skill-brainstorming]] — סיעור מוחות לפני יצירת פיצ'רים או תוכן
- [[skill-dispatching-parallel-agents]] — הפעלת סוכנים במקביל למשימות עצמאיות
- [[skill-executing-plans]] — הרצת תכנית מימוש שנכתבה מראש
- [[skill-finishing-a-development-branch]] — סיום ענף פיתוח (merge / PR / cleanup)
- [[skill-obsidian-bases]] — יצירת ועריכת קבצי `.base` ב-Obsidian
- [[skill-obsidian-markdown]] — כתיבת Markdown בסגנון Obsidian (wikilinks, callouts, frontmatter)
- [[skill-obsidian-vault-workflow]] — הפרוטוקול הקבוע של ה-vault — קריאה לפני וכתיבה אחרי כל משימה
- [[skill-receiving-code-review]] — איך לקבל ביקורת קוד עם קפדנות טכנית
- [[skill-requesting-code-review]] — איך לבקש ביקורת קוד אחרי השלמת משימה
- [[skill-subagent-driven-development]] — פיתוח דרך סוכני-עזר כשהמשימות עצמאיות
- [[skill-systematic-debugging]] — חקירת תקלות באופן שיטתי לפני תיקון
- [[skill-test-driven-development]] — TDD: כתוב טסט, צפה שייפול, כתוב מינימום, רפקטור
- [[skill-using-git-worktrees]] — בידוד עבודה בעזרת git worktree
- [[skill-using-superpowers]] — מבוא כללי לעבודה עם skills
- [[skill-verification-before-completion]] — וידוא ראיות לפני הצהרת השלמה
- [[skill-writing-plans]] — כתיבת תכנית מימוש מסודרת לפני קוד
- [[skill-writing-skills]] — יצירה ועריכה של skills חדשים
- [[skill-gpt-image-gen]] — מעטפת ל-OpenAI Images API (gpt-image-2) — משרת את יובל
