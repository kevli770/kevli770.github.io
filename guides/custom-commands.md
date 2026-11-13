# Custom Commands: פקודות מותאמות אישית ב-Claude Code

> פקודות מותאמות אישית הן קיצורי דרך שאתם בונים בעצמכם ל-Claude Code. במקום לכתוב שוב ושוב את אותן הוראות, אתם יוצרים קובץ Markdown אחד וקוראים לו דרך פקודת slash.

מאת קווין ליאקס. פורסם 2026-04-27. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/custom-commands

## מה זה Custom Commands

פקודות מותאמות אישית הן קיצורי דרך שאתם בונים בעצמכם ל-Claude Code. במקום לכתוב שוב ושוב את אותן הוראות, אתם יוצרים קובץ Markdown אחד וקוראים לו דרך פקודת slash.

**מתי כדאי? כשאתם מוצאים את עצמכם משתמשים בהוראות חוזרות:**

- בדיקות אבטחה לקוד שנכתב
- יצירת טסטים בפורמט מסוים
- עריכת PR לפי תבנית קבועה
- בדיקת תלויות (dependencies)
- תיקוני פגיעויות
- עיצוב קוד בסגנון שלכם

*הערה חשובה: ב-Claude Code Custom Commands התמזגו עם Skills. הם מאוחסנים ב-*`.claude/skills/` *אבל גם* `.claude/commands/` *עדיין עובד.*

## איפה שמים את הקבצים

| מיקום | נתיב | חל על |
|---|---|---|
| **אישי** | `~/.claude/skills/<name>/SKILL.md` | כל הפרויקטים שלכם |
| **פרויקט** | `.claude/skills/<name>/SKILL.md` | הפרויקט הזה בלבד |
| **Plugin** | `<plugin>/skills/<name>/SKILL.md` | איפה שה-plugin פעיל |
| **Enterprise** | מנוהל דרך managed settings | כל המשתמשים בארגון |

## מדריך שלב אחר שלב

### שלב 1: יצירת תיקייה לפקודה

הפקודה הראשונה שלכם: נקרא לה explain-code:

```bash
mkdir -p ~/.claude/skills/explain-code
```

### שלב 2: יצירת קובץ SKILL.md

בתוך התיקייה, יוצרים קובץ SKILL.md עם 2 חלקים:

- YAML frontmatter (בין סימני ---) שאומר ל-Claude מתי להשתמש
- תוכן markdown עם ההוראות שיבוצעו כשהפקודה מופעלת

### דוגמה לקובץ SKILL.md

```markdown
---
name: explain-code
description: Explains code with diagrams
  and analogies. Use when explaining how
  code works or "how does this work?"
---

When explaining code, always include:

1. Start with an analogy: Compare the code
   to something from everyday life
2. Draw a diagram: Use ASCII art to show
   the flow, structure, or relationships
3. Walk through the code step-by-step
4. Highlight a gotcha or common mistake
```

### שלב 3: שימוש בפקודה

שתי דרכים להפעיל:

- ישירות עם slash:

```
/explain-code src/auth/login.ts
```

- בשפה טבעית: Claude יזהה לבד:

```
"How does this code work?"
```

## שדות Frontmatter

| שדה | נדרש? | מה זה עושה |
|---|---|---|
| `name` | לא | שם הפקודה. אם חסר: שם התיקייה. אותיות קטנות, מספרים, מקפים |
| `description` | **מומלץ** | מה הפקודה עושה ומתי להשתמש. Claude מחליט לפי זה |
| `argument-hint` | לא | טקסט שמופיע באוטוקומפליט, למשל \[filename\] |
| `disable-model-invocation` | לא | true = רק אתם יכולים להפעיל. שימושי ל-/deploy |
| `user-invocable` | לא | false = רק Claude יכול. שימושי לידע רקע |
| `allowed-tools` | לא | כלים מותרים בלי לבקש אישור. למשל Read Grep |
| `model` | לא | איזה מודל להשתמש. למשל opus או sonnet |
| `effort` | לא | רמת חשיבה: low / medium / high / xhigh / max |
| `context` | לא | fork = הרצה ב-subagent מבודד |

