# Claude Design: מדריך מפורט, מרעיון לעיצוב מקצועי

> Claude Design הוא מוצר חדש של Anthropic Labs שהושק ב-17 באפריל 2026. הוא מאפשר ליצור עיצובים מקצועיים דרך שיחה עם Claude.

מאת קווין ליאקס. פורסם 2026-04-20. נושא: עיצוב ווידאו.
העמוד המלא: https://kevli770.github.io/guides/claude-design

## מה זה Claude Design

Claude Design הוא מוצר חדש של Anthropic Labs שהושק ב-17 באפריל 2026. הוא מאפשר ליצור עיצובים מקצועיים דרך שיחה עם Claude. מונע על ידי Claude Opus 4.7.

**התוצאה היא לא תמונה סטטית, זה HTML חי, לחיץ, ניתן לבדיקה ולאינטראקציה.**

זמין: `claude.ai/design`

מנויים: Pro, Max, Team, Enterprise (research preview)

מניה נפרדת ל-Design: לא נספר מהשימוש הרגיל שלכם

## איפה מוצאים את זה

בתפריט הראשי של Claude (בסיידבאר משמאל), מופיע עכשיו לשונית Design מתחת ל-Code:

![הסיידבאר של Claude: לחיצה על Design](https://kevli770.github.io/_astro/01.BmDvj9KE.png)

*הסיידבאר של Claude: לחיצה על Design*

## סוגי תוצרים

כשנכנסים ל-Claude Design רואים 4 טאבים בצד שמאל למעלה:

![מסך בחירת סוג תוצר: Prototype, Slide Deck, From Template, Other](https://kevli770.github.io/_astro/02.B0Tgrp-Z.png)

*מסך בחירת סוג תוצר: Prototype, Slide Deck, From Template, Other*

### Prototype: פרוטוטייפים אינטראקטיביים

יוצר דפים אינטראקטיביים שאפשר ללחוץ עליהם ולבדוק. לא סתם תמונה: HTML חי.

- לנדינג פייג'ים מלאים
- דשבורדים ומסכי אדמין
- אפליקציות מובייל (onboarding, flow)
- wireframes ומוקאפים
- טפסים עם conditional logic
- אלמנטים מתקדמים: 3D, voice, video, shaders, צ'אטבוטים מובנים

### Slide Deck: מצגות

מצגות מקצועיות עם עיצוב, גרפים ונקודות דיבור.

- pitch decks למשקיעים
- מצגות הדרכה וסדנאות
- סליידים מונפשים
- אפשרות speaker notes (פחות טקסט בשקפים)
- יצוא ל-PPTX לעריכה נוספת

### From Template: מתבניות

גלריית תבניות מוכנות שאפשר להתחיל מהן ולהתאים. בטאב Examples בצד ימין יש 25+ דוגמאות.

### Other: אחר

כל דבר אחר: סרטוני אנימציה, one-pagers, עמודי שיווק, באנרים, פוסטרים, דפי מידע, אינפוגרפיקות.

## מסך היצירה

אחרי שבוחרים סוג תוצר, נפתח מסך היצירה:

![מסך היצירה: צ'אט משמאל, קנבס מימין](https://kevli770.github.io/_astro/03.BQjdmwLo.png)

*מסך היצירה: צ'אט משמאל, קנבס מימין*

הממשק מפוצל לשניים:

- צד שמאל: צ'אט + הערות (Chat / Comments)
- צד ימין: קנבס, שם התוצרים מופיעים

### Start with context: אפשרויות קלט

בצד שמאל של מסך היצירה יש 4 אפשרויות להוסיף הקשר:

| אפשרות | מה זה עושה |
|---|---|
| **Design System** | מגדיר את המותג שלכם: צבעים, פונטים, רכיבים. כל תוצר חדש ישתמש בזה אוטומטית |
| **Add screenshot** | מעלים צילומי מסך, wireframes, סקיצים, או מוצרים של מתחרים כהשראה |
| **Attach codebase** | מקשרים repo מ-GitHub: Claude קורא את הרכיבים והסגנונות הקיימים |
| **Drag in a Figma file** | גוררים קובץ .fig: Claude מנתח את העיצוב מקומית (בלי העלאה לשרת) |

בתחתית המסך יש גם אזור לגרירה של קבצים:

```
Images, docs, references, Figma links, or folders - Claude will use them as context
```

ולמטה יש אפשרות **Import** + כפתורי מדיה (מצלמה, מיקרופון, הקלטת מסך).

## Design System: המותג שלכם

השלב הראשון והכי חשוב. בתחתית מסך הבחירה (product types) יש כפתור **Set up design system**. לוחצים עליו ומעלים:

- GitHub repo: Claude קורא את הרכיבים והסגנונות
- קבצי Figma (.fig)
- לוגואים, פונטים, פלטת צבעים
- מסמכים (PPTX, PDF) שמשקפים את הסגנון

Claude מנתח אוטומטית:

- פלטת צבעים (primary, secondary, accent)
- טיפוגרפיה (פונטים, גדלים, משקלים)
- רכיבים (buttons, cards, navigation)
- דפוסי לייאוט (spacing, grid)

*אחרי Published: כל פרויקט חדש ישתמש בזה אוטומטית. אפשר לנהל מספר design systems לכל ארגון.*

## שיפור ודיוק

| דרך | מתי להשתמש | דוגמה |
|---|---|---|
| **צ'אט** | שינויים רחבים: צבע, לייאוט, סקציות | "תחליף את הצבע לכחול" |
| **Inline Comments** | תיקונים נקודתיים על רכיב ספציפי | לחיצה על כפתור והערה |
| **Sliders** | spacing, צבע, לייאוט: בזמן אמת | Claude יוצר סליידרים מותאמים |
| **עריכה ישירה** | טקסט ישירות על הקנבס | לחיצה על טקסט ותקלידו |

## יצוא

| פורמט | למה טוב |
|---|---|
| `PDF` | לשליחה, הדפסה, ארכיון |
| `PPTX` | עריכה נוספת ב-PowerPoint |
| `HTML` | דף עצמאי, לחיץ וניתן לבדיקה |
| `Canva` | לעריכה משותפת ושיתוף צוותי |
| `Claude Code` | handoff bundle: המרה לקוד פרודקשן בפקודה אחת |
| `ZIP` | הורדה של הכל |
| `URL` | שיתוף דרך לינק פנימי בארגון |

## שיתוף פעולה

- שיתוף דרך לינק: view only, comment, edit
- שיחות קבוצתיות עם צוות
- עריכה בזמן אמת עם מספר אנשים
- Enterprise: אדמין צריך להפעיל ב-Organization settings

## למי זה מתאים

| תפקיד | איך זה עוזר |
|---|---|
| **יזמים** | pitch decks בלי לחכות למעצב או לעיכוב |
| **מנהלי מוצר** | wireframes ו-flows לשיתוף לפני ריויו עיצוב |
| **משווקים** | לנדינג פייג'ים, עמודי שיווק, באנרים, בלי מעצב |
| **מעצבים** | סיור מהיר של כיוונים רבים |
| **מפתחים** | פרוטוטייפ ויזואלי, אחר כך handoff ל-Claude Code |

## טיפים

- להתחיל עם לייאוט בסיסי ואחר כך להוסיף פרטים
- לתת פידבק ספציפי ומדיד
- לבקש מ-Claude לשמור גרסה לפני ניסיון חדש
- לתכנן responsive מוקדם ככל האפשר
- לבקש ווריאציות להשוואה
- הערות ישירות לפעמים נעלמות לפני עיבוד, להדביק בצ'אט
- codebase גדול? לקשר תיקיות ספציפיות בלבד

## מגבלות

- לא מחליף Figma לתהליכי עיצוב מקצועיים בסקייל
- תכונות מתקדמות (3D, voice) עדיין מוקדמות
- איכות ה-prompt משפיעה הרבה על התוצאה
- Research preview: אין GA עדיין
- לא ניתן לייצא סרטונים כקובץ (צריך הקלטת מסך)

## לינקים

- גישה: [claude.ai/design](https://claude.ai/design)
- הכרזה: [Anthropic Labs Announcement](https://www.anthropic.com/news/claude-design-anthropic-labs)
- מדריך התחלה: [Claude Help Center](https://support.claude.com/en/articles/14604416-get-started-with-claude-design)
- Design System: [Claude Help Center](https://support.claude.com/en/articles/14604397-set-up-your-design-system-in-claude-design)
- טיוטוריאל ביוטיוב: [Full Walkthrough (YouTube)](https://www.youtube.com/watch?v=o7HSVPHCX8I)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
