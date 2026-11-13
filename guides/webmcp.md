# WebMCP: איך חושפים כלי לסוכן AI על האתר שלכם, צעד אחרי צעד

> WebMCP הוא תקן אינטרנט חדש שמאפשר לאתר שלכם להצהיר על הפעולות שהוא יודע לעשות, כדי שסוכן AI יוכל להפעיל אותן ישירות. במקום שהסוכן יצלם את המסך וינחש איפה ללחוץ, האתר פשוט אומר לו: הנה הכלי לקביעת תור, הנה הכלי לשליחת הצעת מחיר, הנה הכלי לרכישה.

מאת קווין ליאקס. פורסם 2026-08-28. נושא: סוכנים.
העמוד המלא: https://kevli770.github.io/guides/webmcp

## מה זה WebMCP

WebMCP הוא תקן אינטרנט חדש שמאפשר לאתר שלכם להצהיר על הפעולות שהוא יודע לעשות, כדי שסוכן AI יוכל להפעיל אותן ישירות. במקום שהסוכן יצלם את המסך וינחש איפה ללחוץ, האתר פשוט אומר לו: הנה הכלי לקביעת תור, הנה הכלי לשליחת הצעת מחיר, הנה הכלי לרכישה.

**את התקן דוחפים Microsoft ו-Google ביחד, תחת קבוצת העבודה Web Machine Learning של ה-W3C. לא אנתרופיק, ולא OpenAI.**

הבדל חשוב מ-MCP הרגיל של אנתרופיק: שם ספק מריץ שרת שהסוכן מתחבר אליו. כאן אין שרת בכלל. הדף עצמו הוא זה שמצהיר על הכלים, והכל רץ בדפדפן של המשתמש.

## למה דווקא עכשיו

גרג אייזנברג ניסח את זה בשלושה שלבים, וזה הניסוח הכי ברור ששמעתי. כל שלב שואל שאלה אחרת על האתר שלכם:

| השלב | השאלה | מה זה נותן לכם |
|---|---|---|
| **SEO** | גוגל מבינה את העמוד? | אנשים מוצאים אתכם בחיפוש |
| **AEO** | ה-AI מצטט אתכם בתשובה? | אתם מופיעים בתשובה של הצ'אט |
| **WebMCP** | הסוכן מסיים אצלכם את המשימה? | **העסקה נסגרת אצלכם, לא רק ההתעניינות** |

שני השלבים הראשונים עוסקים בלהיות **נמצאים**. השלישי עוסק בלהיות **שמישים**. זה ההבדל בין סוכן שקורא עליכם לבין סוכן שקונה מכם.

## איך מדליקים את זה היום

WebMCP כבר קיים בכרום, אבל עדיין מאחורי דגל. לבדיקה מקומית:

1\. פתחו בכרום את הכתובת הבאה:

```
chrome://flags/#enable-webmcp-testing
```

2\. שנו ל-Enabled והפעילו מחדש את הדפדפן.

כדי שזה יעבוד אצל משתמשים אמיתיים ולא רק אצלכם, צריך להירשם ל-Origin Trial (מכרום 149 ומעלה). הלינק להרשמה נמצא בסוף המדריך.

## הדרך המהירה: טופס שכבר יש לכם

זו הדרך שאני ממליץ להתחיל ממנה. אם יש לכם טופס יצירת קשר או קביעת תור באתר, אתם לא כותבים שורת JavaScript אחת. אתם רק מוסיפים שתי תכונות לתגית הטופס.

### לפני

```
<form action="/submit">
  <input type="text" name="firstName">
  <button type="submit">Submit</button>
</form>
```

### אחרי

```
<form toolname="supportRequestTool"
      tooldescription="Submit a request for support."
      action="/submit">

  <label for="firstName">First Name</label>
  <input type="text" name="firstName">

  <select name="category" required
          toolparamdescription="Determines what team handles this request.">
    <option value="billing">Billing Issue</option>
    <option value="technical">Technical Support</option>
  </select>

  <button type="submit">Submit</button>
</form>
```

זהו. הטופס הזה הוא עכשיו כלי שסוכן יכול לקרוא לו. אלה התכונות:

