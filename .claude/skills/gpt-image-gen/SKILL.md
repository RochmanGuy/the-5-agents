---
name: gpt-image-gen
description: Generate an image via OpenAI Images API (model gpt-image-2). Use when an agent needs to turn a text prompt into a PNG file on disk. Reads OPENAI_API_KEY from .env. Trigger keywords - generate image, create image, image from prompt, gpt-image, יצירת תמונה, צור תמונה.
---

# gpt-image-gen

Skill קומפקטי שמפעיל את **OpenAI Images API** ושומר את התמונה כקובץ PNG על הדיסק. הסקיל הוא מעטפת טכנית בלבד — הוא **לא** בוחר prompt, **לא** בוחר סגנון, **לא** בוחר נתיב פלט. הקורא (בדרך כלל **יובל**) אחראי על כל אלה.

---

## ⚠️ אזהרה קריטית — אל תשנה את שם המודל

**המודל הוא `gpt-image-2`. בדיוק זה.**

- `gpt-image-2` יצא ב-**21 באפריל 2026** ו-**הוא מודל אמיתי וקיים** של OpenAI.
- ייתכן שהידע הפנימי שלך (ה-training cutoff) **לא כולל אותו** — **זה לא אומר שהוא לא קיים**.
- **אסור** להחליף ל-`dall-e-3`, `dall-e-2`, `gpt-image-1`, או כל שם אחר.
- **אם יש שגיאה ב-API**, הבעיה כמעט תמיד אחת מהשתיים:
  1. `OPENAI_API_KEY` ריק, לא תקף, או חסר הרשאות
  2. אחד הפרמטרים (`size`, `quality`, `output_format`) לא חוקי

**שגיאה ב-API ≠ סיבה לשנות את שם המודל.** קודם בודקים את ה-error message שחוזר ב-JSON.

---

## Pre-flight checks (לפני שמפעילים)

1. **בדיקת מפתח** — `Read .env` (אם הסוכן יכול), או `Bash` עם:
   ```bash
   grep -E "^OPENAI_API_KEY=.+" .env || echo "MISSING"
   ```
   אם ריק או חסר → להחזיר שגיאה: *"`OPENAI_API_KEY` ריק ב-`.env`. מלא את המפתח לפני יצירת תמונה."*

2. **בדיקת קלט** — הקורא חייב לספק:
   - `prompt` — טקסט תיאור באנגלית
   - `output_path` — נתיב יחסי לקובץ ה-PNG (לדוגמה `yuval/outputs/2026-05-13-cat-coffee.png`)

   אם אחד מהשניים חסר → להחזיר שגיאה.

3. **יצירת תיקיית יעד** אם לא קיימת:
   ```bash
   mkdir -p "$(dirname '<output-path>')"
   ```

---

## הקריאה ל-API

```bash
# 1) טען את משתני הסביבה מ-.env (אם לא נטענו)
set -a; source .env; set +a

# 2) קרא ל-API ושמור את התגובה לקובץ זמני
curl -sS -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2",
    "prompt": "<the prompt>",
    "size": "1024x1024",
    "quality": "medium",
    "output_format": "png"
  }' > /tmp/gpt-image-response.json
```

**אחרי הקריאה — בדוק שגיאה לפני decode:**

```bash
ERR=$(python -c "import json; d=json.load(open('/tmp/gpt-image-response.json')); print(d.get('error',{}).get('message',''))" 2>/dev/null)
if [ -n "$ERR" ]; then
  echo "API error: $ERR"
  exit 1
fi
```

---

## Decode — שני מסלולים

`jq` לא תמיד מותקן ב-Git Bash על Windows, אז יש נפילה מסודרת ל-python.

### מסלול A — `jq` זמין

```bash
if command -v jq >/dev/null 2>&1; then
  jq -r '.data[0].b64_json' /tmp/gpt-image-response.json | base64 --decode > "<output-path>.png"
fi
```

### מסלול B — `jq` חסר, fallback ל-python

```bash
python -c "
import json, base64, sys
with open('/tmp/gpt-image-response.json') as f:
    d = json.load(f)
b64 = d['data'][0]['b64_json']
with open(sys.argv[1], 'wb') as out:
    out.write(base64.b64decode(b64))
" "<output-path>.png"
```

