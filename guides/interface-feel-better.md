# Make Interfaces Feel Better: הסקיל שנותן לסוכן טעם של design engineer, פקודת ההתקנה והפרומפט לאפליקציה קיימת

> Make Interfaces Feel Better הוא סקיל (skill) לסוכני קוד, שכתב Jakub Krehel, design engineer שמפרסם ב-X את הפרטים הקטנים שהופכים ממשק לממשק שמרגיש טוב.

מאת קווין ליאקס. פורסם 2026-09-13. נושא: עיצוב ווידאו.
העמוד המלא: https://kevli770.github.io/guides/interface-feel-better

## מה זה Make Interfaces Feel Better

Make Interfaces Feel Better הוא סקיל (skill) לסוכני קוד, שכתב Jakub Krehel, design engineer שמפרסם ב-X את הפרטים הקטנים שהופכים ממשק לממשק שמרגיש טוב. הוא לקח את הרשימה הזאת, רדיוס פינות, יישור אופטי, אזורי לחיצה, אנימציות, צללים, טיפוגרפיה ואייקונים, וארז אותה כקובץ SKILL.md שהסוכן טוען ומיישם.

הסקיל הוא קוד פתוח ב-GitHub, עם יותר מ- 3,400 כוכבים, ולפי המחבר הותקן יותר מ- 30,000 פעמים. הוא עובד עם Claude Code, וגם עם Codex, Cursor ושאר הסוכנים שתומכים בפורמט של agent skills.

**הרעיון במשפט אחד: במקום להגיד לסוכן "תעשה את זה יותר יפה", נותנים לו טעם וכללים.**

