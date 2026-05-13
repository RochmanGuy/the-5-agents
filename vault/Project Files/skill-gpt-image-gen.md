# skill: gpt-image-gen

## מה זה
Skill קומפקטי שמפעיל את **OpenAI Images API** עם המודל `gpt-image-2` ושומר את הפלט כקובץ PNG על הדיסק. מעטפת טכנית בלבד: curl ל-API, decode של ה-base64, ו-verification. **לא** בוחר prompt, **לא** בוחר סגנון, **לא** בוחר נתיב פלט — הקורא (יובל) אחראי על כל אלה.

## למה זה הסקיל הראשון של הצוות שמשתמש ב-API חיצוני
זה ה-Skill הראשון בפרויקט שיוצא מהמכונה — קורא ל-OpenAI דרך `OPENAI_API_KEY`. הוא משרת ייעודית את [[yuval-agent]], אבל בעיקרון כל סוכן עם Bash + מפתח יכול להפעיל אותו. הוא מובנה כקומפוננטה עצמאית כדי שיובל יישאר רזה ויתרכז ב"איזה prompt לשלוח", לא ב"איך לקרוא ל-API".

## נקודה חשובה — שם המודל
המודל הוא `gpt-image-2` בלבד. הוא יצא ב-21 באפריל 2026. אם הידע הפנימי של מודל Claude לא כולל אותו — זה לא אומר שהוא לא קיים. **אסור** להחליף ל-`dall-e-3`, `dall-e-2`, `gpt-image-1`. שגיאה ב-API בדרך כלל = בעיה במפתח או בפרמטרים, לא בשם המודל. הסקיל מציין את זה מפורשות בראש המסמך כאזהרה גלויה.

## מבנה הסקיל
1. **Pre-flight checks** — לוודא ש-`OPENAI_API_KEY` ב-`.env` לא ריק; לוודא שהקורא סיפק `prompt` ו-`output_path`; ליצור תיקיית יעד אם לא קיימת.
2. **קריאה ל-API** דרך curl עם JSON body (model, prompt, size, quality, output_format).
3. **בדיקת שגיאת API** לפני decode (לוודא שאין `error.message` ב-JSON).
4. **Decode** — fork ל-`jq` אם זמין, fallback ל-python (חשוב ב-Git Bash על Windows שלא תמיד מתקין jq).
5. **Verification** — לוודא שהקובץ קיים ו-`size > 0`.

## פרמטרים אופציונליים
`size` (1024x1024 / 1024x1536 / 1536x1024), `quality` (low/medium/high), `output_format` (png/jpeg/webp), `n` (1-4). כולם עם ברירת מחדל.

## למי משויך
**יובל** — המשתמש העיקרי. בעיקרון, כל סוכן עם Bash + `OPENAI_API_KEY` יכול לקרוא לסקיל.

## נתיב
`.claude/skills/gpt-image-gen/SKILL.md`

## תלויות
- `curl` (מותקן בדרך כלל).
- `python` (נדרש ל-fallback decode + לבניית JSON ללא escaping).
- `jq` (אופציונלי; אם חסר, fallback אוטומטי).
- [[env-file]] עם `OPENAI_API_KEY` תקף.

## קבצים קשורים
- [[yuval-agent]] — הצרכן העיקרי
- [[skills-folder]] — התיקייה
- [[env-example]], [[env-file]] — מקור המפתח
- [[claude-md]] — מגדיר את החיבור יעל↔יובל שעובד דרך הסקיל הזה
- [[yuval-agent-creation]] — Meeting Note של ההקמה
