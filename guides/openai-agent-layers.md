# OpenAI Agent Layers: מה מקבלים בכל שכבה של OpenAI, ואיפה Grok Bot נכנס לתמונה

> הטבלה הרשמית של OpenAI מסכמת את זה בשלוש שורות. הגדרה מינימלית: Agents API.

מאת קווין ליאקס. פורסם 2026-09-13. נושא: סוכנים.
העמוד המלא: https://kevli770.github.io/guides/openai-agent-layers

## מה זה Agents API

ב-10 בספטמבר 2026 OpenAI פתחה לציבור (Public Beta) את Agents API: גישה דרך API לאותה "רתמת סוכן" (harness) שמריצה את Codex ואת ChatGPT Work. במקום לבנות לבד לולאת סוכן, ניהול זיכרון, הרצת פקודות ותיאום בין סוכנים, אתם שולחים משימה, ו-OpenAI מנהלת את הסשן, את הסביבה ואת ההתאוששות מתקלות.

**המשפט שמסביר הכל: תחשבו שקיבלתם עובד מבריק, אבל בלי מחשב, בלי קבצים, בלי הרשאות ובלי דרך לזכור מחר מה הוא עשה היום. זה בדיוק מה שמפיל הרבה מוצרי AI אחרי הדמו. Agents API נותן לעובד הזה את המחשב (Environment), את הזיכרון (Session) ואת הידיים (Tools ו-MCP).**