![הפוסט של Jakub Krehel ב-X, מקור: x.com/jakubkrehel](https://kevli770.github.io/_astro/01.BRi40DQp.png)

*הפוסט של Jakub Krehel ב-X, מקור: x.com/jakubkrehel*

## למה "תעשה את זה יפה" לא עובד

אתם מכירים את זה. Claude בונה לכם אפליקציה, הכל עובד, אבל משהו ב-UI פשוט מרגיש AI. ואז אתם מבקשים ממנו לייפות, והוא עושה שלושה דברים: מחליף צבעים, מוסיף גרדיאנט, וזורק עוד צל איפשהו. הממשק לא נהיה טוב יותר, הוא נהיה עמוס יותר.

הסיבה היא שהבקשה "יפה" לא אומרת לסוכן מה לבדוק. הוא לא יודע שהרדיוס החיצוני צריך להיות הפנימי פלוס ה-padding, שאייקון ליד טקסט מיושר אופטית ולא מתמטית, או שכפתור צריך אזור לחיצה של 44 פיקסלים. הסקיל נותן לו בדיוק את זה: רשימת עקרונות עם מספרים, ואופן בדיקה קבוע.

**הכלל הכי חשוב בסקיל הוא לא כלל עיצוב אלא כלל בדיקה: בזמן review, להאט את האנימציות לעשר אחוז ולעבור על כל מצב, hover, focus, active, loading, empty. מה שמרגיש לא טוב בעשרה אחוז הוא מה שלא בסדר במהירות מלאה.**

## איך מתקינים

ההתקנה היא פקודה אחת בטרמינל, מתוך תיקיית הפרויקט או מכל מקום:

```bash
npx skills add jakubkrehel/make-interfaces-feel-better
```

הפקודה מורידה את הסקיל מ-GitHub ומתקינה אותו לסוכן שמזוהה במכונה (Claude Code, Codex, Cursor ועוד). אחרי ההתקנה, ב-Claude Code מריצים אותו כפקודת סלאש:

```
/make-interfaces-feel-better
```

מי שמעדיף התקנה ידנית: מעתיקים את התיקייה skills/make-interfaces-feel-better מהריפו אל תיקיית הסקילים של Claude Code:

```
~/.claude/skills/make-interfaces-feel-better/SKILL.md
```

הסקיל טוען את עצמו גם בלי פקודה מפורשת כשמבקשים מהסוכן דברים כמו "feels off", "make it feel better", border radius, optical alignment, stagger animations או hover states.

![הריפו ב-GitHub: github.com/jakubkrehel/make-interfaces-feel-better](https://kevli770.github.io/_astro/02.DnF998Jb.png)

*הריפו ב-GitHub: github.com/jakubkrehel/make-interfaces-feel-better*

## הפרומפט שהייתי מריץ על אפליקציה קיימת

הסקיל תומך בשני מצבי review: quick (עד 5 ממצאים, רק HIGH ו-MEDIUM, על המסך הראשי) ו-full (עד 15 ממצאים, על כל הקטגוריות). על אפליקציה קיימת אני מתחיל מ-full, כי הפעם הראשונה היא הפעם שבה מגלים את הבעיות המערכתיות. זה הפרומפט, שורה אחר שורה:

```
/make-interfaces-feel-better full
Scope: src/components and src/app (onboarding, dashboard, settings).
Framework: Next.js + Tailwind. Express every fix in Tailwind, never add a second styling system.
Walk every state: hover, focus, active, loading, empty.
Replay motion at 10% speed in the Animations panel before judging it.
Report findings grouped by principle: Severity, Location (file:line), Before, After, Why.
Then apply every HIGH and MEDIUM fix, and list what you considered but rejected.
```

מה להחליף לפני שמריצים:

- Scope: הנתיבים של המסכים שהמשתמשים באמת עוברים בהם. לא כל הריפו, אחרת מקבלים 15 ממצאים על מסך שאף אחד לא רואה.
- Framework: מה שיש לכם, Tailwind, CSS רגיל או CSS-in-JS. הסקיל דורש להישאר במערכת העיצוב הקיימת, וזה מונע ממנו להכניס ספרייה חדשה רק בשביל תיקון.
- השורה האחרונה: אם אתם רוצים רק דוח בלי שינויים בקוד, מוחקים אותה. הסקיל אז מחזיר ממצאים בלבד.

לאחר הריצה הראשונה, לפני כל מיזוג של פיצ'ר חדש, מספיק הגרסה הקצרה:

```
/make-interfaces-feel-better quick
Scope: the component I just changed and the screen it lives in.
```

## מה מקבלים בחזרה

הסקיל לא מחזיר "נראה טוב". הוא מחזיר דוח בפורמט קבוע, שקל לעבור עליו ולתעדף:

| חלק בדוח | מה יש בו |
|---|---|
| **Scope and Coverage** | המצב (quick או full), הקבצים והמצבים שנבדקו, ומה לא נבדק ולמה. הסקיל לא מרמז שבדק משהו שלא בדק. |
| **Findings** | טבלה לכל עיקרון שהופר, עם Severity (HIGH, MEDIUM, LOW), Location בפורמט file:line, Before, After ו-Why. בעיה מערכתית מאוחדת לשורה אחת עם כל המיקומים. |
| **Considered but Rejected** | שינויים שהסוכן שקל ולא המליץ עליהם, עם הסיבה. זה החלק שמונע תיקונים שפוגעים בעקביות. |
| **Verification** | הפקודות והאינטראקציות שהורצו בפועל ומה נצפה. מה שלא נבדק מסומן Not verified. |
| **Verdict** | Block אם נשאר ממצא HIGH, Needs changes אם נשארו רק MEDIUM או LOW, Approve רק כשאין ממצאים לביצוע. |

![הסקיל רץ ב-Claude Code ומחזיר טבלאות Before ו-After לכל עיקרון, מתוך הדמו של המחבר](https://kevli770.github.io/_astro/03.DWmXyDzH.jpg)

*הסקיל רץ ב-Claude Code ומחזיר טבלאות Before ו-After לכל עיקרון, מתוך הדמו של המחבר*

## 19 הכללים שהסקיל מלמד את הסוכן

אלה העקרונות מתוך SKILL.md, עם המספרים המדויקים שהסקיל דורש. שימו לב שרובם לא דורשים טעם, רק בדיקה.

| קטגוריה | הכלל | הערך המדויק |
|---|---|---|
| **משטחים** | **רדיוס קונצנטרי** | רדיוס חיצוני = רדיוס פנימי + padding. רדיוסים שווים על הורה וילד הם הדבר הכי נפוץ שגורם לממשק להרגיש לא נכון. |
| **משטחים** | **יישור אופטי** | כשמרכוז גיאומטרי נראה לא נכון, מיישרים בעין. אייקונים ליד טקסט, משולשי play ואייקונים לא סימטריים. |
| **משטחים** | **צללים לעומק, גבולות למבנה** | box-shadow שכבתי ושקוף במקום גבול שנועד רק לעומק. גבולות נשארים למפרידים ולמצבי focus. |
| **משטחים** | **אזור לחיצה מינימלי** | 44 על 44 פיקסלים במובייל, לפחות 40 על 40 בדסקטופ צפוף. מרחיבים עם pseudo-element. אזורי לחיצה לא חופפים. |
| **משטחים** | **קו מתאר לתמונות** | outline של פיקסל אחד בשקיפות 10 אחוז: שחור טהור במצב בהיר, לבן טהור במצב כהה, אף פעם לא אפור מגוון. |
| **אנימציות** | **אנימציות שאפשר לקטוע** | CSS transitions למצבי אינטראקציה, כי אפשר לקטוע אותן באמצע. keyframes רק לרצפים שרצים פעם אחת. |
| **אנימציות** | **כניסה מפוצלת** | כניסה חד פעמית מפוצלת לחלקים סמנטיים עם stagger של כ-100ms. לא לאינטראקציות תכופות. |
| **אנימציות** | **יציאה עדינה** | translateY קטן וקבוע במקום גובה מלא. יציאה רכה יותר מכניסה. ease-out לשתיהן. |
| **אנימציות** | **אייקונים מונפשים** | opacity, scale ו-blur במקום החלפת אייקון. scale מ- 0.25 ל- 1, opacity מ- 0 ל- 1, blur מ-4px ל- 0. |
| **אנימציות** | **scale בלחיצה** | scale(0.96) בלחיצה על כפתור. תמיד 0.96, אף פעם לא מתחת ל- 0.95. |
| **אנימציות** | **בלי אנימציה בטעינת דף** | initial={false} על AnimatePresence, כדי שאנימציות כניסה לא ירוצו ברינדור הראשון. |
| **אנימציות** | **ריסון בתנועה** | בלי אנימציה מותאמת על אינטראקציות תכופות. תנועה היא אף פעם לא הפידבק היחיד, תמיד יש גם צבע, אייקון או טקסט. |
| **טיפוגרפיה** | **החלקת גופנים** | -webkit-font-smoothing: antialiased על ה-root, לטקסט חד יותר ב-macOS. |
| **טיפוגרפיה** | **מספרים ברוחב קבוע** | font-variant-numeric: tabular-nums לכל מספר שמתעדכן, כדי שהפריסה לא תקפוץ. |
| **טיפוגרפיה** | **גלישת טקסט** | text-wrap: balance לכותרות, text-wrap: pretty לגוף הטקסט, כדי למנוע מילה בודדת בשורה אחרונה. |
| **אייקונים** | **עובי קו לפי משקל הטקסט** | 1.5px ליד טקסט רגיל (400), 2px ליד semibold (600). סט אייקונים אחד לכל משטח. |
| **אייקונים** | **SVG אחד לכל המצבים** | currentColor, והמצבים (hover, selected, disabled) מגיעים מ-CSS. outline כברירת מחדל, fill רק למצב פעיל. |
| **ביצועים** | **בלי transition: all** | תמיד מציינים מאפיינים מדויקים, למשל transition-property: scale, opacity. |
| **ביצועים** | **will-change במשורה** | רק ל-transform, opacity ו-filter, ורק כשרואים גמגום בפריים הראשון. אף פעם לא will-change: all. |

![אזור לחיצה של 24 פיקסלים מכל צד סביב הכפתור, מתוך הדמו של המחבר](https://kevli770.github.io/_astro/04.Buu3QdVE.jpg)

*אזור לחיצה של 24 פיקסלים מכל צד סביב הכפתור, מתוך הדמו של המחבר*

## quick מול full

| מצב | מה נבדק | מתי להשתמש |
|---|---|---|
| `quick` | המסלול הראשי והמצבים עם הכי הרבה תנועה. רק ממצאי HIGH ו-MEDIUM, עד 5. | לפני כל מיזוג, על הרכיב שנגעתם בו. |
| `full` | כל הסקופ שביקשתם, בחמש הקטגוריות: טיפוגרפיה, משטחים, אנימציות, אייקונים וביצועים. עד 15 ממצאים. | פעם ראשונה על אפליקציה קיימת, ואחרי שינוי גדול בעיצוב. |

אם לא מציינים מצב, הסקיל מריץ full.

## טיפים

- תריצו את הסקיל על מסך אחד קודם. דוח של 15 ממצאים על כל הריפו מרגיש כמו רשימת מטלות, דוח על מסך אחד מרגיש כמו שיפור.
- תפתחו את הדפדפן ליד. הסקיל מבקש לראות כל מצב בעשרה אחוז מהירות, ואם Claude Code שלכם עובד עם דפדפן, הוא באמת יעשה את זה.
- תשמרו את הכללים שחוזרים. אם הרדיוס הקונצנטרי חוזר בשלושה דוחות, תוסיפו אותו ל-CLAUDE.md של הפרויקט, כדי שהסוכן יבנה נכון מהתחלה ולא רק יתקן.
- אל תדלגו על Considered but Rejected. שם רואים מה הסוכן החליט לא לשנות, וזה החלק שמלמד אתכם על העקביות של המערכת שלכם.
- המספרים הם לא המלצה. 0.96 בלחיצה, 44 פיקסלים אזור לחיצה, 100ms ל-stagger. הסקיל דורש אותם בדיוק, וזה מה שמונע מהסוכן להמציא ערכים בכל פעם.
- אותו מחבר פרסם גם סט סקילים משלימים: better-ui, better-typography ו-better-colors, בפקודה npx skills add jakubkrehel/skills.

## לינקים

- הריפו ב-GitHub: [github.com/jakubkrehel/make-interfaces-feel-better](https://github.com/jakubkrehel/make-interfaces-feel-better)
- המאמר המקורי של המחבר: [jakub.kr/writing/details-that-make-interfaces-feel-better](https://jakub.kr/writing/details-that-make-interfaces-feel-better)
- הפוסט ב-X: [x.com/jakubkrehel/status/2068715109065613564](https://x.com/jakubkrehel/status/2068715109065613564)
- הסקיל בקטלוג: [skills.sh/jakubkrehel/make-interfaces-feel-better](https://skills.sh/jakubkrehel/make-interfaces-feel-better)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
