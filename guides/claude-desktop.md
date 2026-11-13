# Claude Desktop: Dispatch, Computer Use וסנכרון מובייל

> האפליקציה הרשמית של Anthropic לעבודה עם Claude על המחשב.

מאת קווין ליאקס. פורסם 2026-04-04. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/claude-desktop

## מה זה Claude Desktop

האפליקציה הרשמית של Anthropic לעבודה עם Claude על המחשב. במקום לעבוד דרך הדפדפן, מקבלים אפליקציה מלאה עם שלוש יכולות רציניות:

- Dispatch: שליחת סוכנים לעבוד ברקע
- Computer Use: Claude רואה ושולט במסך שלך
- סנכרון מובייל: המשך מהטלפון

## הורדה והתקנה

נכנסים ל:

```
https://claude.ai/download
```

|   | Windows | macOS |
|---|---|---|
| **דרישות** | Windows 10+ (64-bit) | macOS 12 Monterey+ |
| **קובץ** | `.exe installer` | `.dmg` |
| **התקנה** | להריץ installer ולעקוב | לגרור ל-Applications |

אחרי ההתקנה, להתחבר עם חשבון Anthropic (אימייל, Google, או SSO).

## Dispatch: סוכנים ברקע

Dispatch מאפשר לשגר סוכני Claude Code שעובדים ברקע. נותנים משימה, Claude עובד, אפשר להמשיך לעשות דברים אחרים. בודקים אחר כך איפה הלך.

### דרישות

Claude Code מותקן דרך npm:

```bash
npm install -g @anthropic-ai/claude-code
```

### שימוש

```bash
claude --background "refactor the auth module"
```

או דרך Claude Desktop: לפתוח צ'אט חדש, לתת משימה, ולשלוח לרקע.

### פקודות שימושיות

| פקודה | מה עושה |
|---|---|
| `claude --background` | מריץ סוכן ברקע |
| `claude --resume <id>` | חוזר לסשן קיים |
| `claude --max-turns 10` | מגביל מספר צעדים |
| `claude -p "prompt"` | מריץ ומדפיס (לא אינטראקטיבי) |

## Computer Use: שליטה במסך

Computer Use נותן ל-Claude לראות את המסך שלך ולשלוט בו. Claude מצלם מסך, מזיז עכבר, לוחץ, מקליד, וגולל.

### הפעלה ב-Claude Desktop

- לפתוח את Claude Desktop
- להיכנס ל-Settings
- לחפש "Computer Use" או "Beta Features"
- להפעיל את ה-toggle

### מה Claude יכול לעשות

- לצלם מסך (screenshots)
- להזיז עכבר וללחוץ (שמאל, ימין, כפול)
- להקליד טקסט וללחוץ קיצורי מקלדת
- לגלול בדפדפן, לנווט באתרים, למלא טפסים

### שימוש דרך API (למפתחים)

```python
import anthropic

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-sonnet-4-20250514",
    tools=[{
        "type": "computer_20250124",
        "name": "computer",
        "display_width_px": 1920,
        "display_height_px": 1080,
    }],
    messages=[{"role": "user",
        "content": "Open browser"}]
)
```

### Docker: סביבה בטוחה

Anthropic מספקים Docker container עם סביבה מבודדת:

```bash
docker run -p 8080:8080 \
  ghcr.io/anthropics/anthropic-computer-use-demo:latest
```

## סנכרון מובייל

השיחות מסתנכרנות אוטומטית בין כל המכשירים שמחוברים לאותו חשבון.

### התקנה על הטלפון

- להוריד Claude מה-App Store (iOS) או Google Play (Android)
- להתחבר עם אותו חשבון שמשתמשים ב-Desktop
- השיחות מסתנכרנות אוטומטית דרך הענן

## הגדרות

### קובץ קונפיגורציה

Claude Desktop משתמש בקובץ JSON להגדרות מתקדמות:

| OS | נתיב |
|---|---|
| **macOS** | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| **Windows** | `%APPDATA%\Claude\claude_desktop_config.json` |

### דוגמה לקונפיג MCP Server

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "some-mcp-server"],
      "env": { "API_KEY": "your-key" }
    }
  }
}
```

### הגדרות Claude Code CLI

| קובץ | תפקיד |
|---|---|
| `~/.claude/settings.json` | הגדרות גלובליות |
| `.claude/settings.json` | הגדרות פרויקט |
| `CLAUDE.md` | כללים והוראות |

התחברות: `claude login`

## שווה לדעת

- Computer Use עדיין בגרסת beta. עובד אבל Claude יכול לטעות
- Claude "רואה" דרך screenshots, לא DOM. רזולוציה 1920x1080 עובדת הכי טוב
- Claude יסרב לבצע פעולות מזיקות ויבקש אישור
- חלק מהיכולות דורשות מנוי Pro או Team

## קרדיטים

מבוסס על התיעוד של [Anthropic Docs](https://docs.anthropic.com)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
