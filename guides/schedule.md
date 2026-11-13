# Claude Schedule: משימות מתזמנות, Dispatch ו-Cowork

> Claude מאפשר לתזמן משימות שרצות אוטומטית, סקירת קוד, ניטור CI, סיכום יומי.

מאת קווין ליאקס. פורסם 2026-04-04. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/schedule

## מה זה Claude Schedule

Claude מאפשר לתזמן משימות שרצות אוטומטית, סקירת קוד, ניטור CI, סיכום יומי. יש 3 רמות:

| רמה | איפה רץ | צריך מחשב | מרווח מינימלי |
|---|---|---|---|
| **Cloud Tasks** | שרתי Anthropic | לא | 1 שעה |
| **Desktop Tasks** | המחשב שלך | כן (app פתוח) | 1 דקה |
| `/loop (CLI)` | המחשב שלך | כן (CLI פתוח) | 1 דקה |

## Cowork: איפה זה נראה

ב-Claude Desktop, לוחצים על Cowork בתפריט העליון. בסיידבאר רואים את Scheduled ו-Dispatch:

![Cowork: איפה זה נראה](https://kevli770.github.io/_astro/01.ao2Famox.png)

## Cloud Tasks: משימות בענן

האפשרות החזקה. רצות על שרתי Anthropic גם כשהמחשב כבוי. בכל ריצה משכפל את ה-repo מ-GitHub מחדש.

### איך יוצרים

- דרך האתר: claude.ai/code/scheduled
- דרך Desktop: Cowork \> Scheduled \> New remote task
- דרך CLI: /schedule או /schedule daily PR review at 9am

### הגדרות

- שם + prompt עצמאי
- בחירת מודל + repo מ-GitHub
- תדירות: Hourly / Daily / Weekdays / Weekly
- branches: ברירת מחדל רק claude/ prefix

## Desktop Tasks: משימות מקומיות

רצות על המחשב שלך עם גישה לקבצים מקומיים. דורש שה-Desktop app יהיה פתוח והמחשב ער.

- Cowork \> Scheduled \> New task \> New local task
- להגדיר: שם, prompt, תדירות, מודל, תיקיית עבודה
- או להגיד ל-Claude: "תתזמן סקירת קוד יומית ב-9 בבוקר"

### טיפ: הרצה ראשונה

אחרי יצירה, ללחוץ Run Now, לצפות בבקשות הרשאות, ולבחור always allow. הרצות הבאות יעברו חלק.

## /loop: פולינג מהיר ב-CLI

הדרך הקלה. עובד רק בתוך הסשן הנוכחי. נסגר כשיוצאים.

```
/loop 5m check if the deployment finished
/loop check the build every 2 hours
/loop 20m /review-pr 1234
/loop check the build   # default: every 10m
```

### תזכורות חד-פעמיות

```
remind me at 3pm to push the release branch
in 45 minutes, check the integration tests
```

## Cron: תחביר קסטום

ל-Cloud tasks אפשר להגדיר cron מותאם אישית דרך `/schedule update`. הפורמט: 5 שדות, דקה שעה יום-בחודש יום-בשבוע

| ביטוי | משמעות |
|---|---|
| `*/5 * * * *` | כל 5 דקות |
| `0 9 * * *` | כל יום ב-9 בבוקר |
| `0 9 * * 1-5` | ימי חול ב-9 בבוקר |
| `0 * * * *` | כל שעה עגולה |
| `0 0 * * 0` | כל יום ראשון בחצות |

*כל הזמנים בשעון מקומי (לא UTC).*

## ניהול משימות

| פעולה | איך |
|---|---|
| **רשימה** | /schedule list או לשאול "מה המשימות שלי?" |
| **הרצה מיידית** | Run Now בדף המשימה, או /schedule run |
| **עריכה** | איקון עריכה בדף, או /schedule update |
| **מחיקה** | לשאול "תמחק את משימת הדפלוי" |
| **השהייה/המשכה** | toggle בדף פרטי המשימה |

## דוגמאות שימוש

- סקירת PR יומית: כל בוקר ב-9 סוקרת PRs פתוחים
- ניטור CI: בדיקת כישלונות build וסיכום
- סנכרון דוקומנטציה: עדכון אוטומטי אחרי merge
- אודיט אבטחה שבועי: בדיקת dependencies
- ניטור דפלוי: /loop 5m check if deployment finished

## שווה לדעת

- דורש מנוי Pro, Max, Team או Enterprise
- /loop זמין לכל משתמשי Claude Code (v2.1.72+)
- Cloud tasks: אין גישה לקבצים מקומיים (רק GitHub clone)
- Desktop tasks: דורש מחשב ער + app פתוח
- לבטל תזמון: CLAUDE\_CODE\_DISABLE\_CRON=1

## קרדיטים

מבוסס על התיעוד של [Anthropic Docs](https://docs.anthropic.com)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
