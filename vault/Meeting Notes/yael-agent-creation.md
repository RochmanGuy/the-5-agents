# Yael Agent Creation

## Overview
הגדרת **יעל** כסוכן הראשון הפעיל בצוות, אחרי שעד עכשיו [[agents-folder]] הכילה רק `.gitkeep`. יעל היא כותבת התוכן — sub-agent של Claude Code, מוגדרת ב-`.claude/agents/yael.md` (קובץ flat עם frontmatter + system prompt בעברית). הזרימה: לוקחת מאמר גלם מ-[[content-folder]], קוראת את הסגנון מ-[[yael-folder]] (`style-guide.md` + `reference/`), משכתבת, ושומרת שני קבצים ב-[[output-folder]] (MD + HTML עצמאי). הכלים שלה מוגבלים ל-`Read, Write, Edit, Glob, Grep` בלבד — אין Bash, WebSearch, גישה ל-API, או הפעלת סוכנים אחרים. בעקבות הסשן הזה התווסף גם סעיף **"הוראות ניתוב"** ב-[[claude-md]] עם trigger keywords (עברית + אנגלית) שמתאר מתי ראובן מפעיל את יעל.

## Open Questions
- `yael/style-guide.md` הוא stub עם TODO — ראובן עדיין לא ניסח בפועל את הטון, המבנה, אוצר המילים, או הדוגמאות. בלי זה, יעל תכתוב לפי הבנה כללית בלבד.
- `yael/reference/` ריקה (רק README) — אין דוגמאות לטקסטים בסגנון של הצוות שיעל תלמד מהם.
- אילו הם **המאמרים הראשונים** שיוזרמו ל-[[content-folder]] לבדיקה?
- האם יעל צריכה לתחזק לוג של מה ששכתבה (היסטוריה ב-`Output/`?), או שכל ריצה היא עצמאית?
- מתי נגדיר את `chen-agent`? (יובל נוצר ב-[[yuval-agent-creation]] בסשן הבא; חן עדיין ממתינה.)
- האם להוסיף פקודת slash `/rewrite <file>` ב-[[commands-folder]] שתפעיל את יעל אוטומטית?

## Session Log

### 2026-05-13 — יצירת יעל כסוכן ראשון [shipped]
- **What was done:**
  - יצירת `.claude/agents/yael.md` — frontmatter (`name: yael`, `tools: Read, Write, Edit, Glob, Grep`, description עם trigger keywords) + system prompt בעברית שמגדיר זהות, פרוטוקול, flow, כללי תוכן, וגבולות.
  - יצירת תיקיית `yael/` עם `style-guide.md` (stub) ו-`reference/README.md`.
  - יצירת `Content/.gitkeep` ו-`Output/.gitkeep`.
  - עדכון [[claude-md]]: הוספת סעיף **"## הוראות ניתוב"** עם תת-סעיף ליעל (מתי להפעיל, trigger keywords עברית+אנגלית, כלים זמינים, נתיב הסוכן). הסעיף בנוי כך שייקלטו אליו יובל וחן בעתיד.
  - עדכון [[agents-folder]] — מ"כרגע ריקה" ל"מכילה את [[yael-agent]]"; הוספת wikilinks לכל הקבצים הקשורים.
  - יצירת תיעוד חדש ב-vault: [[yael-agent]], [[yael-folder]], [[content-folder]], [[output-folder]] תחת `Project Files/`, ועדכון של [[_index|Project Files index]].
- **Decisions:**
  - **לא להגדיר model ב-frontmatter** — להשאיר ירושה מההורה. למה: המשתמש לא הביע העדפה והברירת-מחדל סבירה לכתיבה. אם יתברר שצריך מודל יותר חזק/חלש לכתיבה, אפשר לעדכן ב-frontmatter בעתיד בלי לגעת ב-system prompt.
  - **system prompt בעברית** — עקבי עם שאר הפרויקט (CLAUDE.md, vault). למה: כך יעל "חושבת" באותה שפה של המשתמש והצוות. ה-frontmatter description גם הוא דו-לשוני כדי שה-trigger keywords יעבדו לבקשות באנגלית.
  - **חוקי הסרת promo רק לקישורי המחבר** — הבחנה מפורשת ב-system prompt בין "אזכור מותג בתוך הסיפור" (נשאר) ל"קישור החוצה לבלוג/ניוזלטר" (יוצא). הסיבה: דרישה מפורשת של המשתמש שצוטטה verbatim ב-CLAUDE.md הראשוני שלו.
  - **HTML single-file inline CSS, לא CDN** — למה: רוב ה-CDN לא יעבדו ב-archive offline, ו-CSS frameworks (Tailwind, Bootstrap) הם overkill לעמוד טקסט. system fonts בלבד, ~720px רוחב מקסימלי, line-height נדיב. RTL/LTR אוטומטי לפי שפת המאמר.
  - **`yael/` בשורש ולא תחת `.claude/`** — למה: זה תוכן עבודה (style-guide, references), לא תצורה של Claude Code. הפרדה משאירה את `.claude/` נקייה לקבצי תצורה בלבד.
- **Notes / Caveats:**
  - `style-guide.md` ו-`reference/` ריקים — יעל תעבוד מיד אבל ללא העדפות סגנוניות ספציפיות. בסיכום שלה היא תציין זאת מפורשות.
  - לא יצרתי `yuval.md` או `chen.md` — מחוץ לתחום הסשן הזה.
  - הסעיף "מבנה התיקיות" ב-CLAUDE.md עדיין מתאר את `.claude/agents/` כללית, בלי לפרט שיש בה את `yael.md`. סעיף "הוראות ניתוב" החדש כבר עושה זאת, אז אין כפילות נדרשת.
- **Related:** [[agents-folder]], [[yael-agent]], [[yael-folder]], [[content-folder]], [[output-folder]], [[claude-md]], [[vault-initialization]]
