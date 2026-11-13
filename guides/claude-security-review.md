# Claude Code Security Review: סורק אבטחה חינמי בתוך Claude Code, ובכל PR שאתם פותחים

> ל-Claude Code יש סורק אבטחה מובנה. הוא לא תוסף, לא כלי חיצוני ולא משהו שצריך להתקין, הוא כבר יושב אצלכם בטרמינל מאחורי פקודה אחת

מאת קווין ליאקס. פורסם 2026-08-22. נושא: אבטחה ופרטיות.
העמוד המלא: https://kevli770.github.io/guides/claude-security-review

## מה זה בכלל

ל-Claude Code יש סורק אבטחה מובנה. הוא לא תוסף, לא כלי חיצוני ולא משהו שצריך להתקין, הוא כבר יושב אצלכם בטרמינל מאחורי פקודה אחת:

```
/security-review
```

הוא קורא את השינויים שלכם בקוד, מבין את ההקשר, עוקב אחרי זרימת הנתונים בין קבצים, ומחזיר רשימת ממצאי אבטחה עם הסבר לכל אחד, ואז יכול גם לתקן אותם בשבילכם.

**במקביל, אנטרופיק מתחזקת GitHub Action רשמי וחינמי שעושה בדיוק את אותו דבר, אבל אוטומטית, על כל Pull Request שנפתח ברפו שלכם.**

## למה זה רלוונטי דווקא עכשיו

ב-21 באוגוסט 2026 אנטרופיק שדרגה את מוצר Claude Security למודל Claude Mythos 5, המודל הסייברי החזק שלה. המספרים מהדשבורד הרשמי שלהם: 1,596 פגיעויות שדווחו ב-281 פרויקטי קוד פתוח, מהן 88 קיבלו רשומת CVE או GHSA.

אבל Mythos 5 עצמו נעול. מאז אפריל 2026 הוא בגישה מוגבלת בהזמנה בלבד, אין לו תיבת פרומפטים פתוחה, ומי שכן מקבל גישה מקבל רק פלט מסונן, ממצא ו-patch מתויג. וגם זה, כרגע, ל-Enterprise בלבד.

**הנקודה של המדריך הזה: הכלי שאתם באמת יכולים להשתמש בו כבר נמצא אצלכם, והוא חינם.**

## איך מריצים: הפקודה בטרמינל

פותחים את Claude Code בתיקיית הפרויקט, ומריצים:

```bash
cd my-project
claude
/security-review
```

מה קורה מרגע זה:

- Claude סורק את הקוד ומאתר בעיות אבטחה פוטנציאליות
- כל ממצא מגיע עם הסבר מפורט, מה הבעיה, איפה היא, ולמה היא מסוכנת
- אפשר לבקש ממנו לתקן ישירות, בלי לצאת מהטרמינל

*טיפ: הריצו את זה לפני commit משמעותי, לא אחרי. זה הרגע שבו הכי זול לתקן.*

## למי זה זמין

| מסלול | מה מקבלים |
|---|---|
| **Claude Pro / Max** | ריצות של סקירת אבטחה כלולות במנוי, בלי תשלום נוסף |
| **API Console** | חשבון pay-as-you-go: משלמים לפי צריכה |
| **Claude Enterprise** | בנוסף, מוצר Claude Security המלא שרץ על Mythos 5, בבטא ציבורית |

## לחבר GitHub Action שסורק כל PR

זה החלק שמשנה את המשחק. במקום לזכור להריץ סריקה, היא רצה לבד על כל Pull Request שנפתח.

### שלב 1: להוסיף את מפתח ה-API כסוד ברפו

ב-GitHub נכנסים ל-Settings, ואז Secrets and variables, ואז Actions, ומוסיפים סוד חדש בשם:

```
CLAUDE_API_KEY
```

### שלב 2: ליצור את קובץ ה-workflow

יוצרים בתוך הרפו את הקובץ:

```
.github/workflows/security-review.yml
```

ומדביקים לתוכו את התוכן הבא (זה בדיוק מה שמופיע ב-README הרשמי):

```
name: Security Review

permissions:
  pull-requests: write
  contents: read

on:
  pull_request:

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          ref: ${{ github.event.pull_request.head.sha || github.sha }}
          fetch-depth: 2

      - uses: anthropics/claude-code-security-review@main
        with:
          comment-pr: true
          claude-api-key: ${{ secrets.CLAUDE_API_KEY }}
```

### שלב 3: זהו

מרגע שהקובץ נמצא ב-branch הראשי, כל PR חדש מריץ סריקה, וה-Action משאיר תגובות inline על השורות הבעייתיות בדיוק.

## הגדרות שאפשר לשנות ב-Action

