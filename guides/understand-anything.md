# Understand Anything: להבין כל codebase, גרף ידע בפקודה אחת

> Understand Anything הוא כלי חינמי בקוד פתוח שהופך כל codebase לגרף ידע אינטראקטיבי שאפשר לחקור, לחפש ולשאול עליו שאלות. במקום לקרוא אלפי שורות קוד לא מוכר, אתם מקבלים מפה חיה שמראה איך הכל מחובר.

מאת קווין ליאקס. פורסם 2026-07-29. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/understand-anything

## מה זה Understand Anything?

Understand Anything הוא כלי חינמי בקוד פתוח שהופך כל codebase לגרף ידע אינטראקטיבי שאפשר לחקור, לחפש ולשאול עליו שאלות. במקום לקרוא אלפי שורות קוד לא מוכר, אתם מקבלים מפה חיה שמראה איך הכל מחובר.

הוא רץ ישירות בתוך כלי ה-AI שאתם כבר עובדים איתם, קלוד קוד, Cursor, Codex, Copilot, Gemini CLI ועוד. בפקודה אחת הוא סורק את הפרויקט ובונה גרף של כל הקבצים, הפונקציות, המחלקות והתלויות ביניהם.

**יותר מ-76 אלף כוכבים בגיטהאב, אחד הכלים החמים ביותר להבנת קוד לא מוכר.**

## הבעיה שהוא פותר

קוד שקיבלתם ממישהו אחר בלי תיעוד יכול להיות סיוט. אתם מפחדים לגעת כי אתם לא יודעים מה מחובר למה, איפה נשברת הזרימה, ומה יישבר אם תשנו קובץ מסוים.

במקום לבזבז ימים על קריאת קוד של מישהו אחר, Understand Anything מוציא את הידע החוצה בפקודה אחת ומראה לכם את התמונה המלאה, מי מדבר עם מי, ומה התלוי במה.

## איך זה עובד?

הכלי משלב שתי גישות משלימות שנותנות גם דיוק וגם הבנה:

- Tree-sitter: ניתוח דטרמיניסטי של מבנה הקוד. מחלץ עובדות מדויקות (פונקציות, מחלקות, imports, קריאות) בלי לנחש ובלי לצרוך טוקנים.
- LLM: ניתוח סמנטי. מייצר תקצירים בשפה פשוטה והסברים על הארכיטקטורה והכוונה מאחורי הקוד.

מאחורי הקלעים רצים חמישה סוכנים: project-scanner, file-analyzer, architecture-analyzer, tour-builder ו-graph-reviewer. התוצר הסופי נשמר בקובץ `.ua/knowledge-graph.json` בתוך הפרויקט.

## איך מתקינים

בתוך קלוד קוד, שתי פקודות בלבד:

```
/plugin marketplace add Egonex-AI/Understand-Anything
/plugin install understand-anything
```

עובד גם ב-Cursor, VS Code עם Copilot, Codex, OpenCode, Gemini CLI ועוד למעלה מ-10 פלטפורמות דרך installer של שורה אחת.

## הפקודות העיקריות

| פקודה | מה היא עושה |
|---|---|
| `/understand` | סריקה ראשונית ובניית גרף הידע של הפרויקט |
| `/understand-dashboard` | פתיחת הדשבורד האינטראקטיבי לצפייה בגרף |
| `/understand-chat` | שאילת שאלות חופשיות על ה-codebase |
| `/understand-diff` | ניתוח ההשפעה של שינוי: מה עלול להישבר |
| `/understand-explain` | העמקה בקובץ או בפונקציה ספציפית |
| `/understand-onboard` | יצירת מדריך כניסה (onboarding) לפרויקט |
| `/understand-domain` | חילוץ הידע העסקי שמאחורי הקוד |
| `/understand --auto-update` | עדכון אוטומטי של הגרף אחרי כל commit |

## הדשבורד האינטראקטיבי

אחרי הסריקה נפתח דשבורד ויזואלי שמראה את גרף הידע של הפרויקט:

- גרף אינטראקטיבי: צמתים לחיצים לכל קובץ, פונקציה ומחלקה, צבועים לפי שכבה ארכיטקטונית
- חיפוש חכם: Fuzzy ו-Semantic, כדי למצוא בדיוק את מה שאתם מחפשים
- פרטי צומת: הקוד, הקשרים, והסבר בשפה פשוטה על כל רכיב
- Guided Tours: סיור מודרך שמסביר את ה-codebase שלב אחרי שלב
- Diff Impact: מראה מה יישבר אם תיגעו בקובץ מסוים

אפשר לפתוח את הדשבורד גם בלי קלוד קוד (דורש Node.js גרסה 18 ומעלה):

```bash
npx https://github.com/Egonex-AI/Understand-Anything/releases/latest/download/understand-anything-viewer.tgz /path/to/project
```

## טיפים

- התחילו עם /understand על כל הפרויקט, ואז /understand-dashboard כדי לראות את התמונה הגדולה
- לפני שאתם משנים קובץ קריטי, הריצו /understand-diff כדי לראות מה עלול להישבר
- חדשים בפרויקט? /understand-onboard מייצר לכם מדריך כניסה מסודר
- בדשבורד, השתמשו ב-Semantic Search כשאתם לא זוכרים את שם הקובץ אבל יודעים מה הוא עושה
- הפעילו /understand --auto-update כדי שהגרף יתעדכן אוטומטית אחרי כל commit
- אפשר למקד את הסריקה לתת-תיקייה בלבד: /understand \[שם התיקייה\]

## מגבלות

- הסריקה הראשונית על פרויקט גדול יכולה לקחת כמה דקות ולצרוך טוקנים של ה-LLM
- הניתוח הסמנטי טוב כמו המודל שמאחוריו, שווה לעבור על התקצירים ולא לקבל אותם כמובנים מאליהם

## לינקים

- האתר הראשי: [understand-anything.com](https://understand-anything.com)
- דמו אינטראקטיבי: [understand-anything.com/demo](https://understand-anything.com/demo/)
- הקוד בגיטהאב: [github.com/Egonex-AI/Understand-Anything](https://github.com/Egonex-AI/Understand-Anything)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
