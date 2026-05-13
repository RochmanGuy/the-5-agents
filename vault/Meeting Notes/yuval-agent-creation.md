# Yuval Agent Creation

## Overview
הגדרת **יובל** כסוכן השני בצוות (אחרי [[yael-agent-creation|יעל]]), יחד עם הסקיל הראשון של הצוות שמשתמש ב-API חיצוני — [[skill-gpt-image-gen]] (מעטפת ל-OpenAI Images API עם המודל `gpt-image-2`). יובל מוגדר ב-`.claude/agents/yuval.md` עם כלים `Read, Write, Edit, Bash, Glob` (Bash דרוש לקריאת ה-API). תיקיית עבודה ב-`yuval/` עם `reference/` ו-`outputs/` (אותו דפוס כמו [[yael-folder]]). הסקיל ב-`.claude/skills/gpt-image-gen/SKILL.md` עם אזהרה גלויה לא להחליף את שם המודל (`gpt-image-2` יצא 21/04/2026, אמיתי גם אם לא מופיע ב-training cutoff של Claude), pre-flight checks למפתח, ו-decode עם fallback מ-jq ל-python (חשוב ב-Git Bash על Windows). בנוסף, [[claude-md|CLAUDE.md]] עודכן עם תת-סעיף ניתוב ליובל וסעיף חדש "תהליכים משולבים" שמתאר את החיבור יעל↔יובל: יעל משאירה placeholders של `{{IMAGE_NEEDED}}` ב-MD ו-`<!-- IMAGE_NEEDED -->` ב-HTML, ראובן מפעיל את יובל למלא, וראובן משלב חזרה.

## Open Questions
- `yuval/reference/` ריקה (רק README) — אין דוגמאות סגנון. עד שראובן/המשתמש יוסיף תמונות והערות, יובל יעבוד עם סגנון ניטרלי וייציין זאת בדיווח.
- ה-`OPENAI_API_KEY` ב-`.env` ריק — המשתמש צריך למלא לפני הרצה ראשונה. הסקיל בודק זאת ב-pre-flight ומחזיר שגיאה תיאורית אם חסר.
- האם להוסיף פקודת slash `/illustrate-article <path>` ב-[[commands-folder]] שתפעיל אוטומטית את הזרימה יעל→יובל ללא תיאום ידני של ראובן?
- האם להגדיר את **חן** (החוקרת) בסשן הבא, או לחכות ליצירת מאמר מלא קודם כדי לבחון את החיבור יעל↔יובל בפועל?
- ה-skill `gpt-image-gen` כתוב במונחי Bash/curl. האם לעטוף ב-Python script ייעודי לעיתיד (יותר עמיד ל-cross-platform), או להישאר עם Bash כל עוד הצוות עובד ב-Git Bash על Windows?

## Session Log

### 2026-05-13 — יצירת יובל וסקיל gpt-image-gen [shipped]
- **What was done:**
  - יצירת `.claude/skills/gpt-image-gen/SKILL.md` — frontmatter (name, description עם trigger keywords) + מסמך מלא: אזהרה בולטת על שם המודל, pre-flight checks, curl ל-API, בדיקת `error.message` לפני decode, decode עם fallback מ-jq ל-python, verification של גודל הקובץ, פרמטרים אופציונליים (size/quality/format/n), דוגמה מלאה end-to-end, וגבולות (הסקיל לא בוחר prompt/style/path).
  - יצירת `.claude/agents/yuval.md` — frontmatter (`name: yuval`, `tools: Read, Write, Edit, Bash, Glob`, description דו-לשונית) + system prompt בעברית: זהות, פרוטוקול 3-שלבים (לימוד reference → חילוץ מאפיינים → התאמה), ניסוח prompt באנגלית, קריאה לסקיל, שמירת prompt-history ב-sibling `.txt`, verification, ופורמט דיווח מובנה.
  - יצירת `yuval/reference/README.md` + `yuval/outputs/.gitkeep`.
  - עדכון `CLAUDE.md`: תת-סעיף "יובל — מעצב התמונות" בהוראות ניתוב (מקביל לתת-הסעיף של יעל), סעיף חדש "תהליכים משולבים" עם תהליך "מאמר עם תמונות" (5 שלבים), הוספת שורות `yuval/` ו-`yael/`/`Content/`/`Output/` למבנה התיקיות, ועדכון פסקת "הערה" — יעל ויובל פעילים, חן עוד לא.
  - עדכון `.claude/agents/yael.md`: סעיף חדש "זיהוי צרכי תמונה" עם פורמטים מקבילים ל-MD ול-HTML, כללי placeholder (אנגלית, 2-4 משפטים, סגנון כללי, עד 3 placeholders למאמר רגיל / 5 לארוך), עדכון "גבולות" — יעל מזהה אבל לא מייצרת, ועדכון "איך לחזור לראובן" — חובה לציין את רשימת ה-placeholders בסיכום.
  - תיעוד vault: [[yuval-agent]], [[yuval-folder]], [[skill-gpt-image-gen]] חדשים ב-`Project Files/`; עדכון [[agents-folder]], [[skills-folder]], [[claude-md]], [[yael-agent]], ו-[[_index|Project Files index]].
