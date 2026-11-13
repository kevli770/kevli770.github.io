# Caveman: Skill שמוריד 75% מהטוקנים

> Caveman הוא skill (תוסף) ל-Claude Code שמלמד את Claude לדבר כמו איש מערות. בלי מילות כמו "כמובן, אשמח לעזור", מילות קישור או חפירות מיותרות.

מאת קווין ליאקס. פורסם 2026-04-27. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/caveman

## מה זה Caveman

Caveman הוא skill (תוסף) ל-Claude Code שמלמד את Claude לדבר כמו איש מערות. בלי מילות כמו "כמובן, אשמח לעזור", מילות קישור או חפירות מיותרות. רק העצם: קצר, חד.

הסלוגן שלו: `"why use many token when few token do trick"`

### מה התוצאה

- עד 75% פחות טוקנים: הוצאה נמוכה יותר
- פי 3 יותר מהיר בתגובות
- 100% דיוק טכני נשמר: לא מאבדים מידע
- קוד, URLs, נתיבים ופקודות נשארים ללא שינוי

## התקנה ב-Claude Code

פקודה אחת בטרמינל:

```bash
claude plugin marketplace add JuliusBrussee/caveman
claude plugin install caveman@caveman
```

### או חלופית: סניפט אולווייס-און

להוסיף ל-

```
~/.claude/CLAUDE.md
```

את השורות הבאות:

```
Terse like caveman. Technical substance
exact. Only fluff die. Drop: articles,
filler (just/really/basically),
pleasantries, hedging. Fragments OK.
Short synonyms. Code unchanged.
Pattern: [thing] [action] [reason].
[next step]. ACTIVE EVERY RESPONSE.
```

## איך משתמשים

אחרי התקנה, יש כמה דרכים להפעיל:

- פקודת slash: /caveman
- בשפה טבעית: "talk like caveman" או "caveman mode"
- בקשה: "less tokens please"

### לעצור

```
"stop caveman"
"normal mode"
```

## רמות עוצמה

| רמה | פקודה | מה זה עושה |
|---|---|---|
| **Lite** | `/caveman lite` | מוריד מילות מיותרות, שומר על דקדוק |
| **Full** | `/caveman full` | ברירת מחדל. קטעים, בלי תויות יידוע, דחיסה מלאה |
| **Ultra** | `/caveman ultra` | קיצור מקסימלי, קיצורים וסמלים |
| **Wenyan** | `/caveman wenyan` | סינית ספרותית קלאסית: דחיסה מרבית |

## דוגמה: לפני ואחרי

### Claude רגיל (69 tokens)

```
"The reason your React component is re-rendering is likely because you are creating a new object reference on each render cycle. To fix this, wrap the object in useMemo to maintain reference equality between renders."
```

### Caveman Full (19 tokens)

```
"New object ref each render. Inline object prop = new ref = re-render. Wrap in useMemo."
```

### Caveman Ultra

```
"Inline obj prop → new ref → re-render. useMemo."
```

## Skills נוספים בחבילה

| Skill | מה זה עושה |
|---|---|
| `caveman-commit` | commits בסגנון conventional, עד 50 תווים בכותרת |
| `caveman-review` | הערות PR בשורה אחת עם סיבה |
| `caveman-compress` | מכווץ קבצי זיכרון (כמו CLAUDE.md). חוסך \~46% בטוקני קלט |
| `caveman-help` | כרטיס עזרה מהיר עם כל הפקודות |

## נתוני חיסכון

| מדד | חיסכון |
|---|---|
| **טוקני פלט** | **65-75% הפחתה** |
| **טוקני קלט (caveman-compress)** | **כ-46% בממוצע** |
| **מהירות** | **כ-3x יותר מהיר** |
| **דיוק טכני** | **100% (לא מאבדים מידע)** |
| **טווח חיסכון** | 22%-87% (בהתאם למשימה) |

*הערה: הסקיל לא מפחית טוקני חשיבה (thinking) אלא רק טוקני פלט.*

## לינקים

- GitHub Repo: [github.com/JuliusBrussee/caveman](https://github.com/juliusbrussee/caveman)
- רישיון: MIT, חינם לגמרי כמו ממותה בעטיך

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
