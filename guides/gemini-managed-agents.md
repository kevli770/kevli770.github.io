# Gemini Managed Agents: סוכני AI מנוהלים בקריאה אחת מהקוד, Antigravity ב-Gemini API

> הסוכן נקרא Antigravity, הוא רץ על המודל Gemini 3.5 Flash, ומופעל בתוך סביבת לינוקס מבודדת ומאובטחת שגוגל מארחת (sandbox).

מאת קווין ליאקס. פורסם 2026-07-20. נושא: Gemini וגוגל.
העמוד המלא: https://kevli770.github.io/guides/gemini-managed-agents

## מה זה סוכנים מנוהלים ב-Gemini API

גוגל השיקה סוכנים מנוהלים (Managed Agents) ישירות ב-Gemini API. במקום להקים לבד את כל התשתית שסוכן AI צריך כדי לרוץ, אתם מקבלים סוכן מלא בקריאת API אחת, וגוגל מתחזקת את הכול.

**הסוכן נקרא Antigravity, הוא רץ על המודל Gemini 3.5 Flash, ומופעל בתוך סביבת לינוקס מבודדת ומאובטחת שגוגל מארחת (sandbox). הוא חושב, מריץ קוד, מנהל קבצים, גולש ברשת, הכול בלי שרת משלכם.**

![הסוכן המנוהל של גוגל: Antigravity, מריץ תת-משימות בענן](https://kevli770.github.io/_astro/01.CzeZS_dE.png)

*הסוכן המנוהל של גוגל: Antigravity, מריץ תת-משימות בענן*

## הבעיה שזה פותר

עד היום, כדי לבנות סוכן AI שמריץ קוד ומבצע פעולות אמיתיות, הייתם צריכים להקים לבד את כל התשתית: סביבה מבודדת ומאובטחת, התקנת חבילות, ניהול קבצים ואבטחה. ימים של עבודה, לפני שבכלל התחלתם.

| עד היום: ידני | עכשיו: מנוהל |
|---|---|
| מקימים סביבה מבודדת ומאובטחת | גוגל מארחת ומתחזקת את הסביבה |
| מתקינים חבילות ומנהלים קבצים | הכול מובנה בתוך ה-sandbox |
| דואגים לאבטחה ולבידוד | בידוד ואבטחה כלולים כברירת מחדל |
| **ימים של עבודה** | **קריאת API אחת** |

## הקריאה שמפעילה סוכן

כל מה שצריך זה קריאה אחת. מגדירים איזה סוכן, מה המשימה, ובאיזו סביבה, וזהו:

```python
from google import genai

client = genai.Client()

interaction = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Read Hacker News, summarize the top 10 stories, and save the results as a PDF.",
    environment="remote",
)

print(interaction.output_text)
```

![הקריאה מהתיעוד הרשמי של Gemini API](https://kevli770.github.io/_astro/02.BoqAS_8v.png)

*הקריאה מהתיעוד הרשמי של Gemini API*

## איך מתחילים

1\. מתקינים את ה-SDK של גוגל:

```bash
pip install google-genai
```

2\. מגדירים מפתח API (משיגים אחד ב-Google AI Studio) כמשתנה סביבה:

```bash
export GEMINI_API_KEY="your-key-here"
```

3\. מריצים את הקוד מהחלק הקודם. הסוכן ירוץ בענן, יבצע את המשימה, ויחזיר לכם את התוצאה ב-output\_text.

## מה הסוכן יכול לעשות

| יכולת | מה זה כולל |
|---|---|
| **הרצת קוד** | מריץ פקודות Bash, Python ו-Node.js, מתקין חבילות, מריץ טסטים ובונה אפליקציות |
| **ניהול קבצים** | קורא, כותב, עורך, מחפש ומציג קבצים בתוך ה-sandbox |
| **גישה לרשת** | חיפוש בגוגל ושליפת כתובות URL כדי להביא מידע עדכני |
| **זיכרון ארוך** | דחיסת הקשר אוטומטית (סביב 135k טוקנים) לתמיכה בשיחות ארוכות ורב-שלביות |

## התאמה אישית עם AGENTS.md

בדיוק כמו ב-Claude Code, אפשר להתאים את הסוכן בעזרת קבצי Markdown. מעלים קובץ AGENTS.md עם ההוראות, ו-skills תחת התיקייה .agents/skills/, והסוכן טוען אותם ישירות לתוך ה-sandbox.

**כך אתם מגדירים לסוכן מי הוא, איך לעבוד, ואילו כישורים עומדים לרשותו, בלי לכתוב את זה מחדש בכל קריאה.**

## לחבר את הכלים שלכם דרך MCP

הסוכן יכול להתחבר לכלים החיצוניים שלכם דרך שרתי MCP מרוחקים. מוסיפים למערך ה-tools פריט מסוג mcp\_server עם השם וה-URL של השרת:

```
tools = [{
    "type": "mcp_server",
    "name": "my-tools",
    "url": "https://your-mcp-server.example.com",
    # optional: "headers": {...}, "allowed_tools": [...]
}]
```

כך הסוכן מבצע פעולות אמיתיות מול המערכות שלכם, לא רק מדבר עליהן.

## סביבות והרצה ברקע

לפרמטר environment יש שלוש צורות:

- remote: סביבת sandbox חדשה ונקייה בכל קריאה
- env\_abc123: שימוש חוזר בסביבה קיימת (שומר על מצב בין קריאות)
- אובייקט EnvironmentConfig מלא: להגדרת מקורות וכללי רשת מותאמים

למשימות ארוכות אפשר להריץ ברקע עם background=True, ה-API מחזיר מיד מזהה interaction, ואתם בודקים את הסטטוס (polling) עד ש-completed או failed:

```
interaction = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="...",
    environment="remote",
    background=True,   # requires store=True (default)
)
```

## גוגל לא הראשונה

שווה לדעת: גוגל לא ההמצאה הראשונה בתחום. לאנתרופיק כבר יש את Claude Managed Agents, ו-Antigravity הוא גם ה-IDE שלהם.

**ההבדל שחשוב פה: מדובר בסוכן שקוראים לו ישירות מהקוד, ומתאימים אותו עם AGENTS.md, בדיוק בגישה שכבר הכרתם מ-Claude Code.**

## טיפים

- התחילו עם environment=remote כדי לקבל סביבה נקייה, ועברו לשימוש חוזר בסביבה רק כשצריך לשמור מצב
- שמרו הוראות קבועות ב-AGENTS.md במקום לחזור עליהן בכל קריאה, זה מקצר ומייצב את התוצאות
- למשימות שלוקחות זמן (סקרייפינג, בנייה, מחקר) השתמשו ב-background=True ואל תחסמו את התהליך
- חברו רק את הכלים שאתם באמת צריכים דרך allowed\_tools ב-MCP, פחות הרשאות, יותר בטוח
- יש שני סוכנים מנוהלים: Antigravity (כללי) ו-Deep Research (למחקר מעמיק), בחרו לפי המשימה
- בזכות דחיסת ההקשר האוטומטית אפשר להריץ שיחות ארוכות בלי לדאוג לגבול הטוקנים

## לינקים

- ההכרזה הרשמית: [Introducing Managed Agents in the Gemini API](https://blog.google/innovation-and-ai/technology/developers-tools/managed-agents-gemini-api/)
- התיעוד של Antigravity Agent: [ai.google.dev/gemini-api/docs/antigravity-agent](https://ai.google.dev/gemini-api/docs/antigravity-agent)
- סקירת ה-Agents ב-Gemini API: [ai.google.dev/gemini-api/docs/agents](https://ai.google.dev/gemini-api/docs/agents)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
