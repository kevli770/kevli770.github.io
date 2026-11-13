# Is Agentic: לבדוק בחצי דקה אם סוכני AI בכלל רואים את האתר שלכם, ולתקן את מה שהם לא רואים

> is-agentic הוא כלי חינמי שמשרת מטרה אחת: לבדוק כמה האתר שלכם קריא לסוכני AI. אתם מכניסים דומיין, והוא מחזיר ציון ודוח שמראה בדיוק איפה סוכן נתקע.

מאת קווין ליאקס. פורסם 2026-08-27. נושא: סוכנים.
העמוד המלא: https://kevli770.github.io/guides/is-agentic

## מה זה is-agentic

is-agentic הוא כלי חינמי שמשרת מטרה אחת: לבדוק כמה האתר שלכם קריא לסוכני AI. אתם מכניסים דומיין, והוא מחזיר ציון ודוח שמראה בדיוק איפה סוכן נתקע.

הכלי הושק על ידי Vercel, והסריקה עצמה מורצת על ידי חברה בשם Ora. באתר הרשמי כתוב במפורש "Every scan is run by Ora", כלומר Vercel בנתה את המעטפת, ואורה מריצה את הבדיקות.

**בלי הרשמה, בלי מפתח API, בלי כרטיס אשראי. הכל ציבורי וקריא בלבד.**

## למה סוכן AI לא רואה את האתר שלכם

בן אדם פותח דפדפן. הדפדפן מריץ את ה-JavaScript, ורק אז התוכן מופיע על המסך. סוכן AI לרוב לא מריץ דפדפן מלא, הוא מושך את ה-HTML הגולמי ומנסה לקרוא אותו.

אם כל התוכן של הדף נטען רק אחרי שה-JavaScript רץ, מה שהסוכן מקבל זה עמוד ריק. הוא לא מוצא את המידע, ולא מצליח להשלים פעולה באתר.

זה לא באג שלכם. רוב הכלים שבונים אתרים עם AI מייצרים אפליקציה שנטענת בצד הלקוח, וזו התנהגות ברירת המחדל.

**הדוגמה הכי טובה היא vercel.com עצמם. בדוח שלהם רשום מילה במילה:**

```
The vercel.com domain itself is a JavaScript-heavy Next.js
application that returns minified HTML, making it unreadable
to the agent and forcing it to supplement with external sources.
```

*כלומר גם החברה שבנתה את הכלי נופלת בדיוק באותה נקודה. זו לא בושה: זו נקודת פתיחה.*

## איך מריצים את הבדיקה

### אפשרות 1: טרמינל (הכי מהיר)

פותחים טרמינל וכותבים פקודה אחת. צריך שיהיה מותקן Node.js:

```bash
npx is-agentic yoursite.com
```

אם כבר קיים דוח שמור, הוא חוזר מיד. אם לא: הכלי מתחיל סריקה חדשה ומחכה לתוצאה. לפלט מובנה למכונה:

```bash
npx is-agentic yoursite.com --json
```

### אפשרות 2: האתר

נכנסים ל-is-agentic.com, מדביקים כתובת בשדה ולוחצים Score. הדוח נפתח בעמוד ייעודי שאפשר לשתף.

### אפשרות 3: API

קריאה אחת מחזירה את הדוח האחרון כ-JSON. קריאה בלבד, בלי אימות:

```bash
curl "https://is-agentic.com/api/v1/report?url=https%3A%2F%2Fexample.com"
```

## איך קוראים את הדוח

הציון מחולק לשלושה מאגרים. הכל לפי המתודולוגיה הרשמית של הכלי:

| מאגר | נקודות | מה נמדד |
|---|---|---|
| **Essential** | **80** | היסודות: תוכן שמוגש מהשרת, קודי HTTP נכונים, מבנה מסמך ברור, שגיאות שאפשר להתאושש מהן, פקדים שמישים |
| **Recommended** | **20** | נבדק רק אם קיימת ראיה שהמשטח רלוונטי, API, OAuth, GraphQL, שרת MCP, פורטל מפתחים, מסחר |
| **Bonus** | **עד 5** | פורמטים מתפתחים. אף פעם לא חובה, רק מוסיף נקודות |

בדיקה שלא רלוונטית לאתר שלכם פשוט לא נספרת, היא לא מורידה ניקוד. תוצאה חלקית מקבלת ניקוד יחסי.

### ארבע קטגוריות הגישה הקריטית

החלק החשוב בדוח נקרא Critical access. הוא עונה על שאלה אחת: האם סוכן רגיל מצליח להיכנס, לקרוא ולהפעיל את האתר הציבורי שלכם.

- Agents can reach the site, גישת סורקים והגנות בוט
- Core content is available: התוכן נשאר נגיש בלי מסלול שתלוי בדפדפן
- Navigation fails safely: הפניות ודפים חסרים נותנים לסוכן דרך להתאושש
- Controls are understandable: טפסים ופקדים חושפים שמות ומבנה שמישים

## חמש התקלות הנפוצות והפרומפט המוכן לכל אחת

לכל תקלה בדוח יש כפתור Copy fix שמעתיק פרומפט מוכן. אלה חמש התקלות שהופיעו בדוח של vercel.com, עם התיקון המדויק:

### 1. Agent-friendly 404s

הבעיה: נתיב שלא קיים מחזיר 200 עם מעטפת האפליקציה, אז הסוכן מאמין שכל נתיב קיים.

**התיקון:**

```
Return a real HTTP 404 (or 410) status for nonexistent paths -
never a 200 with your app shell. Give the 404 response a short
markdown body pointing agents at your sitemap, llms.txt, or docs index.
```