### לוגיקה משולבת (להעתקה ישירה)

```bash
if command -v jq >/dev/null 2>&1; then
  jq -r '.data[0].b64_json' /tmp/gpt-image-response.json | base64 --decode > "$OUTPUT_PATH"
else
  python -c "
import json, base64, sys
with open('/tmp/gpt-image-response.json') as f:
    d = json.load(f)
open(sys.argv[1], 'wb').write(base64.b64decode(d['data'][0]['b64_json']))
" "$OUTPUT_PATH"
fi
```

---

## Verification אחרי decode

```bash
if [ ! -s "$OUTPUT_PATH" ]; then
  echo "ERROR: output file missing or empty at $OUTPUT_PATH"
  exit 1
fi
echo "OK: image saved at $OUTPUT_PATH ($(wc -c < "$OUTPUT_PATH") bytes)"
```

---

## פרמטרים אופציונליים

| פרמטר | ערכים חוקיים | ברירת מחדל |
|---|---|---|
| `model` | `gpt-image-2` בלבד | `gpt-image-2` |
| `size` | `1024x1024` (square), `1024x1536` (portrait), `1536x1024` (landscape) | `1024x1024` |
| `quality` | `low`, `medium`, `high` | `medium` |
| `output_format` | `png`, `jpeg`, `webp` | `png` |
| `n` | 1-4 (כמה תמונות) | `1` |

הקורא יכול לעדכן את ה-JSON body לפי הצורך. כל שאר השדות מטופלים בברירת מחדל של ה-API.

---

## דוגמה מלאה (end-to-end)

יובל מקבל בקשה "תמונה של חתול שותה קפה בסגנון איור מינימליסטי":

```bash
PROMPT="A minimalist illustration of a cat sitting at a small wooden table, sipping coffee from a white mug. Soft pastel palette, clean white background, gentle morning light."
OUTPUT_PATH="yuval/outputs/2026-05-13-cat-coffee.png"

mkdir -p "$(dirname "$OUTPUT_PATH")"
set -a; source .env; set +a

curl -sS -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "$(python -c "import json,sys; print(json.dumps({'model':'gpt-image-2','prompt':sys.argv[1],'size':'1024x1024','quality':'medium','output_format':'png'}))" "$PROMPT")" \
  > /tmp/gpt-image-response.json

# בדיקת שגיאה
ERR=$(python -c "import json; d=json.load(open('/tmp/gpt-image-response.json')); print(d.get('error',{}).get('message',''))" 2>/dev/null)
[ -n "$ERR" ] && { echo "API error: $ERR"; exit 1; }

# decode
if command -v jq >/dev/null 2>&1; then
  jq -r '.data[0].b64_json' /tmp/gpt-image-response.json | base64 --decode > "$OUTPUT_PATH"
else
  python -c "
import json, base64, sys
d = json.load(open('/tmp/gpt-image-response.json'))
open(sys.argv[1],'wb').write(base64.b64decode(d['data'][0]['b64_json']))
" "$OUTPUT_PATH"
fi

# verify
[ -s "$OUTPUT_PATH" ] && echo "OK: $OUTPUT_PATH" || echo "FAILED"
```

**שימוש ב-`python -c` לבניית ה-JSON** הוא מתחכם בכוונה: מונע escaping של תווים מיוחדים ב-prompt (גרשיים, שורות חדשות, אפוסטרופים) שהיו שוברים את ה-`-d`.

---

## גבולות הסקיל

- **לא בוחר prompt** — הקורא מנסח.
- **לא בוחר סגנון / reference** — באחריות הקורא.
- **לא בוחר נתיב פלט** — הקורא מספק.
- **לא יוצר metadata sidecar** (`.txt` עם היסטוריה) — הקורא יוצר אם רוצה.
- **לא מבצע עריכה / inpainting** — רק generations endpoint.

הסקיל = curl + decode + verify. כל היתר על הסוכן הקורא.

---

## תלויות

- `curl` — מותקן בדרך כלל. אם חסר, יש להתקין.
- `python` — נדרש ל-fallback וגם לבניית JSON. כל סביבת dev מודרנית כוללת.
- `jq` — אופציונלי; אם חסר, fallback ל-python פועל אוטומטית.
- `OPENAI_API_KEY` ב-`.env` — חובה.
