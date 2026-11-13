# Higgsfield MCP x Claude: להפוך את קלוד למכונת שיווק עם 31 כלי AI בחיבור אחד

> Higgsfield MCP הוא שרת רשמי של חברת Higgsfield שמאפשר ל-Claude (גרסת Desktop, claude.ai וגם Claude Code) להפעיל ישירות 31 כלי AI מתקדמים ליצירת תמונות, וידאו ותוכן שיווקי.

מאת קווין ליאקס. פורסם 2026-05-23. נושא: עיצוב ווידאו.
העמוד המלא: https://kevli770.github.io/guides/higgsfield-mcp

## מה זה Higgsfield MCP

Higgsfield MCP הוא שרת רשמי של חברת Higgsfield שמאפשר ל-Claude (גרסת Desktop, claude.ai וגם Claude Code) להפעיל ישירות 31 כלי AI מתקדמים ליצירת תמונות, וידאו ותוכן שיווקי. במקום לחבר כל כלי בנפרד דרך API שונה, חיבור יחיד מפעיל את כולם בו זמנית, כולל מודלים חדשים שמתווספים אוטומטית.

**השרת הושק רשמית ב-30 באפריל 2026 ופוסט הבלוג הרשמי פורסם ב-8 במאי 2026. החיבור עובד דרך פרוטוקול MCP (Model Context Protocol) של Anthropic ולוקח פחות משתי דקות להתקין.**

## איזה כלים נכללים בחיבור

Higgsfield חושף דרך ה-MCP את הסט המלא של המודלים שלהם, כולל אינטגרציות עם מודלים חיצוניים מובילים. החלוקה היא:

| קטגוריה | כמות | דוגמאות בולטות |
|---|---|---|
| **יצירת תמונות** | **17 מודלים** | `Nano Banana 2, Flux 2, Seedream V5 Lite, Soul 2, GPT Image 2` |
| **יצירת וידאו** | **14 מודלים** | `Seedance 2.0, Kling 3.0, Veo 3.1 Lite, Sora 2, Cinematic Studio v2` |
| **סטודיו שלם** | יחידות מובנות | Marketing Studio, Cinema Studio, AI Influencer, Canvas |
| **עדכונים** | **אוטומטי** | מודל חדש שיוצא נכנס לחיבור הקיים בלי שינוי הגדרות |

## איך מתקינים: שלוש דרכים

יש שלוש דרכים רשמיות להתחבר. בחרו לפי איך אתם עובדים עם קלוד.

### דרך 1: Claude Desktop ו-claude.ai (הכי קל)

הדרך המומלצת למי שעובד מהדפדפן או מאפליקציית Claude Desktop. מתאים גם ל-Mobile.