בדיקה מהירה: הפקודה הזו חייבת להדפיס 404:

```bash
curl -s -o /dev/null -w "%{http_code}" https://yoursite.com/no-such-path
```

### 2. Content without JavaScript

הבעיה: התוכן מופיע רק אחרי שה-JavaScript רץ, אז הסוכן מקבל עמוד ריק. זו התקלה שהכי הרבה אתרים שנבנו עם AI נופלים בה.

**התיקון:**

```
Server-side render your homepage so AI crawlers see meaningful
content without JavaScript. Ensure an H1 and 500+ chars of text
in raw HTML.
```

### 3. Scoped permissions

הבעיה: ההרשאות של ה-API מתוארות בפרוזה לבני אדם, אבל לא בפורמט שמכונה יכולה לקרוא.

**התיקון:**

```
Declare scoped API permissions where machines can read them:
named OAuth scopes in your OpenAPI security schemes, or
scopes_supported in RFC 9728 protected-resource metadata.
```

### 4. Rate limit response headers

הבעיה: הסוכן לא יודע כמה בקשות מותר לו לשלוח, אז הוא נחסם בלי אזהרה.

**התיקון:**

```
Return standard rate-limit headers on your API responses
(the RFC RateLimit headers, plus Retry-After on a 429) so agents
can self-throttle, and document the conventions alongside your API.
```

### 5. Agent onboarding friction

הבעיה: כדי לקבל גישה צריך למלא טופס "contact sales". סוכן לא ממלא טפסים.

**התיקון:**

```
Offer a free tier or trial, self-serve API key generation,
and a sandbox environment. Agents can't fill out
'contact sales' forms.
```

## איך מתקנים בפועל: צעד אחרי צעד

1\. מריצים את הבדיקה על הדומיין שלכם.

```bash
npx is-agentic yoursite.com
```

2\. פותחים את הדוח באתר ונכנסים לסעיף Fix these gaps first. התקלות מסודרות לפי סדר חשיבות, Critical access קודם.

3\. לוחצים Copy fix על התקלה הראשונה. אפשר גם ללחוץ Prompt to improve כדי לקבל בריף אחד שמאגד את כל ההמלצות.

4\. מדביקים את הפרומפט לסוכן הקוד שלכם, Claude Code, Cursor, Codex, מה שאתם עובדים איתו, יחד עם הקשר קצר על הפרויקט:

```
Here is a finding from an agent-readiness audit of our site.
Apply the fix in this repo, then show me the diff:
<paste the Copy fix text here>
```

5\. מעלים את השינוי לאוויר ולוחצים Rescan בדוח. הציון מתעדכן.

**חשוב: לא לתקן הכל בבת אחת. שתי התקלות הראשונות בקטגוריית Critical access נותנות את רוב הרווח.**

## לחבר את זה לסוכן שלכם, MCP

אפשר לחבר את הכלי ישירות לסוכן דרך שרת MCP ציבורי, בלי אימות:

```
https://is-agentic.com/mcp
```

השרת חושף שלושה כלים:

- is\_agentic\_get\_report: מחזיר דוח מוכן
- is\_agentic\_get\_methodology: מסביר איך הציון מחושב
- is\_agentic\_get\_developer\_docs: מחזיר את תיעוד האינטגרציה

ככה הסוכן יכול לבדוק את האתר, לקרוא את הדוח ולתקן, בלי שתצטרכו להעתיק שום דבר ידנית.

## טיפים

- תריצו קודם על המתחרים שלכם. זה לוקח 30 שניות ונותן לכם תמונה מיידית של איפה אתם עומדים
- הציון הוא סיכום ברמת מבט, החלק השימושי הוא הבדיקות והראיות מתחתיו
- בדיקות של API, OAuth, GraphQL ו-MCP נדלקות רק אם נמצאה ראיה שהמשטח קיים. אם אין לכם API, זה לא פוגע בציון
- אחרי כל תיקון תריצו Rescan. הכלי שומר תוצאה טרייה בקאש לשש שעות, אז אין טעם לרענן כל דקה
- התוצאה יכולה להשתנות משלוש סיבות: שיניתם את האתר, המתודולוגיה השתנתה, או שבדיקה תלוית-זמן החזירה תשובה אחרת
- llms.txt וסייטמאפ הם הדרך הזולה ביותר להוסיף נקודות, הם גם מה שה-404 החדש שלכם אמור להצביע עליו
- אם אתם על Next.js, App Router עם רכיבי שרת פותר את רוב הבעיה של התוכן ללא JavaScript

## מגבלות שכדאי להכיר

- הסריקה בודקת התנהגות ציבורית בנקודת זמן אחת
- בדיקות אוטומטיות יכולות להחזיר תוצאה שגויה לכאן או לכאן, או תוצאה חלקית
- ציון גבוה הוא לא תעודה של אבטחה, נגישות, איכות או תאימות
- הציון לא מעיד על קשר או המלצה מצד הכלי לאתר שנסרק
- הרווח המשוער שמופיע ליד המלצה הוא כיווני, לא הבטחה

## לינקים

- האתר והבדיקה: [is-agentic.com](https://is-agentic.com/)
- תיעוד למפתחים (CLI, API, MCP): [is-agentic.com/docs](https://is-agentic.com/docs)
- המתודולוגיה המלאה של הציון: [is-agentic.com/methodology](https://is-agentic.com/methodology)
- הדוח לדוגמה של vercel.com: [is-agentic.com/scan/vercel.com](https://is-agentic.com/scan/vercel.com)
- Ora: החברה שמריצה את הסריקות: [ora.ai](https://ora.ai)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