- **Decisions:**
  - **כלי `Edit` ליובל בנוסף ל-Read/Write/Bash/Glob.** המשתמש השאיר את זה לשיקול דעת. למה Edit: יובל צריך לעדכן את ה-`.txt` של prompt-history באיטרציה (משנה רק את ה-prompt, לא כותב מחדש את כל ההיסטוריה). Edit מאפשר fine-grained changes ולא מאלץ Write מלא.
  - **לא לגעת ב-`.env`/`.env.example`.** שניהם **כבר מכילים** `OPENAI_API_KEY` (ה-example עם placeholder, ה-`.env` עם ריק). המשתמש ימלא את הערך האמיתי ידנית.
  - **`yuval/outputs/` עם `.gitkeep` ולא ב-`.gitignore`.** מבנה התיקייה צריך להיות ב-git כדי שיובל ימצא אותה אצל מי שמשכפל. הקבצים הספציפיים שיובל יוצר (תמונות PNG + history TXT) יידחפו לגיט — זה ה-deliverable של הצוות, יש ערך בהיסטוריה.
  - **fallback מ-jq ל-python ב-decode** ולא רק jq. למה: Git Bash על Windows (סביבת המשתמש) לא תמיד כולל jq. python כן (כלול ב-Anaconda / WinPython / כל setup מודרני). הסקיל בודק `command -v jq` ובוחר אוטומטית.
  - **שני פורמטים מקבילים של placeholder** (`{{IMAGE_NEEDED}}` ב-MD, `<!-- IMAGE_NEEDED -->` ב-HTML), לא רק MD. למה: HTML נצרך ישירות (פתיחה בדפדפן), ו-`{{...}}` יציג כתוכן ויפר את הקריאה. קומנט מסתיר אבל נשאר ניתן לחיפוש/החלפה אוטומטית.
  - **אזהרה כפולה על שם המודל** — גם בסקיל וגם בסוכן. למה: זה הסיכון העיקרי — מודל Claude עלול "לתקן" את שם המודל ל-`dall-e-3` או `gpt-image-1` בגלל training cutoff. שתי הדגשות מקבילות (אזהרה גלויה בראש הסקיל + תזכורת קצרה ב-system prompt של יובל) מורידות סיכון בלי overhead משמעותי.
  - **system prompts בעברית עם prompts באנגלית** — אותו דפוס כמו יעל. למה: יובל "חושב" באותה שפה של המשתמש, אבל ה-prompt ל-gpt-image-2 באנגלית כי האיכות טובה יותר. יעל לכן כותבת את התיאורים של ה-placeholders באנגלית כדי שיעברו כמו שהם.
- **Notes / Caveats:**
  - `yuval/reference/` ריקה — יובל יעבוד מיד אבל ייפול לסגנון ניטרלי. המשתמש צריך לאסוף 3-5 תמונות+הערה לפני סשן ה-styling הראשון.
  - `OPENAI_API_KEY` ריק ב-`.env` — חובה למלא לפני הרצה ראשונה. הסקיל מחזיר שגיאה תיאורית אם ריק.
  - ה-`python -c` בדוגמה לבניית JSON ב-curl מתחכם בכוונה — מונע escaping של גרשיים/אפוסטרופים ב-prompt. זה לא הכי קריא, אבל יציב ל-prompts מורכבים.
  - לא יצרתי `chen.md` — מחוץ לתחום הסשן.
  - הסקיל לא תומך ב-image edit / inpainting — רק `/v1/images/generations`. אם יידרש בעתיד edit, יהיה צריך סקיל נפרד.
- **Related:** [[agents-folder]], [[yuval-agent]], [[yuval-folder]], [[skill-gpt-image-gen]], [[claude-md]], [[yael-agent]], [[yael-agent-creation]], [[env-example]], [[env-file]], [[skills-folder]]
