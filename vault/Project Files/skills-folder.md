# .claude/skills/

## מה זה
תיקיית **Skills** — מודולים של ידע ופרוטוקולים שכל סוכן יכול להפעיל בעת הצורך. כל skill תת-תיקייה עם `SKILL.md` ולעיתים קבצי עזר.

## תכולה נוכחית (19 skills)
- ניהול עבודה: [[skill-brainstorming]], [[skill-writing-plans]], [[skill-executing-plans]], [[skill-dispatching-parallel-agents]], [[skill-subagent-driven-development]]
- איכות קוד: [[skill-test-driven-development]], [[skill-systematic-debugging]], [[skill-verification-before-completion]], [[skill-receiving-code-review]], [[skill-requesting-code-review]]
- Git & זרימה: [[skill-using-git-worktrees]], [[skill-finishing-a-development-branch]]
- Obsidian: [[skill-obsidian-vault-workflow]], [[skill-obsidian-markdown]], [[skill-obsidian-bases]]
- מטא: [[skill-using-superpowers]], [[skill-writing-skills]]
- יצירת תוכן חזותי: [[skill-gpt-image-gen]] — מעטפת ל-OpenAI Images API (gpt-image-2), משרת את [[yuval-agent]]

## למי משויך
**כל הצוות**. ראובן בוחר איזה skill להפעיל למשימה; כל סוכן יכול לקרוא ל-skill רלוונטי.

## נתיב
`.claude/skills/`

## קבצים קשורים
- [[claude-md]] — נקודת הכניסה שמפנה ל-skills
- [[agents-folder]] — סוכנים שצורכים את הסקילים
- [[commands-folder]] — פקודות שיכולות לפתוח skill
