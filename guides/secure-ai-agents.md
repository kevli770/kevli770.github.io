# Secure AI Agents: סוכני AI מאובטחים עם Sandboxes ו-MCP Tunnels

> אנטרופיק שחררו עדכון מהותי לסוכנים המנוהלים (Managed Agents) שמשנה את הדרך בה Claude Agent רץ בארגונים. עד עכשיו, כש-Claude Agent קרא קובץ או הריץ קוד, הכול קרה על השרתים של אנטרופיק.

מאת קווין ליאקס. פורסם 2026-05-23. נושא: אבטחה ופרטיות.
העמוד המלא: https://kevli770.github.io/guides/secure-ai-agents

## מה זה Secure AI Agents

אנטרופיק שחררו עדכון מהותי לסוכנים המנוהלים (Managed Agents) שמשנה את הדרך בה Claude Agent רץ בארגונים. עד עכשיו, כש-Claude Agent קרא קובץ או הריץ קוד, הכול קרה על השרתים של אנטרופיק. הקוד, ה-Database, ה-API Keys, הכול עבר דרכם. לחברות עם דאטה רגיש זה היה Deal breaker.

עכשיו זה השתנה. המוח של הסוכן (התכנון וההיגיון) ממשיך לרוץ באנטרופיק, אבל ה-Execution (הרצת הקוד, גישה לכלים, חיבור לשירותים פנימיים) עובר לרוץ ב-Sandbox בתשתית שלכם, עם המדיניות הארגונית שלכם.

**בקיצור: המוח אצל הספק, הביצוע אצלכם. דאטה רגיש לא יוצא החוצה.**

## למה זה חשוב

הסיבה שזה דרמטי היא שעד היום, ארגונים פיננסיים, רפואיים, ביטחוניים וגופים עם דרישות Compliance מחמירות פשוט לא יכלו לאמץ סוכני AI לעבודה אמיתית. כל ה-Execution קרה אצל הספק, כלומר הקוד שהסוכן הריץ, הקבצים שהוא קרא ומפתחות ה-API ששימשו אותו, הכול עבר דרך תשתית חיצונית.

עם המודל החדש, ה-Sensitive Data לא עוזב את הגבולות שלכם. ה-Network Policies והלוגים הקיימים בארגון ממשיכים לעבוד. תשתית האבטחה שכבר השקעתם בה מכסה גם את הסוכן.

כשבאותו שבוע אנטרופיק, Cloudflare ו-LangChain דוחפות לכיוון הזה, מבינים לאן העולם הולך. מי שילמד עכשיו לעבוד עם Secure AI Agents הולך להיות מבוקש בארגונים.

## איך זה עובד טכנית

הארכיטקטורה החדשה מפצלת את הסוכן לשני חצאים פיזיים שמתקשרים ביניהם:

