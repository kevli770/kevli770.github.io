# MCP Servers: איך לחבר את Claude לכל הכלים שלכם

> MCP זה ראשי תיבות של Model Context Protocol. תחשבו על זה ככה: Claude מדבר עברית, Gmail מדבר סינית, ו-Slack מדבר ספרדית.

מאת קווין ליאקס. פורסם 2026-04-28. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/mcp-servers

## מה זה MCP

MCP זה ראשי תיבות של Model Context Protocol. תחשבו על זה ככה: Claude מדבר עברית, Gmail מדבר סינית, ו-Slack מדבר ספרדית. שרת MCP הוא המתורגמן שמחבר ביניהם.

**Anthropic המציאה שפה אחת שבה כל אפליקציה בעולם יכולה "לדבר" עם Claude. כל חיבור כזה נקרא שרת MCP. ברגע שמתקינים שרת MCP, Claude מקבל יכולות חדשות, לקרוא מיילים, לפתוח קבצים, לשלוח הודעות בסלאק, וכל מה שתרצו.**

היום יש כבר אלפי שרתי MCP מוכנים: Gmail, Drive, Slack, Notion, GitHub, YouTube, Linear, Jira ועוד.

## איך זה עובד

MCP פועל בארכיטקטורה פשוטה:

- Client (Claude Code, Claude Desktop), מי ששולח בקשות
- Server (שרת MCP של Gmail/Slack/וכו), מי שעונה ומפעיל את השירות
- Protocol (MCP): השפה הסטנדרטית שביניהם

*כשאתם אומרים ל-Claude "שלח מייל לדן", הוא לא באמת שולח, הוא מבקש משרת ה-MCP של Gmail לשלוח, וזה עובד.*

## שרתי MCP פופולריים

| שרת | מה הוא נותן ל-Claude |
|---|---|
| **Gmail** | לקרוא ולשלוח מיילים, לחפש תיבת דואר, לסנן |
| **Google Drive** | לפתוח קבצים, ליצור מסמכים, לחפש תוכן |
| **Slack** | לקרוא הודעות, לשלוח, לסכם ערוצים |
| **Notion** | ליצור דפים, לחפש בכל ה-workspace, לעדכן databases |
| **GitHub** | לפתוח issues, לבדוק PRs, לקרוא repos |
| **Playwright** | להפעיל דפדפן, ללחוץ, להקליד, ליצור automation |
| **Linear / Jira** | לנהל משימות, לפתוח tickets, לעקוב אחרי sprints |
| **YouTube** | לקבל תמלולים, לחפש סרטונים, להוציא מטא-דאטה |

## איך מתקינים MCP

### שלב 1: למצוא את השרת

חיפוש פשוט בגוגל:

```
"<שם הכלי> MCP"
```

למשל: "gmail mcp" או "playwright mcp". בדרך כלל תמצאו רפו ב-GitHub או בדף הרשמי של הכלי.

### שלב 2: התקנה דרך Claude Code

הפקודה הסטנדרטית:

```bash
claude mcp add <שם> <command>
```

דוגמה ל-Playwright MCP:

```bash
claude mcp add playwright npx @playwright/mcp@latest
```

### שלב 3: לצאת ולהיכנס מחדש ל-Claude

Claude טוען שרתי MCP בהתחלה של השיחה. צריך לעשות exit ולהתחיל מחדש.

### שלב 4: לבדוק שהכל עובד

```
/mcp
```

Claude יראה לכם רשימה של כל שרתי ה-MCP הזמינים והכלים שכל אחד מספק.

## דוגמה מלאה: Playwright MCP

Playwright MCP הוא דוגמה מצוינת, הוא מאפשר ל-Claude לפתוח דפדפן ולפעול בו אוטומטית. הוא נבנה על ידי Microsoft (יוצרי Playwright).

### מה הוא נותן ל-Claude

- ניווט ושליטה: click, hover, type, drag, navigate
- אינטראקציה עם דפים: מילוי טפסים, העלאת קבצים, dialogs
- בדיקה: snapshots (accessibility tree), screenshots, console messages
- ניהול tabs: פתיחה, סגירה, מעבר בין tabs
- Network mocking: מענה מותאם, סימולציית offline
- ניהול storage: cookies, localStorage, sessionStorage
- יצירת PDFs ויומני trace

### התקנה

```bash
claude mcp add playwright npx @playwright/mcp@latest
```

### דרישות

- Node.js 18 ומעלה
- Claude Code, Claude Desktop, Cursor, או VS Code

### דוגמת שימוש

אחרי התקנה, אפשר פשוט להגיד ל-Claude:

```
"פתח את rotter.net וקרא את 3 הכותרות הראשיות"
"מלא טופס יצירת קשר באתר שלי, השתמש בנתוני test"
"קח צילום של https://example.com/page"
"פתח את LinkedIn ושלח לי סיכום של המשרות החדשות"
```

### הגדרות מתקדמות

Playwright MCP תומך באפשרויות רבות:

- --browser chrome|firefox|webkit|msedge
- --headless להרצה ללא UI
- --device לדמות מובייל ספציפי
- --user-data-dir לשמירת session
- --allowed-hosts להגביל גישה לאתרים מסוימים

## איפה למצוא עוד שרתי MCP

מקורות מומלצים:

- GitHub: לחפש "\<service\> mcp" או mcp-servers
- Anthropic Docs: רשימת שרתי MCP רשמיים
- mcp.so: אוסף קהילתי של שרתי MCP
- awesome-mcp-servers ב-GitHub: רשימה מאורגנת

## טיפים

- אחרי התקנה, תמיד להריץ /mcp לוודא שהשרת מופיע ברשימה
- שרת לא מופיע? לעשות exit ולהיכנס שוב ל-Claude Code
- הרבה שרתים דורשים API keys, בדקו ב-README של כל שרת
- מומלץ להתקין רק שרתים מ-publishers מוכרים (Microsoft, Anthropic, GitHub וכו)
- שרתי MCP רצים מקומית: הנתונים שלכם לא יוצאים לאף ענן
- אפשר להגביל הרשאות של שרת MCP דרך הגדרות פרויקט

## לינקים

- Playwright MCP: [github.com/microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp)
- פרוטוקול MCP רשמי: [modelcontextprotocol.io](https://modelcontextprotocol.io)
- רשימת שרתים: [awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers)
- דוקומנטציה: [Claude Code MCP](https://code.claude.com/docs/en/mcp)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