| פרמטר | מה הוא עושה | ברירת מחדל |
|---|---|---|
| `claude-api-key` | מפתח ה-API לסריקה: חובה | **חובה** |
| `comment-pr` | האם להשאיר תגובות עם הממצאים על ה-PR | `true` |
| `exclude-directories` | רשימת תיקיות להוציא מהסריקה, מופרדות בפסיק | - |
| `claude-model` | איזה מודל לסרוק איתו | Opus 4.1 |
| `claudecode-timeout` | תקרת זמן לסריקה בדקות | `20` |
| `run-every-commit` | להריץ על כל commit ולא רק פעם אחת ל-PR | `false` |
| `custom-security-scan-instructions` | נתיב לקובץ עם כללי סריקה משלכם | - |
| `false-positive-filtering-instructions` | נתיב לקובץ עם כללי סינון משלכם | - |

## מה הוא תופס

| קטגוריה | דוגמאות |
|---|---|
| **Injection** | SQL, command, LDAP, XPath, NoSQL, XXE |
| **XSS** | reflected, stored, DOM-based |
| **הרשאות ואימות** | אימות שבור, הסלמת הרשאות, גישה ישירה לאובייקטים |
| **דליפת מידע** | סודות מוטמעים בקוד, מידע רגיש בלוגים, חשיפת מידע |
| **הצפנה** | אלגוריתמים חלשים, ניהול מפתחות לקוי |
| **ולידציה של קלט** | ולידציה חסרה, סניטציה לקויה, buffer overflow |
| **לוגיקה עסקית** | race conditions, בעיות TOCTOU |
| **קונפיגורציה** | ברירות מחדל לא בטוחות, headers חסרים, CORS פרוץ |
| **שרשרת אספקה** | תלויות פגיעות, typosquatting |
| **הרצת קוד** | RCE דרך deserialization, הזרקת pickle או eval |

## מה הוא בכוונה לא מדווח

יש לכלי שכבת סינון שמורידה רעש. הקטגוריות הבאות מסוננות החוצה כדי שלא תטבעו בהתראות שווא:

- מניעת שירות (DoS)
- חוסר rate limiting
- מיצוי זיכרון או מעבד
- ולידציית קלט גנרית בלי השפעה מוכחת
- open redirect

*זו החלטת מוצר מכוונת: עדיף פחות ממצאים אבל אמיתיים.*

## להתאים את הסריקה לצוות שלכם

הפקודה עצמה היא קובץ markdown שאפשר להעתיק ולערוך. מעתיקים את הקובץ מהרפו הרשמי אל תוך הפרויקט שלכם:

```
.claude/commands/security-review.md
```

מרגע שהקובץ קיים אצלכם, Claude Code מריץ את הגרסה שלכם. שם אפשר להגדיר מה נחשב קריטי בקוד שלכם, אילו תבניות להתעלם מהן, ומה רמת הרגישות הרצויה.

## אזהרת אבטחה חשובה

**ה-Action אינו מחוסן מפני prompt injection. אנטרופיק ממליצה במפורש להפעיל אותו רק על PR-ים ממקור אמין.**

אם הרפו שלכם ציבורי ומקבל תרומות מבחוץ, הפעילו בהגדרות ה-Actions את האפשרות Require approval for all external contributors, כדי שה-workflow ירוץ רק אחרי שמתחזק אישר את ה-PR.

## הפרומפט המדויק לסוכן

אם לא מתחשק לכם לערוך קבצי YAML ידנית, תנו לסוכן לעשות את זה. הדביקו לו את זה:

```
Run /security-review on this repo.
Then set up the official Anthropic security-review GitHub Action:
create .github/workflows/security-review.yml using
anthropics/claude-code-security-review@main with comment-pr: true,
wire claude-api-key to the CLAUDE_API_KEY repo secret,
and make it run on every pull_request before merge.
```

ובעברית, אם אתם עובדים ככה:

**תפעיל security review, תחבר את GitHub Actions לרפו שלי, שיבדוק כל PR לפני מיזוג.**

## טיפים

- הריצו את הסריקה לפני commit גדול, לא אחרי, שם היא הכי משתלמת
- הוסיפו את node\_modules, dist ו-vendor ל-exclude-directories כדי לא לשרוף זמן סריקה
- השאירו comment-pr במצב true: הערך האמיתי הוא בתגובות על השורות עצמן
- העתיקו את security-review.md לפרויקט וכתבו בו מה קריטי אצלכם ספציפית
- אל תתייחסו לזה כתחליף ל-pentest או ל-code review אנושי, זו שכבה נוספת
- ברפו ציבורי, חייבים להפעיל אישור ידני להרצות של תורמים חיצוניים
- run-every-commit נשמע מפתה, אבל על רפו עמוס הוא ישרוף לכם קרדיטים, השאירו false

## לינקים

- הרפו הרשמי של ה-Action: [github.com/anthropics/claude-code-security-review](https://github.com/anthropics/claude-code-security-review)
- התיעוד הרשמי של הפקודה: [support.claude.com](https://support.claude.com/en/articles/11932705-automated-security-reviews-in-claude-code)
- מוצר Claude Security: [anthropic.com/product/security](https://www.anthropic.com/product/security)
- דשבורד הפגיעויות של אנטרופיק: [red.anthropic.com/2026/cvd](https://red.anthropic.com/2026/cvd/)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
