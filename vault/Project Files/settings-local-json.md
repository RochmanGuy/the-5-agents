# .claude/settings.local.json

## מה זה
הגדרות מקומיות של Claude Code: בעיקר הרשאות (`allow`/`deny`) לפעולות מסוכנות. גיט-איגנור.

## מה כתוב בו כרגע
מתיר פקודת `mkdir -p` להקמת `.claude/agents`, `.claude/skills`, `.claude/commands` בלבד.

## למי משויך
**ראובן** — מנהל את ההרשאות של הצוות מול הסביבה המקומית.

## נתיב
`.claude/settings.local.json`

## קבצים קשורים
- [[gitignore]] — מאפשר לכל מפעיל להגדיר הרשאות שונות בלי לדרוס
- [[claude-md]] — הסביבה ש-settings.json משלים אותה
