# CLAUDE.md: קובץ הזיכרון של Claude Code

> CLAUDE.md הוא קובץ זיכרון של Claude Code. כמו שאתם מגדירים שעון מעורר באייפון או בוחרים תמונת רקע בטלפון, כך אתם מגדירים כללים והעדפות בקובץ הזה.

מאת קווין ליאקס. פורסם 2026-04-24. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/claude-md

## מה זה CLAUDE.md

CLAUDE.md הוא קובץ זיכרון של Claude Code. כמו שאתם מגדירים שעון מעורר באייפון או בוחרים תמונת רקע בטלפון, כך אתם מגדירים כללים והעדפות בקובץ הזה.

**Claude קורא אותו כל פעם שמתחילים שיחה, וכך הוא יודע איך לעבוד איתכם לפני שאמרתם מילה.**

### איפה שמים את הקובץ

- גלובלי (לכל הפרויקטים): \~/.claude/CLAUDE.md
- לפרויקט ספציפי: בשורש הפרויקט, CLAUDE.md

*אין תשובה אחת נכונה למה לכתוב. זה תלוי בפרויקט שלכם ובאיך שאתם עובדים.*

## השיטה של בוריס צ'רני

בוריס צ'רני, האדם שיצר את Claude Code ב-Anthropic, משתף ב-X שיטות עבודה פנימיות. מישהו הפך את הפוסטים שלו לקובץ CLAUDE.md מוכן לשימוש. הנה 6 הכללים:

## 1. לתכנן לפני שמתחילים

הכלל הזה אומר ל-Claude לחשוב על תוכנית לפני שהוא קופץ למשימה. במקום מיד לכתוב קוד, Claude עוצר, מציג את הגישה שלו, ורק כשהתוכנית נראית טוב, מתחיל לבנות.

אם משהו משתבש? הוא עוצר ומתכנן מחדש במקום לדחוף קדימה עם גישה שבורה.

### הקוד ב-CLAUDE.md

```markdown
## Workflow Orchestration
### 1. Plan Node Default
- Enter plan mode for ANY non-trivial task
  (3+ steps or architectural decisions)
- If something goes sideways, STOP and
  re-plan immediately
- Use plan mode for verification steps too
- Write detailed specs upfront
```

## 2. סוכנים במקביל

זה כמו להעסיק צוות מטבח במקום לנסות לבשל את כל הארוחה לבד. הכלל הזה אומר ל-Claude להשתמש בסוכנים נפרדים למשימות כמו מחקר או חיפוש, כך השיחה הראשית נשארת נקייה ומסודרת.

### הקוד ב-CLAUDE.md

```markdown
### 2. Subagent Strategy
- Use subagents liberally to keep main
  context window clean
- Offload research, exploration, and
  parallel analysis to subagents
- For complex problems, throw more compute
  at it via subagents
- One task per subagent for focused execution
```

## 3. לולאת שיפור עצמי

Claude מתחיל להתנהג כמו העובד הכי טוב שלכם. אחרי כל תיקון שאתם נותנים לו, הוא מעדכן קובץ לקחים, ולא עושה את אותה טעות פעמיים.

### הקוד ב-CLAUDE.md

```markdown
### 3. Self-Improvement Loop
- After ANY correction from the user:
  update tasks/lessons.md with the pattern
- Write rules for yourself that prevent
  the same mistake
- Ruthlessly iterate on these lessons
- Review lessons at session start
```

## 4. אימות לפני סיום

Claude אף פעם לא מסמן משימה כ-"בוצע" בלי להוכיח שזה עובד. הוא מריץ בדיקות, בודק לוגים, ושואל את עצמו "האם הייתי מאשר את זה?"

### הקוד ב-CLAUDE.md

```markdown
### 4. Verification Before Done
- Never mark a task complete without
  proving it works
- Diff behavior between main and your changes
- Ask yourself: "Would a staff engineer
  approve this?"
- Run tests, check logs, demonstrate correctness
```

## 5. לדרוש אלגנטיות

לפני שמציגים פתרון, לעצור ולשאול "יש דרך יותר אלגנטית?" אם התיקון מרגיש hacky, לחשוב מחדש. אבל לא להגזים עם תיקונים פשוטים.

### הקוד ב-CLAUDE.md

```markdown
### 5. Demand Elegance (Balanced)
- For non-trivial changes: pause and ask
  "is there a more elegant way?"
- If a fix feels hacky: implement the
  elegant solution
- Skip this for simple, obvious fixes
- Challenge your own work before presenting
```

## 6. תיקון באגים אוטונומי

כשנותנים ל-Claude דיווח על באג, הוא פשוט מתקן. לא שואל איך, לא מבקש הכוונה. מצביע על לוגים, שגיאות, טסטים נכשלים, ופותר. אפס החלפת הקשר מכם.

### הקוד ב-CLAUDE.md

```markdown
### 6. Autonomous Bug Fixing
- When given a bug report: just fix it
- Point at logs, errors, failing tests
  - then resolve them
- Zero context switching required from user
- Go fix failing CI tests without being told
```

## ניהול משימות

הקובץ גם כולל תהליך לניהול משימות:

```markdown
## Task Management
1. Plan First: Write plan to tasks/todo.md
2. Verify Plan: Check in before implementation
3. Track Progress: Mark items complete as you go
4. Explain Changes: High-level summary at step
5. Document Results: Add review section
6. Capture Lessons: Update tasks/lessons.md
```

## עקרונות ליבה

```markdown
## Core Principles
- Simplicity First: Make every change as
  simple as possible. Minimal code impact.
- No Laziness: Find root causes. No temp fixes.
  Senior developer standards.
- Minimal Impact: Touch only what's necessary.
  Avoid introducing bugs.
```

## איך להשתמש בקובץ

### אפשרות 1: להעתיק את הקובץ המוכן

לקחת את הקובץ המלא מהלינק ולשים בשורש הפרויקט:

```markdown
# להוריד את הקובץ:
# https://gist.github.com/hqman/
#   e29cb6386c539d795767e8c3fd2c959b

# לשים בשורש הפרויקט:
CLAUDE.md
```

### אפשרות 2: להתאים לעצמכם

הקובץ הזה הוא נקודת התחלה. אתם יכולים להוסיף, למחוק, לשנות. דוגמאות:

- להוסיף כללים ספציפיים לפרויקט (שפה, פריימוורק, סטק)
- להוסיף "תמיד תכתוב בעברית" או "תמיד תעשה commit אחרי כל שינוי"
- למחוק כללים שלא רלוונטיים לפרויקט שלכם
- להוסיף נתיבים לכלים ו-APIs שהפרויקט משתמש בהם

## לינקים

- הקובץ המלא: [GitHub Gist, Boris CLAUDE.md](https://gist.github.com/hqman/e29cb6386c539d795767e8c3fd2c959b)
- דוקומנטציה רשמית: [Claude Code Docs, CLAUDE.md](https://docs.anthropic.com/en/docs/claude-code/claude-md)
- קורס חינם: [anthropic.com/learn](https://www.anthropic.com/learn)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