![מה זה Agents API](https://kevli770.github.io/_astro/01.DsAKj13s.jpg)

*ההכרזה הרשמית:* Build and run cloud agents with the Codex harness, fully managed by OpenAI

## השכבות של OpenAI, ומה מקבלים בכל אחת

OpenAI היום מציעה חמש דרכים לעבוד עם סוכנים, וכל אחת יושבת שכבה מעל הקודמת. ככל שעולים, פחות דברים בידיים שלכם ופחות שליטה. ככל שיורדים, יותר עבודה ויותר שליטה.

| השכבה | מה מקבלים | מה עליכם לנהל | למי זה מתאים |
|---|---|---|---|
| **Responses API** | קריאה ישירה למודל. אתם בונים את הסוכן מאפס. | הכל: לולאת הסוכן, זיכרון, כלים, שרתים. | מי שצריך שליטה מקסימלית. |
| **Agents SDK** | ספריית קוד פתוח שמריצה את לולאת הסוכן וההעברות (handoffs) בתוך האפליקציה שלכם. | הפריסה, האחסון, האישורים, זמן הריצה. | צוותים שרוצים כלים מותאמים ופריסה משלהם. |
| **Agents API** | רתמת Codex מנוהלת: סשנים ארוכים, דחיסת הקשר אוטומטית, תתי-סוכנים, סנדבוקס. | רק הכלים ומה הסוכן אמור להשיג. | משימות ארוכות בלי להקים תשתית. |
| **Codex** | סוכן הקוד של OpenAI: CLI במחשב שלכם או ענן. הרתמה שלו פתוחה ב-GitHub. | הריפו, ההנחיות, האישור של כל שינוי. | מפתחים שעובדים על קוד. |
| **ChatGPT Work** | סוכן מוכן בתוך ChatGPT, מבוסס Codex, שפועל על האפליקציות והקבצים שלכם. | כמעט כלום. מגדירים ומאשרים. | מי שרוצה עובד מוכן, לא לבנות מוצר. |

הטבלה הרשמית של OpenAI מסכמת את זה בשלוש שורות. הגדרה מינימלית: Agents API. שליטה מותאמת: Agents SDK. גמישות מקסימלית: Responses API.

## ארבעת המושגים של Agents API

ה-API כולו בנוי סביב ארבעה מושגים. אם מבינים אותם, מבינים את כל התיעוד.

| מושג | מה זה | במילים של הסרטון |
|---|---|---|
| **Agent** | המודל, ההנחיות, הכלים ושרתי ה-MCP שזמינים לו. | העובד המבריק. |
| **Environment** | סנדבוקס או מחשב שבו הסוכן ניגש לקבצים, טוען skills ומריץ פקודות. | המחשב, הקבצים וההרשאות. |
| **Session** | מופע עמיד (durable) של סוכן שעובד על משימות ומגיב לקלט. נשמר בין תורות ובין שעות. | הזיכרון: מה הוא עשה היום. |
| **Events and items** | הקלט שאתם שולחים והפלט שהסוכן מייצר בזמן הסשן, בסטרימינג. | השיחה עם העובד. |

## איך מתחילים

### שלב 1: מפתח API

יוצרים מפתח API בפלטפורמה של OpenAI עם ההרשאות api.agents.read, api.agents.write ו-api.responses.write, ושומרים אותו במשתנה הסביבה:

```bash
export OPENAI_API_KEY=sk-...
```

### שלב 2: התקנה

מעדכנים את ה-SDK הרשמי:

```bash
pip install --upgrade openai
```

או ב-JavaScript:

```bash
npm install openai
```

### שלב 3: סשן ראשון

הדוגמה הרשמית: יוצרים סשן עם סביבה מנוהלת של OpenAI, שולחים משימה, ומקבלים את האירועים בסטרימינג.

```python
from openai import OpenAI

with OpenAI() as client:
    with client.beta.agents.sessions.create(
        agent={
            "model": "gpt-6-astra",
            "instructions": "Write clean code, run it, and report the actual output.",
        },
        environment={"type": "openai_hosted"},
        input="Create tree.py that prints a tree of the current directory. Run it.",
        stream=True,
    ) as events:
        for event in events:
            print(event.to_json(indent=None), flush=True)
```

### שלב 4: מעקב והמשך

האירוע agent.session.turn.completed אומר שהתור הסתיים. שומרים את ה-session\_id כדי להמשיך את אותה עבודה מחר, או מוחקים את הסשן כשסיימתם.

## דוגמה מעשית: אתר עם תקלה

זה התרחיש מהסרטון. יש לכם אתר ויש בו תקלה. במקום סוכן אחד שמנסה הכל ברצף, הסוכן הראשי מפצל את העבודה: תת-סוכן אחד בודק את הלוגים, אחר את הגרסה שעלתה, ואחר את החבילות שהתעדכנו. כל תת-סוכן מחזיק הקשר משלו, והראשי מאחד את הממצאים.

- מגדירים כמה תתי-סוכנים מותר במקביל, למשל max\_concurrent\_subagents: 4, כדי לשלוט בעלות.
- מחברים את הכלים שהסוכנים צריכים: שרת MCP של הלוגים, גישה לריפו, חיפוש באינטרנט.
- העבודה נשמרת בסשן גם אם היא נמשכת שעות. כשההקשר מתמלא, OpenAI דוחסת אותו אוטומטית (compaction) ואתם לא כותבים לוגיקה של סיכום.
- אפשר להתערב באמצע (steering): לשלוח הודעה לסשן פעיל ולשנות כיוון בלי להתחיל מחדש.

## איפה הסוכן רץ: סביבות ההרצה

ה-Environment הוא המחשב של העובד. יש שלוש אפשרויות, ואפשר גם בלי סביבה בכלל אם הסוכן רק מדבר עם כלים.

| סביבה | מה זה אומר |
|---|---|
| **OpenAI-hosted** | OpenAI מקימה ומנהלת את הסנדבוקס, על אותה תשתית של Codex ו-ChatGPT. אפשר להגדיר חבילות, גישה לרשת והורדת תוצרים (artifacts). |
| **Self-hosted** | מריצים codex exec-server בסביבה שלכם. הקוד והקבצים לא יוצאים מהשרת שלכם, והסוכן המנוהל שולח לשם פקודות. |
| **שותפים** | סנדבוקס אצל ספק חיצוני: Blaxel, Cloudflare, Daytona, DigitalOcean, E2B, Modal, Oracle, Runloop, Vercel. |

## OpenAI מול Anthropic מול Grok Bot

OpenAI לא המציאה את הקטגוריה. Anthropic כבר מציעה רעיון דומה עם Claude Managed Agents: רתמה מוכנה שרצה בתשתית מנוהלת, בנויה על אותם ארבעה מושגים (Agent, Environment, Session, Events). OpenAI נכנסת לקטגוריה עם גרסה משלה.

![OpenAI מול Anthropic מול Grok Bot](https://kevli770.github.io/_astro/02.CI1vr3i0.jpg)

*התיעוד של* Anthropic: Managed Agents *לצד* Messages API*, אותה חלוקה בדיוק*

Grok Bot של xAI יושב קומה מעל. הוא לא API לבניית מוצר, אלא עובד מוכן בתור מוצר: כל סוכן מקבל מחשב משלו בענן, מתחבר עם המשתמש שלכם לכלים שאתם כבר עובדים איתם, נכנס גם למערכות ישנות בלי API, ועובד גם אחרי שסגרתם את הלפטופ.

|   | OpenAI Agents API | Claude Managed Agents | Grok Bot |
|---|---|---|---|
| **השכבה** | API למפתחים | API למפתחים | **מוצר למשתמש קצה** |
| **מה בונים** | מוצר משלכם על הרתמה של Codex | מוצר משלכם על הרתמה של Claude | לא בונים. מגדירים עובד |
| **מי מריץ** | OpenAI, או סנדבוקס שלכם או של שותף | Anthropic (וגם על AWS) | xAI, מחשב ענן לכל סוכן |
| **שליטה** | **גבוהה** | **גבוהה** | נמוכה, אבל פשוט להבין |

**השורה התחתונה מהסרטון: OpenAI לא בהכרח מאחור בתשתית. ל-Grok פשוט יש כרגע סיפור מוצר שקל יותר להבין. אם אתם בונים מוצר משלכם, ה-API נותן הרבה יותר שליטה. אם אתם רוצים עובד מוכן, Grok Bot או ChatGPT Work יהיו הדרך הקצרה.**

## טיפים

- התחילו מ-OpenAI-hosted. תעברו ל-Self-hosted רק כשיש סיבה אמיתית (קוד רגיש, רשת פנימית).
- תנו לסוכן הנחיות שמסתיימות בדיווח: בסוף כתוב אילו קבצים שינית ומה נשאר שבור. זה מה שהופך סטרימינג של אירועים למשהו קריא.
- הגבילו max\_concurrent\_subagents. תתי-סוכנים במקביל זה מהיר, אבל כל אחד שורף טוקנים משלו.
- שמרו את ה-session\_id בבסיס הנתונים שלכם. הסשן הוא הזיכרון, ובלעדיו כל תור מתחיל מאפס.
- חברו כלים דרך MCP במקום לכתוב wrappers. הרתמה כבר יודעת לטעון הגדרות כלים רק כשצריך (tool search).
- אין עמלה נפרדת ל-Agents API בבטא: משלמים על טוקנים, כלים בתשלום וזמן סנדבוקס. תעקבו אחרי זמן הסנדבוקס בסשנים ארוכים.

## מגבלות שכדאי לדעת

- בטא. השמות והפרמטרים עוד יכולים להשתנות (הגרסה מסומנת agents=v1).
- שמירת נתונים רק בארצות הברית כרגע, ואין תמיכה ב-Zero Data Retention.
- סשן ארוך הוא גם חשבון ארוך. משימה של שעות בסנדבוקס מנוהל עולה יותר מקריאה אחת למודל.

## לינקים

- ההכרזה הרשמית: [openai.com/index/introducing-the-agents-api](https://openai.com/index/introducing-the-agents-api/)
- תיעוד Agents API: [developers.openai.com/api/docs/guides/agents-api/overview](https://developers.openai.com/api/docs/guides/agents-api/overview)
- Quickstart: [developers.openai.com/api/docs/guides/agents-api/quickstart](https://developers.openai.com/api/docs/guides/agents-api/quickstart)
- תתי-סוכנים: [developers.openai.com/api/docs/guides/agents-api/multi-agent](https://developers.openai.com/api/docs/guides/agents-api/multi-agent)
- בחירת שכבה (Agents API / SDK / Responses): [developers.openai.com/api/docs/guides/agents](https://developers.openai.com/api/docs/guides/agents)
- הרתמה הפתוחה של Codex: [github.com/openai/codex](https://github.com/openai/codex)
- Claude Managed Agents: [platform.claude.com/docs/en/managed-agents/overview](https://platform.claude.com/docs/en/managed-agents/overview)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
