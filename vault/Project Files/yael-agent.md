# .claude/agents/yael.md

## מה זה
קובץ ההגדרה של **יעל**, כותבת התוכן של הצוות. זה ה-sub-agent הראשון שהוגדר בפרויקט. הקובץ הוא flat (`yael.md`, לא תיקייה) — Markdown עם frontmatter שמגדיר שם, תיאור, וכלים, ואחריו system prompt שמכתיב את הזהות, הפרוטוקול, ו-flow העבודה.

## מה יעל עושה
- לוקחת מאמר גלם מ-`Content/` ומשכתבת אותו בסגנון של הצוות.
- שומרת שני קבצים ב-`Output/`: גרסת Markdown נקייה וגרסת HTML עצמאית מעוצבת לקריאה.
- מסירה אוטומטית קישורים/CTAs/חתימות promotional של המחבר המקורי, ושומרת אזכורי מותגים שמופיעים בתוך הסיפור.
- **מזהה איפה המאמר זקוק לתמונה ומשאירה placeholders** — `{{IMAGE_NEEDED: "..."}}` ב-MD ו-`<!-- IMAGE_NEEDED: ... -->` ב-HTML. ראובן יפעיל אחר-כך את [[yuval-agent]] למלא אותם.

## כלים זמינים ליעל
`Read, Write, Edit, Glob, Grep` בלבד. **אין** Bash, **אין** WebSearch, **אין** WebFetch, **אין** גישה ל-API חיצוני, **אין** הפעלת סוכנים אחרים.

## תלויות
- [[yael-folder]] — תיקיית העבודה. יעל קוראת בתחילת כל משימה את `style-guide.md` ואת `reference/`.
- [[content-folder]] — הקלט (מאמרי גלם).
- [[output-folder]] — הפלט (MD + HTML).

## למי משויך
**יעל** — זה קובץ הזהות שלה.

## נתיב
`.claude/agents/yael.md`

## קבצים קשורים
- [[agents-folder]] — התיקייה שבה הקובץ יושב
- [[claude-md]] — מגדיר מתי ראובן מפעיל את יעל (הוראות ניתוב + trigger keywords)
- [[yael-folder]], [[content-folder]], [[output-folder]] — תלויות הקלט/פלט
- [[yael-agent-creation]] — Meeting Note של הסשן שבו יעל נוצרה