![הארכיטקטורה של Self-Hosted Sandboxes: מקור Anthropic](https://kevli770.github.io/_astro/01.eYyouIHk.png)

*הארכיטקטורה של Self-Hosted Sandboxes: מקור Anthropic*

| רכיב | מה הוא עושה ואיפה הוא רץ |
|---|---|
| `Agent Loop` | המוח. אחראי על Orchestration, ניהול Context, החזרה משגיאות וקבלת החלטות. רץ בתשתית של אנטרופיק. |
| `Sandbox Control Plane` | שכבת הניהול שמתאמת בין המוח לבין ה-Sandboxes. רצה בתשתית שלכם או אצל שותף תשתית. |
| `Sandbox Executors` | הסביבות שבפועל מריצות את הקוד וקוראות קבצים. רצות אצלכם, עם CPU, Memory, Network Policy ולוגים שאתם שולטים בהם. |

## MCP Tunnel: חיבור לרשת הפרטית שלכם

בעיה קלאסית עם סוכני AI: רוב הכלים הפנימיים בארגון לא חשופים לאינטרנט. ה-MCP Server שלכם יושב מאחורי Firewall, ב-VPC פרטי, בלי IP ציבורי. עד עכשיו זה אומר שהסוכן פשוט לא יכל להגיע אליו.

MCP Tunnel פותר את זה דרך Gateway קל שאתם מריצים אצלכם, שפותח חיבור Outbound יחיד החוצה. אין צורך לפתוח פורטים נכנסים, אין צורך לחשוף את ה-MCP Server, התעבורה מוצפנת מקצה לקצה.

![איך MCP Tunnel מחבר את הסוכן לרשת הפרטית: מקור Anthropic](https://kevli770.github.io/_astro/02._kMv8H4E.png)

*איך MCP Tunnel מחבר את הסוכן לרשת הפרטית: מקור Anthropic*

**התרחישים שבהם זה משחק תפקיד:**

- חיבור ל-Database פנימי שלא חשוף החוצה
- גישה ל-Internal API של החברה ללא חשיפה ציבורית
- שימוש בכלי Observability וניטור פנים-ארגוניים
- Integration עם Identity Providers ארגוניים

## שותפי תשתית

אנטרופיק יוצאת עם ארבעה שותפי השקה. כל אחד מהם מציע פרופיל אחר של Sandbox, ובחירת הספק תלויה בסוג העבודה שהסוכן עושה.

| שותף | מה הוא מביא לשולחן |
|---|---|
| **Cloudflare** | MicroVMs ו-Isolates בקנה מידה גלובלי. Zero-Trust Secrets Injection, Proxy מותאם ל-Egress Control וחיבור לשירותים פנימיים דרך הרשת של Cloudflare. |
| **Daytona** | Composable Computers: סביבות Stateful ארוכות חיים עם גישת SSH או URL מאומת. אפשר להשהות ולשחזר עם שמירה מלאה של ה-State. |
| **Modal** | פלטפורמת ענן ל-AI Workloads עם זמני התנעה של שנייה ופחות, תמיכה במאות אלפי Sandboxes במקביל וגישה ל-CPU או GPU לפי דרישה. |
| **Vercel** | VM Security עם VPC Peering וזמני התנעה של מילישניות. הזרקת Credentials בגבול הרשת כך שסודות לא נכנסים בכלל ל-Sandbox. |

## איך מתחילים

המסלול הקצר ביותר להבין את הפיצ'ר הוא להפעיל את ה-Code Execution Tool מה-API של אנטרופיק. זה ה-Sandbox המנוהל של אנטרופיק, וזו הדרך לראות איך נראית ההפרדה בין מוח לביצוע.

### שלב 1: קריאה למודל עם Code Execution

דוגמת קריאה מינימלית ל-Claude Opus 4.7 עם ה-Tool מופעל:

```bash
curl https://api.anthropic.com/v1/messages \
  --header "x-api-key: $ANTHROPIC_API_KEY" \
  --header "anthropic-version: 2023-06-01" \
  --header "content-type: application/json" \
  --data '{
    "model": "claude-opus-4-7",
    "max_tokens": 4096,
    "messages": [{ "role": "user",
      "content": "Analyze this CSV and plot the trend" }],
    "tools": [{
      "type": "code_execution_20250825",
      "name": "code_execution"
    }]
  }'
```

### שלב 2: SDK בפייתון

```python
import anthropic
client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-opus-4-7",
    max_tokens=4096,
    messages=[{"role":"user",
               "content":"Run a quick stats summary"}],
    tools=[{"type":"code_execution_20250825",
            "name":"code_execution"}])
```

### שלב 3: מעבר ל-Self-Hosted Sandbox

ברגע שהבנתם איך ה-Tool עובד מול ה-Sandbox המנוהל של אנטרופיק, אפשר להחליף את ה-Executor ל-Sandbox שאתם מריצים אצלכם דרך אחד משותפי התשתית (Cloudflare, Daytona, Modal או Vercel). הזרימה של ה-API נשארת זהה, רק שכבת ה-Execution עוברת לתחום שלכם.

### שלב 4: MCP Tunnel לכלים פנימיים

מפעילים את ה-MCP Tunnel דרך הגדרות ה-Workspace ב-Claude Console (נדרשים הרשאות Admin). מתקינים את ה-Gateway הקל בתוך הרשת הפרטית, ומחברים אותו ל-MCP Server הקיים. הסוכן יוכל לקרוא ל-MCP Server דרך ה-Tunnel בלי שתחשפו פורטים החוצה.

## Before / After: איפה רץ ה-Execution

| מאפיין | לפני (Hosted) | אחרי (Managed) |
|---|---|---|
| **מיקום הרצת הקוד** | שרתי אנטרופיק | **התשתית שלכם** |
| **מיקום ה-Data** | עובר דרך הספק | **נשאר בארגון** |
| **API Keys ו-Secrets** | נשלחים החוצה | **נשארים מקומית** |
| **Network Policy** | של הספק | **המדיניות שלכם** |
| **גישה לכלים פנימיים** | נדרש לחשוף החוצה | **MCP Tunnel סגור** |
| **Audit Logging** | בלוגים של הספק | **בלוגים שלכם** |

## מתי באמת צריך את זה

לא כל סוכן צריך לרוץ ב-Sandbox עצמאי. ה-Code Execution Tool של אנטרופיק (ה-Hosted Sandbox) מצוין לרוב המקרים. המעבר ל-Self-Hosted Sandbox עם MCP Tunnel הופך לקריטי בארבעה תרחישים מובהקים:

### 1. דאטה רגיש שאסור לעזוב את הארגון

רשומות רפואיות (HIPAA), נתוני לקוחות פיננסיים (PCI-DSS), קוד מקור קנייני או IP מסווג. ברגע שיש שכבת רגולציה שמחייבת Data Residency, ה-Execution חייב לרוץ בתשתית שאתם שולטים בה.

### 2. גישה לכלים פנימיים שאינם חשופים החוצה

Database פנימי, Internal CRM, מערכת Ticketing ארגונית, Kubernetes Cluster פנימי. במקום לחשוף אותם דרך VPN או Reverse Proxy, ה-MCP Tunnel נותן לסוכן גישה דרך חיבור Outbound יחיד.

### 3. עומסי GPU או Workloads כבדים

עיבוד תמונה, אימון מודלים קטנים, סימולציות מדעיות, רינדור 3D. אצל אנטרופיק יש מגבלות משאבים, אצל Modal או Daytona אפשר להקצות GPU ייעודי לסוכן ספציפי.

### 4. Audit ו-Compliance פנים-ארגוני

כשמחלקת Security רוצה לראות כל פקודה שהסוכן הריץ ב-SIEM הארגוני, וכשהדוחות הרגולטוריים חייבים להראות שהקוד רץ בסביבה מבוקרת. Self-Hosted Sandbox פותר את שניהם.

## טיפים

- התחילו עם ה-Code Execution Tool של אנטרופיק כדי ללמוד את ה-API לפני שמרימים Sandbox עצמאי
- בחרו את שותף התשתית לפי סוג ה-Workload: Modal ל-GPU ולעומסים מקבילים, Daytona ל-Sessions ארוכים, Cloudflare ל-Edge ו-Vercel ל-Web Tasks
- ה-MCP Tunnel הוא הכלי הנכון לחיבור Databases פנימיים ו-Internal APIs, לא לחשוף אותם החוצה דרך Reverse Proxy
- Audit Logging הופך להיות שלכם, ודאו שאתם מחברים את לוגי ה-Sandbox ל-SIEM הארגוני
- מגדירים מראש מגבלות CPU ו-Memory לכל סוכן כדי למנוע Runaway Loops שצורכים משאבים
- Code Execution Tool לא זמין כרגע ל-Zero Data Retention, אם זה דרישה רגולטורית, מתחילים ישר ב-Self-Hosted Sandbox
- MCP Tunnel נמצא ב-Research Preview, צריך לבקש גישה מ-Anthropic לפני שמשתמשים

## לינקים

- ההודעה הרשמית של אנטרופיק על Managed Agents: [`claude.com/blog/claude-managed-agents-updates`](https://claude.com/blog/claude-managed-agents-updates)
- דוקומנטציית Code Execution Tool: [`docs.anthropic.com/code-execution-tool`](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/code-execution-tool)
- Cloudflare Agents Platform: [`developers.cloudflare.com/agents`](https://developers.cloudflare.com/agents/)
- LangChain LangGraph: [`langchain.com/langgraph`](https://www.langchain.com/langgraph)
- Daytona: [`daytona.io`](https://www.daytona.io)
- Modal: [`modal.com`](https://modal.com)
- Vercel Sandbox: [`vercel.com`](https://vercel.com)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