## העברת ארגומנטים

אפשר להעביר ערכים לפקודה דרך placeholders:

| Placeholder | מה זה |
|---|---|
| `$ARGUMENTS` | כל הארגומנטים שהועברו, כמחרוזת אחת |
| `$ARGUMENTS[0]` | הארגומנט הראשון בלבד (מתחיל מ-0) |
| `$0, $1, $2` | קיצור ל-$ARGUMENTS\[N\] |
| `${CLAUDE_SESSION_ID}` | ID של השיחה הנוכחית |
| `${CLAUDE_SKILL_DIR}` | התיקייה של ה-skill, שימושי להפניה לקבצים |

### דוגמה עם ארגומנטים

```markdown
---
name: fix-issue
description: Fix a GitHub issue
---

Fix GitHub issue $ARGUMENTS following our
coding standards.

1. Read the issue description
2. Understand the requirements
3. Implement the fix
4. Write tests
5. Create a commit
```

שימוש: `/fix-issue 123`

## דוגמאות מעשיות

### דוגמה 1: בדיקת אבטחה

```markdown
---
name: security-check
description: Scan code for security issues
allowed-tools: Read Grep
---

Scan $ARGUMENTS for security issues:
- SQL injection vulnerabilities
- XSS attack vectors
- Hardcoded secrets or API keys
- Unsafe deserialization

Report findings with severity levels.
```

### דוגמה 2: Deploy עם הגנות

```markdown
---
name: deploy
description: Deploy to production
disable-model-invocation: true
allowed-tools: Bash(npm run *) Bash(git push *)
---

Deploy $ARGUMENTS to production:

1. Run the test suite
2. Build the application
3. Push to deployment target
4. Verify deployment succeeded
```

### דוגמה 3: דינמיק עם פקודות shell

עם הסינטקס `` !`<command>` `` אפשר להריץ פקודות shell ולהזריק את הפלט:

```markdown
---
name: pr-summary
description: Summarize a pull request
allowed-tools: Bash(gh *)
---

## PR context
- Diff: !`gh pr diff`
- Comments: !`gh pr view --comments`
- Files: !`gh pr diff --name-only`

## Task
Summarize this pull request...
```

## טכניקות מתקדמות

### הרצה ב-subagent (context: fork)

מאפשר הרצה במקביל בלי לזהם את ההיסטוריה הראשית:

```markdown
---
name: deep-research
description: Research a topic thoroughly
context: fork
agent: Explore
---

Research $ARGUMENTS thoroughly:
1. Find relevant files using Glob and Grep
2. Read and analyze the code
3. Summarize findings
```

### הגבלת הרשאות

לחסום פקודה ספציפית מ-Claude:

```markdown
# בקובץ permissions:
Skill(deploy *)
```

### דחיפה ל-Git

פקודות פרויקט אפשר ל-commit ל-git כדי לשתף עם הצוות:

```bash
git add .claude/skills/
git commit -m "Add team commands"
```

## טיפים

- תיאור (description) טוב חשוב יותר מהשם, Claude משתמש בו להחליט מתי להפעיל
- כללו מילות מפתח שאנשים יגידו באופן טבעי בתיאור
- פקודות עם תופעות לוואי (deploy, send), תמיד עם disable-model-invocation: true
- שמרו SKILL.md מתחת ל-500 שורות. חומר הפנייה: בקבצים נפרדים
- אם הפקודה לא מופעלת: בדקו עם /skills איזה כן זמין
- שינויים בקבצי skill נכנסים לתוקף מיידית, בלי הפעלה מחדש

## לינקים

- דוקומנטציה רשמית: [Claude Code Skills](https://code.claude.com/docs/en/slash-commands)
- Agent Skills Standard: [agentskills.io](https://agentskills.io)
- הרשאות וכלים: [Permissions docs](https://code.claude.com/docs/en/permissions)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
