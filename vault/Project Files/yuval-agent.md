# .claude/agents/yuval.md

## מה זה
קובץ ההגדרה של **יובל**, מעצב התמונות של הצוות. ה-sub-agent השני בפרויקט (אחרי [[yael-agent]]). הקובץ הוא flat (`yuval.md`, לא תיקייה) — Markdown עם frontmatter שמגדיר שם, תיאור, וכלים, ואחריו system prompt בעברית שמכתיב זהות, פרוטוקול 3-שלבים (לימוד reference → ניסוח prompt → קריאה לסקיל), שמירת prompt-history, verification, ודיווח מובנה.

## מה יובל עושה
- מקבל בקשת תמונה מראובן, סורק את `yuval/reference/` ללימוד סגנון הצוות (תמונות + הערות).
- מנסח prompt באנגלית שמשלב את הבקשה עם מאפייני הסגנון שחילץ.
- קורא ל-skill [[skill-gpt-image-gen]] שמפעיל את OpenAI Images API עם מודל `gpt-image-2`.
- שומר את ה-PNG ב-`yuval/outputs/<YYYY-MM-DD>-<slug>.png` + sibling `.txt` עם prompt-history מלא לאיטרציה.
- מוודא ש-output קיים ולא ריק, ומחזיר לראובן דיווח מובנה (נתיב, references שהשפיעו, prompt סופי).

## כלים זמינים ליובל
`Read, Write, Edit, Bash, Glob`. **Bash** דרוש כי הסקיל קורא ל-curl ול-decode (jq/python). **Edit** נכלל כדי שיוכל לעדכן את ה-`.txt` של היסטוריית prompts באיטרציה. **אין** WebSearch, **אין** WebFetch, **אין** הפעלת סוכנים אחרים.

## תלויות
- [[yuval-folder]] — תיקיית העבודה. יובל סורק את `reference/` בתחילת כל בקשה ושומר ב-`outputs/`.
- [[skill-gpt-image-gen]] — מעטפת ה-API. בלעדיו יובל לא יכול לייצר תמונה.
- [[env-file]] — חייב להכיל `OPENAI_API_KEY` תקף.

## למי משויך
**יובל** — זה קובץ הזהות שלו.

## נתיב
`.claude/agents/yuval.md`

## קבצים קשורים
- [[agents-folder]] — התיקייה שבה הקובץ יושב
- [[claude-md]] — מגדיר מתי ראובן מפעיל את יובל + תהליך החיבור עם יעל
- [[yuval-folder]] — תיקיית העבודה
- [[skill-gpt-image-gen]] — הסקיל שיובל קורא לו
- [[yael-agent]] — שותפת לתהליך "מאמר עם תמונות"
- [[yuval-agent-creation]] — Meeting Note של הסשן שבו יובל נוצר