| התכונה | על מה | מה היא עושה |
|---|---|---|
| `toolname` | `form` | השם של הכלי, כמו שהסוכן רואה אותו |
| `tooldescription` | `form` | מה הכלי עושה. זה מה שהסוכן קורא כדי להחליט אם להשתמש בו, אז כאן שווה להשקיע |
| `toolautosubmit` | `form` | מרשה לסוכן לשלוח את הטופס בלי לחיצה של אדם |
| `toolparamdescription` | כל שדה | מסביר לסוכן מה השדה הזה אומר, בתוך ה-JSON Schema שנוצר אוטומטית |

## הדרך המלאה: כלי בקוד

לכל מה שהוא לא טופס, חיפוש, סינון, שינוי מצב בדף, יש API בקוד. זו הדוגמה הרשמית מהמאגר של ה-W3C:

```
await document.modelContext.registerTool({
  name: "add-todo",
  description: "Add a new item to the user's active todo list",
  inputSchema: {
    type: "object",
    properties: {
      text: { type: "string", description: "The text content" }
    },
    required: ["text"]
  },
  async execute({ text }) {
    await addTodoItemToCollection(text);
    return {
      content: [{
        type: "text",
        text: `Added todo item: "${text}" successfully.`
      }]
    };
  }
});
```

**שימו לב לטעות שחוזרת בהמון כתבות:**

זה `document.modelContext` ולא `navigator.modelContext`. קוד שתעתיקו מכתבות עם navigator פשוט לא ירוץ.

## מגבלות ואבטחה

החלק הזה חשוב, כי הוא מה שמפריד בין מי שמבין את התקן למי שרק שמע עליו:

- WebMCP עובד רק במסמכים מבודדי-מקור (origin-isolated documents).
- שני ה-API חסומים מאחורי Permissions Policy בשם tools, שברירת המחדל שלו היא self. אייפריים ממקור אחר צריך allow="tools" מפורש.
- התקן תוכנן לזרימות בדפדפן עם אדם בתוך הלולאה. הוא לא נועד לסוכנים שרצים headless בשרת.
- גילוי הכלים הוא הבעיה הפתוחה: אין רישום מרכזי. הסוכן צריך להגיע לאתר שלכם כדי לגלות שיש בו כלים.

*הנקודה האחרונה היא גם ההזדמנות. מי שיצהיר על כלים מוקדם יהיה שם כשהסוכנים יתחילו להגיע.*

## טיפים

- התחילו מהטופס הכי רווחי שיש לכם. קביעת תור או בקשת הצעת מחיר, לא טופס יצירת קשר כללי.
- התיאור הוא המוצר. tooldescription הוא מה שגורם לסוכן לבחור בכם, אז כתבו אותו כמו שאתם כותבים כותרת מודעה.
- שם כלי אחד, פעולה אחת. עדיף שלושה כלים ברורים מכלי אחד עם חמישה מצבים.
- אל תדליקו toolautosubmit על משהו שעולה כסף לפני שבדקתם את הזרימה עד הסוף.
- תארו כל שדה עם toolparamdescription. שדה בלי תיאור הוא שדה שהסוכן ינחש.
- בדקו בעצמכם: פתחו את הדף עם הדגל דלוק ובקשו מסוכן לבצע את הפעולה. אם הוא נתקע, התיאור אשם.
- אל תזרקו את ה-SEO. WebMCP מוסיף שכבה, הוא לא מחליף את שתי הקודמות.

## לינקים

- המאגר הרשמי של התקן ב-W3C: [github.com/webmachinelearning/webmcp](https://github.com/webmachinelearning/webmcp)
- טיוטת התקן: [webmachinelearning.github.io/webmcp](https://webmachinelearning.github.io/webmcp/)
- התיעוד של Chrome: [developer.chrome.com/docs/ai/webmcp](https://developer.chrome.com/docs/ai/webmcp)
- ה-API של הטפסים: [developer.chrome.com/docs/ai/webmcp/declarative-api](https://developer.chrome.com/docs/ai/webmcp/declarative-api)
- ה-API בקוד: [developer.chrome.com/docs/ai/webmcp/imperative-api](https://developer.chrome.com/docs/ai/webmcp/imperative-api)
- ההכרזה של Chrome: [developer.chrome.com/blog/webmcp-epp](https://developer.chrome.com/blog/webmcp-epp)
- הרשמה ל-Origin Trial: [developer.chrome.com/origintrials](https://developer.chrome.com/origintrials/#/register_trial/4163014905550602241)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
