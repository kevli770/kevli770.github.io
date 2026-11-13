# Humanizer: לנקות את ה-AI מהכתיבה שלך

> מבוסס על github.com/blader/humanizer Skill ל-Claude Code שלוקח טקסט שנכתב על ידי AI ומנקה ממנו את ה"ריח" של AI.

מאת קווין ליאקס. פורסם 2026-04-03. נושא: פרודוקטיביות.
העמוד המלא: https://kevli770.github.io/guides/humanizer

מבוסס על [github.com/blader/humanizer](https://github.com/blader/humanizer)

## מה זה

Skill ל-Claude Code שלוקח טקסט שנכתב על ידי AI ומנקה ממנו את ה"ריח" של AI. יודעים את הסגנון הזה?

`"delve", "leverage"`, em dash בכל פינה, רשימות מסודרות מדי, סיכום שמתחיל ב-`"In conclusion"`?

Humanizer מזהה 29 דפוסים כאלה ומתקן אותם. מבוסס על מדריך ויקיפדיה "Signs of AI writing" שנכתב ע"י WikiProject AI Cleanup.

## למה בכלל

ChatGPT ו-Claude כותבים בצורה שמי שקורא הרבה טקסט מזהה תוך שנייה. לא בגלל שהתוכן רע, אלא בגלל שיש דפוסים חוזרים שצורמים.

Humanizer עושה שני מעברים על הטקסט: פעם ראשונה כותב מחדש, פעם שנייה בודק "מה עדיין נשמע רובוטי?" ומתקן שוב.

## 29 דפוסים ב-5 קטגוריות

![29 דפוסים ב-5 קטגוריות](https://kevli770.github.io/_astro/01.CcjIsKYC.jpg)

| קטגוריה | # | דוגמאות |
|---|---|---|
| **Content** | 6 | ניפוח חשיבות, name dropping, ניתוחים שטחיים, שפה שיווקית |
| **Language** | 7 | מילון AI טיפוסי, הימנעות מ-"is/are", כלל שלוש, מחזור מילים נרדפות |
| **Style** | 8 | em dash בכל מקום, bold מוגזם, רשימות עם כותרות מודגשות, title case |
| **Comm.** | 3 | שאריות chatbot, הסתייגויות cutoff, טון חנפני |
| **Filler** | 5 | ביטויי מילוי, הסתייגות יתר, סיכומים גנריים |

## התקנה

שתי שורות בטרמינל:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/blader/humanizer.git ~/.claude/skills/humanizer
```

בלי dependencies, בלי build, בלי npm install. רק הקובץ SKILL.md עושה את העבודה.

## שימוש

בתוך Claude Code:

```
/humanizer
[הטקסט שלכם כאן]
```

או פשוט לכתוב: `"Please humanize this text:"` ולהדביק.

## התאמה לסגנון שלך (voice calibration)

אם תדביקו 2-3 פסקאות מהכתיבה שלכם לפני הטקסט, Humanizer ינתח את הסגנון וינסה להתאים את התוצאה. משפטים קצרים? ארוכים? סלנג? אקדמי? הוא ילמד מהדוגמה.

```
/humanizer
Here's a sample of my writing for voice matching:
[פסקאות שלכם]

Now humanize this text:
[הטקסט לתיקון]
```

## לפני / אחרי

**לפני:**

```
"In the rapidly evolving landscape of artificial intelligence, it is crucial to leverage cutting-edge tools that can fundamentally transform how we approach content creation."
```

**אחרי:**

```
"AI tools keep getting better at writing. Here's one that cleans up the robot voice so your text sounds like you actually wrote it."
```

## שווה לדעת

- voice sample נותן תוצאות הרבה יותר עקביות. שווה את ההשקעה
- הדפוסים מבוססים על ויקיפדיה אנגלית, אז על אנגלית זה הכי חזק
- עובד גם על פסקה בודדת, לא חייבים טקסט שלם
- קוד פתוח, MIT

## קרדיטים

Skill נוצר על ידי [@blader](https://github.com/blader)

מבוסס על [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)

רישיון: MIT

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