![דף ההתקנה הרשמי ב-higgsfield.ai/mcp עם 3 השלבים והכתובת להעתקה](https://kevli770.github.io/_astro/01.xTqfRQ2F.png)

*דף ההתקנה הרשמי ב-higgsfield.ai/mcp עם 3 השלבים והכתובת להעתקה*

**1. נכנסים ל-**[`higgsfield.ai/mcp`](https://higgsfield.ai/mcp) ומעתיקים את כתובת ה-MCP:

```
https://mcp.higgsfield.ai/mcp
```

**2. בקלוד פותחים** `Settings` \> `Connectors`, לוחצים על `Add custom connector`**, נותנים שם** `Higgsfield` ומדביקים את הכתובת.

**3. לוחצים** `Connect`, נרשמים או מתחברים לחשבון Higgsfield, ובוחרים `Always Allow`. זהו, אפשר להתחיל לבקש תמונות וסרטונים מקלוד ישירות.

### דרך 2: Claude Code (וגם Cursor / Codex)

אם אתם עובדים מהטרמינל עם Claude Code (או Cursor, Codex, או כל סוכן אחר תואם MCP), Higgsfield ממליצים על מסלול CLI. הוא מהיר יותר ומשלב את הכלים כ-skills.

![דף ה-CLI ב-higgsfield.ai/cli מציג שלוש פקודות פשוטות](https://kevli770.github.io/_astro/02.CSaXmHxc.png)

*דף ה-CLI ב-higgsfield.ai/cli מציג שלוש פקודות פשוטות*

**שלוש פקודות ובסיימתם:**

```bash
npm install -g @higgsfield/cli
higgsfield auth login
npx skills add higgsfield-ai/skills
```

לאחר הפקודה השנייה, נפתח דפדפן ל-OAuth של חמש שניות. אחרי הפקודה השלישית, הכלים נטענים כ-skills זמינים לסוכן. בשלב הזה אפשר פשוט לכתוב לקלוד:

```
Generate an image with Higgsfield of a coffee cup on a desk
```

והוא בוחר את המודל המתאים, מייצר את התמונה ומחזיר אותה ישירות לצ'אט.

### דרך 3: One-liner ל-Claude Code

מי שמעדיף שורת פקודה יחידה בלי CLI נפרד, אפשר לחבר ישירות דרך פקודת ה-MCP המובנית של Claude Code:

```bash
claude mcp add --transport http --scope user higgsfield https://mcp.higgsfield.ai/mcp
```

פקודה אחת מוסיפה את החיבור לסקופ המשתמש (זמין בכל הפרויקטים). תרצו בכל זאת לבצע `higgsfield auth login` כדי להזדהות.

## איך משתמשים בפועל

אחרי החיבור, ה-MCP חושף את הכלים כפונקציות שקלוד יכול לקרוא להן בעצמו. אין צורך לזכור שמות מודלים, מתארים מה רוצים והוא בוחר. דוגמאות לפניות שעובדות מצוין:

| מה מבקשים מקלוד | מה קלוד עושה ברקע |
|---|---|
| **"תכין לי קמפיין שיווקי שלם להמבורגר Burger Queen"** | מייצר תמונת hero, יוצר 4 וריאציות UGC עם דמויות, מכין mockup של דף נחיתה, מחזיר חבילה מלאה |
| **"תייצר 5 תמונות קרוסלות לאינסטגרם על העסק שלי"** | ניגש ל-Nano Banana, כותב 5 פרומפטים שונים בעצמו, מחזיר את הסדרה עם מיתוג עקבי |
| **"תהפוך את התמונה הזאת לסרטון 5 שניות"** | שולח את התמונה ל-Seedance 2.0 או Kling 3.0 (לפי הסגנון), מחזיר MP4 מוכן |
| **"תכין AI Influencer שעקבי לאורך קמפיין"** | מפעיל את ה-AI Influencer Studio של Higgsfield, שומר על אותו פנים בכל התמונות |
| **"תייצר לוגו ובאנרים עם הסגנון של החברה שלי"** | משתמש ב-Flux 2 או GPT Image 2 לטיפוגרפיה, מחזיר חבילת מיתוג |

## דוגמה מלאה: קמפיין שיווקי בפרומפט אחד

דוגמת ההשקה הרשמית של Higgsfield. פרומפט אחד לקלוד הפיק קמפיין שיווקי שלם להמבורגר בדיוני בשם Burger Queen, כולל:

- תמונת hero קולנועית של ההמבורגר
- וידאו של ההמבורגר עם אפקטים (Hyper Motion)
- ארבע דמויות UGC עם ההמבורגר ביד (#burgerqueen)
- Brand Book שלם של המותג עם פלטת צבעים וטיפוגרפיה
- תפריט מסעדה מעוצב
- Mockup של דף נחיתה לאתר

הפרומפט המקורי שעבד:

```
"Make a full marketing campaign for this burger using Higgsfield"
```

*עם תמונת בורגר מצורפת כקלט. הכול נוצר אוטומטית בתוך שיחת קלוד אחת, ללא מעבר בין כלים, ללא העתקה של פרומפטים, וללא APIs נפרדים.*

## טיפים מהשטח

- בקשו מקלוד "להתייעץ איתי לפני שתייצר", הוא יכתוב פרומפט מוצע ויחכה לאישור. חוסך קרדיטים.
- אם המודל שבחר לא נראה לכם, בקשו במפורש: "תייצר עם Seedream" או "תנסה עם Nano Banana".
- ה-AI Influencer הכי חזק לסדרות עקביות, לקמפיין עם אותו פנים על פני 10+ תמונות.
- לוידאו מקצועי השתמשו ב-Seedance 2.0 או Veo 3.1. לוידאו מהיר וזול יותר: Kling 3.0.
- תמיד אפשר לבקש מקלוד לערוך תמונה קיימת, Higgsfield תומך גם ב-image editing דרך MCP.
- סטטוס משימה ארוכה (וידאו ארוך) נשאר זמין דרך job\_status, קלוד יודע לבדוק בעצמו.
- תקציב יומי: בקשו "תייצר רק אם זה פחות מ-X קרדיטים", קלוד מכבד הגבלות.

## מה חשוב לדעת לפני

Higgsfield MCP חינמי להתקנה, אבל כל יצירה צורכת קרדיטים מהחשבון שלכם בהיגספילד. המודלים היותר חזקים (Veo 3, Sora 2) צורכים יותר.

יש מגבלת קצב על הקריאות, אם מבקשים 20 תמונות בבת אחת, חלק מהן ייכנסו לתור.

התוצרים לא נשמרים אוטומטית במחשב, קלוד מציג אותם בצ'אט. כדאי לבקש `"save the results to ./outputs"` לסיים בקובץ מקומי.

## לינקים רשמיים

- דף ה-MCP הראשי: [`higgsfield.ai/mcp`](https://higgsfield.ai/mcp)
- דף ה-CLI להתקנה ב-Claude Code: [`higgsfield.ai/cli`](https://higgsfield.ai/cli)
- פוסט הבלוג הרשמי על ההשקה: [`higgsfield.ai/blog/Generate-AI-Videos-From-Claude-with-Higgsfield-MCP`](https://higgsfield.ai/blog/Generate-AI-Videos-From-Claude-with-Higgsfield-MCP)
- הודעת ההשקה הרשמית ב-X: [`x.com/higgsfield/status/2049477473843450067`](https://x.com/higgsfield/status/2049477473843450067)
- Anthropic על MCP: [`modelcontextprotocol.io`](https://modelcontextprotocol.io)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
