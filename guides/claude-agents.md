# Claude Agents: המדריך המלא לפקודה החדשה של Claude Code שמשנה איך עובדים במקביל

> Agent View זה מסך אחד שמרכז את כל ה-sessions של Claude Code שרצים אצלכם ברקע. במקום לפתוח עשרה טרמינלים, לעבור בין חלונות, ולנסות לזכור מי סיים, מי תקוע ומי מחכה לתשובה, יש לכם dashboard אחד שמראה את הכל בזמן אמת.

מאת קווין ליאקס. פורסם 2026-05-25. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/claude-agents

## מה תלמדו במדריך הזה

Claude Code השיקה ב-2026 פקודה אחת שמשנה את כל הזרימה של עבודה עם AI agents במקביל. במקום עשרה טרמינלים פתוחים, יש לכם dashboard אחד. במקום לאבד את החוט בין שיחות, אתם רואים את הכל במבט.

**המדריך הזה הוא צעד-אחרי-צעד מהחיפוש הראשון ועד המאסטר הסופי. תקראו בערך 12 דקות ותסיימו עם:**

- **הבנה מלאה** של מה זה Agent View ומה הוא מחליף
- **מתי וכיצד להפעיל** את הפקודה `claude agents`
- **שליטה ב-13 קיצורי המקלדת** שהופכים אתכם למהירים פי 10
- **שלושה patterns של עבודה במקביל** מהיומיום של מפתחים
- **איך מנהלים sessions מ-shell** בלי לפתוח את ה-UI, לסקריפטים ואוטומציות
- **כל המגבלות** של ה-Research Preview ואיך לעקוף אותן

*Agent View: צילום מסך רשמי מהדוקומנטציה של Anthropic*

## מה זה Agent View

Agent View זה מסך אחד שמרכז את כל ה-sessions של `Claude Code` שרצים אצלכם ברקע. במקום לפתוח עשרה טרמינלים, לעבור בין חלונות, ולנסות לזכור מי סיים, מי תקוע ומי מחכה לתשובה, יש לכם dashboard אחד שמראה את הכל בזמן אמת.

**הפקודה** `claude agents` **יצאה במאי 2026 כ-Research Preview של Anthropic. כל session שאתם מדפצ'ים הופך לתהליך עצמאי שממשיך לרוץ ברקע גם כשסוגרים את הטרמינל, ויש supervisor process שמנהל אותם.**

### הקונספט בתמונה אחת

![Claude Code: הכלי שעליו Agent View רץ](https://kevli770.github.io/_astro/01.B2S6oI_K.png)

*Claude Code: הכלי שעליו Agent View רץ*

Agent View הוא שכבה חדשה מעל `Claude Code`. כל session שתפעילו עם הפקודה הזו ימשיך לרוץ באופן עצמאי, ותוכלו לקפוץ בין כל ה-sessions בנוחות.

## דרישות מקדימות

לפני שאתם רצים להפעיל את הפקודה, וודאו שיש לכם:

- **מנוי בתשלום ל-Claude** (Pro או Max), background sessions צורכים מה-quota שלכם בדיוק כמו sessions רגילים
- **Claude Code גרסה 2.1.139 ומעלה**, בדקו עם הפקודה הבאה:

```bash
claude --version
```

- טרמינל מודרני: `Warp`, `iTerm2`, `Windows Terminal` או דומה. הקיצורים הויזואליים עובדים יותר טוב בטרמינלים חדשים
- פרויקט שאתם עובדים עליו: יחיד או מספר ריפוזיטוריים

## איך משדרגים את Claude Code

אם הגרסה שלכם מתחת ל-2.1.139, ה-Auto-Updater של Claude Code יעדכן אתכם אוטומטית בהפעלה הבאה. כדי לכפות עדכון מיידי:

```bash
claude update
```

אחרי השדרוג, ה-supervisor process יעבור אוטומטית לגרסה החדשה, לא צריך לעצור sessions שכבר רצים. זה אחד מהמרכיבים הכי חשובים: התהליכים שלכם ממשיכים לעבוד גם כשהשתנה הקוד מתחתיהם.

## הפעלה ראשונה: שלב אחר שלב

### שלב 1: פותחים טרמינל בתיקיית הפרויקט

נכנסים לתיקייה שבה אתם עובדים על הפרויקט שלכם. אם זה לא Git repo, הפיצ'ר של בידוד קבצים לא יעבוד אוטומטית (יש דרך לעקוף, נסביר בהמשך).

```bash
cd ~/projects/my-app
```

### שלב 2: מפעילים את הפקודה

```bash
claude agents
```

המסך נפתח עם שורת קלט בתחתית (לדיספצ' של session חדש) וטבלה ריקה (כי עדיין לא דיספצ'תם כלום).

### שלב 3: מקלידים פרומפט ראשון

בשורת הקלט הקלידו משימה, למשל:

```
investigate the flaky SettingsChangeDetector test
```

לחצו `Enter` וייווצר session ראשון. הוא יופיע כשורה חדשה במצב Working.

**טיפ חשוב: כל פרומפט שאתם מקלידים מתחיל session חדש. אם אתם רוצים להמשיך שיחה קיימת, תפתחו אותה עם Enter או חץ ימינה ותכתבו שם, אל תקלידו בשורה התחתונה.**

![Agent View: גרסה בהירה. שני הצבעים זמינים בהתאם להגדרות הטרמינל שלכם](https://kevli770.github.io/_astro/02.BylExmkY.png)

*Agent View: גרסה בהירה. שני הצבעים זמינים בהתאם להגדרות הטרמינל שלכם*

## הבנת הקבוצות בדשבורד

Agent View מקבץ את ה-sessions לפי קטגוריות, מסודרות כך שמה שדורש מכם תשומת לב נמצא תמיד למעלה:

| קבוצה | מה היא מכילה |
|---|---|
| `Pinned` | sessions שנעצתם ידנית עם Ctrl+T. ה-supervisor לא מכבה אותם גם כשהם לא פעילים |
| `Ready for review` | sessions שפתחו Pull Request: מחכים לסקירה ומיזוג מצדכם |
| `Needs input` | Claude שואל אתכם שאלה או מבקש החלטת permissions, הקבוצה הכי דחופה |
| `Working` | sessions פעילים שמריצים כלים או מייצרים תשובה ברקע |
| `Completed` | גמרו את המשימה (כולל הצליחו, נכשלו, ונעצרו) |

## מצבי session ומה אייקון מציג

כל שורה מתחילה באייקון שצבעו ותנועתו מציגים את המצב הנוכחי של ה-session:

| מצב | אייקון | משמעות |
|---|---|---|
| **Working** | מנוסה (animated) | Claude פעיל: מריץ כלים או מייצר תשובה |
| **Needs input** | צהוב | מחכה לתשובה ספציפית או החלטת permissions |
| **Idle** | דהוי | ה-session ריק וזמין לפרומפט הבא שלכם |
| **Completed** | ירוק | המשימה הסתיימה בהצלחה |
| **Failed** | אדום | השגיאה תפסה את התהליך: תפתחו לראות מה קרה |
| **Stopped** | אפור | עצרתם אותו ידנית עם Ctrl+X או claude stop |

## קיצורי מקלדת: שיודעים אותם, אתם מהירים פי 10

ב-Agent View אפשר ללחוץ `?` כדי לראות את כל הקיצורים. הינה הכי שימושיים:

| קיצור | פעולה |
|---|---|
| `Up / Down` | ניווט בין שורות |
| `Space` | Peek: פותח חלון תצוגה מקדימה של הפלט האחרון או השאלה שמחכה |
| `Enter / Right` | Attach: נכנסים לשיחה המלאה. ה-Agent View נסגר וזה הופך ל-Claude Code רגיל |
| `Left` | Detach: על שורת קלט ריקה בתוך session, חוזרים ל-Agent View |
| `Shift+Enter` | דיספצ' session חדש ונכנס אליו מיד |
| `Alt+1..9` | Attach מהיר ל-session 1-9 בתיקייה הנוכחית |
| `Ctrl+T` | Pin: מצמיד session לראש הרשימה ומונע ממנו להיכבות |
| `Ctrl+R` | שינוי שם ה-session |
| `Ctrl+S` | מעבר בין קיבוץ לפי מצב לקיבוץ לפי תיקייה |
| `Ctrl+X` | עצירת session. לחיצה שניה תוך 2 שניות = מחיקה |
| `Ctrl+G` | פותח את שורת הפרומפט ב-VISUAL או EDITOR של מערכת ההפעלה |
| `Esc` | סגירת peek, ניקוי הקלט, או יציאה |
| `?` | הצגת כל הקיצורים |

*הדשבורד עם הסטטוסים והפוטר שמראה את הקיצורים החשובים (enter to open, space to reply, ctrl+x to delete)*

## Peek & Reply: הפיצ'ר הכי חשוב

זה הפיצ'ר שבזכותו Agent View שווה את הזמן ללמוד אותו. במקום לפתוח שיחה שלמה בשביל לראות מה קורה, לוחצים `Space` על שורה ונפתח חלון תצוגה מקדימה.

מה מקבלים ב-Peek:

- הפלט האחרון של ה-session (לא כל ההיסטוריה, רק מה שרלוונטי כרגע)
- השאלה שהוא מחכה לתשובה עליה (אם הוא בסטטוס Needs input)
- כל ה-Pull Requests שהוא פתח
- שורת קלט לתשובה מהירה

### טריקים שיוקרים זמן ב-Peek

- `Up / Down`: דיפדוף בין sessions בלי לסגור את ה-peek
- `Right`: מ-peek ישר ל-attach
- `Tab`: אם ה-session תקוע, ממלא תשובה מומלצת שאפשר לערוך לפני שליחה
- אם השאלה היא רב-ברירתית (1/2/3), פשוט לוחצים את הספרה
- מתחילים תשובה עם `!` כדי לשלוח פקודת Bash במקום טקסט רגיל

## שלוש דרכים לדיספצ' session חדש

### 1. מתוך Agent View

הקלידו פרומפט בשורה התחתונה ו-Enter. ה-session ייווצר אוטומטית בתיקייה שממנה פתחתם את Agent View. אפשר גם להדביק תמונה (screenshot, diagram) בשורת הפרומפט.

מודיפיקטורים מתקדמים בפרומפט:

| תחביר | פעולה |
|---|---|
| `@subagent-name` | הרצה של subagent ספציפי בתור ה-agent הראשי של ה-session |
| `@repo-name` | מריץ את ה-session בריפו שמתחת לתיקייה ההורית |
| `/skill-name` | מציע סקילים לדיספצ' כפרומפט |
| `#123 or PR URL` | אם כבר יש session על אותו PR, בוחר אותו במקום ליצור חדש |

### 2. מתוך session פעיל

נמצאים בתוך session ורוצים להעביר אותו לרקע? הקלידו:

```
/bg
```

או `/background`: אותה פקודה. אפשר גם להוסיף הוראה אחרונה:

```
/bg run the test suite and fix any failures
```

קיצור עוד יותר: לחיצה על `Left` על שורת קלט ריקה, מעבירה את ה-session לרקע ופותחת Agent View באותה פעולה.

### 3. ישירות מה-shell

רוצים להריץ session מהטרמינל בלי לפתוח Agent View?

```bash
claude --bg "investigate the flaky checkout test"
```

עם שם מותאם ו-subagent ספציפי:

```bash
claude --bg --agent code-reviewer --name "pr-1234-review" "address review comments on PR 1234"
```

Claude יחזיר ID קצר שאפשר להשתמש בו אחר כך עם פקודות `claude attach`, `claude logs`, `claude stop`.

## איך Claude מבודד עריכות בין sessions במקביל

כש-3 sessions עורכים את אותו פרויקט, איך הם לא דורסים אחד את השני? Claude יוצר אוטומטית **Git Worktrees** נפרדים לכל session ב-

```
.claude/worktrees/
```

כל session קורא מאותו checkout אבל כותב ל-worktree משלו. כשמסיימים, אפשר למרג' את השינויים או למחוק את ה-worktree עם ה-session.

**הערה חשובה: מחיקת session דרך** `Ctrl+X` **כפול = מחיקה גם של ה-worktree, כולל שינויים שלא קומיטתם. לפני שמוחקים, תוודאו שמרגתם או דחפתם את העבודה.**

אם אתם רוצים לבטל את הבידוד (לפרויקט שלא תומך ב-worktrees), הוסיפו לקובץ `.claude/settings.json`:

```json
{
  "worktree": {
    "bgIsolation": "none"
  }
}
```

## שלוש דוגמאות מעשיות מהיומיום

### דוגמה 1: PR ביקורת ובאג fix במקביל

יש לכם PR שצריך לסקור, ובאג שדחוף לתקן. במקום לעבוד עליהם בטור:

```bash
claude agents
```

ואז בשורת הקלט (פעמיים, פעם כל פרומפט):

```
review PR #1234 and suggest improvements
investigate the flaky SettingsChangeDetector test in src/settings/
```

שני sessions רצים במקביל. כשאחד מבקש קלט: האייקון שלו צהוב, אתם רואים את זה מיד, פותחים peek, עונים, סוגרים. השני ממשיך לרוץ בלי הפרעה.

### דוגמה 2: שלושה sub-tasks של פרויקט אחד

פרויקט גדול שאפשר לפצל ל-3 משימות עצמאיות:

```
refactor the auth middleware to use JWT instead of sessions
write unit tests for the new UserService class
update the API documentation with the new /v2/users endpoint
```

כל אחד עובד ב-worktree משלו, אין דריסות. כשכולם מסיימים, אתם רואים סטטוס ירוק לכולם, נכנסים, סוקרים, ומרגים. ה-supervisor שומר את כולם רצים גם אם סגרתם את הטרמינל בינתיים.

### דוגמה 3: שימוש כ-Agent View כנקודת כניסה ראשית

הטיפ הכי גדול: במקום להפעיל `claude` כל פעם בנפרד, תפעילו תמיד את

```bash
claude agents
```

ותדפצ'ו את כל המשימות משם. כשאתם רוצים שיחה ישירה: `Right` כדי להיכנס, `Left` כדי לחזור. זה משנה את כל הזרימה, אתם עוברים משיחה אחת ללוח בקרה.

## אינטגרציה עם Pull Requests

כש-session פותח PR: מופיע נקודת סטטוס בקצה הימני של השורה (linkable hyperlink בטרמינלים שתומכים):

| צבע נקודה | סטטוס PR |
|---|---|
| **צהוב** | מחכה ל-checks או לסקירה, או checks נכשלו |
| **ירוק** | Checks עברו ואין סקירה חוסמת, מוכן למרג' |
| **סגול** | נמרג' |
| **אפור** | Draft או סגור |

ברוב המקרים, השורה הזו היא הנקודה שאתם מרימים את התוצאה: סוקרים את ה-PR ומרגים כשהנקודה הופכת ירוקה.

## סינון: איך למצוא session מהר

כשיש לכם 20+ sessions, פשוט להקליד בשורת הפרומפט כדי לסנן (במקום לדפצ'):

| סינון | מראה |
|---|---|
| `a:<name>` | sessions שמריצים subagent בשם הזה |
| `s:working` | רק sessions שעובדים כרגע |
| `s:blocked` | כל מה שמחכה לכם: מצוין לפתיחת היום |
| `#1234` | ה-session שעובד על ה-PR הזה |

## ניהול sessions מה-shell (לסקריפטים)

לכל session יש ID קצר שמודפס כשמדפצ'ים. אפשר להשתמש בו מה-terminal בלי לפתוח את Agent View:

```bash
claude agents --json                   # הדפסת כל ה-sessions כ-JSON
claude attach <id>                     # נכנסים לשיחה
claude logs <id>                       # רואים את הפלט האחרון
claude stop <id>                       # עוצרים session
claude respawn <id>                    # מפעילים מחדש (שומר שיחה)
claude respawn --all                   # מפעילים מחדש את כל ה-sessions
claude rm <id>                         # מסירים מהרשימה
claude daemon status                   # מצב ה-supervisor process
```

**טיפ מתקדם: הפקודה** `claude agents --cwd ~/projects/my-app` **פותחת Agent View מסונן רק ל-sessions של פרויקט ספציפי. שימושי כשעובדים על כמה פרויקטים מקבילים.**

## טיפים שמבדילים בין משתמש למקצוען

- **פין sessions חשובים עם Ctrl+T -** ה-supervisor כובה אוטומטית sessions ש-idle יותר משעה. פין מונע את זה ושומר אותם זמינים מיד.
- **השתמשו ב-claude agents כנקודת הכניסה הקבועה -** במקום claude רגיל. אתם תרוויחו לוח בקרה במקום שיחה בודדת.
- **העברה מהירה עם החץ השמאלי -** בכל session, על שורת קלט ריקה, חץ שמאלה = מעבר ל-Agent View. אפשר לכבות את זה ב-/config אם זה מפריע.
- **שמות תיאוריים עם --name -** השם האוטומטי נוצר מהפרומפט. אם המשימה ארוכת-טווח, תנו לה שם קצר וברור עם Ctrl+R.
- **שלבו עם Skills -** אם יש לכם משימות חוזרות (בדיקת PRs, סקירת אבטחה, רענון תיעוד), תארזו אותן כ-Skill ותדפצ'ו עם /skill-name.
- **Permission mode מתקדם -** פתחו את Agent View עם --permission-mode plan ו--model opus כדי לקבוע ברירת מחדל לכל ה-sessions שתדפצ'ו.
- **Bash מתוך peek -** תשובה שמתחילה ב-! נשלחת כפקודת Bash. למשל !ls יריץ ls במקום לשלוח כטקסט.
- **פיצול קיבוץ ל-directory -** Ctrl+S עובר בין קיבוץ לפי מצב לקיבוץ לפי תיקייה. שימושי כשעובדים בריבוי פרויקטים.

## מגבלות שצריך לדעת לפני שמתחילים

- **Quota מהיר יותר פי N -** 10 sessions במקביל = quota פי 10. תכננו את ה-usage שלכם בהתאם.
- **Sessions לוקאליים בלבד -** רצים על המחשב שלכם. נשמרים על sleep, אבל נעצרים על shutdown. כשמדליקים מחדש: הם בסטטוס Failed עד שמפעילים אותם מחדש.
- **Research Preview -** הממשק והקיצורים עשויים להשתנות. תעקבו אחרי הדוקומנטציה הרשמית.
- **Worktrees נמחקים עם ה-session -** Ctrl+X+Ctrl+X מוחק גם את ה-worktree, כולל שינויים שלא קומיטתם. תקפידו לקומט קודם.
- **רב-ברירתי לא נתמך אצל כולם -** בחיבור דרך Bedrock, Vertex AI או Foundry, Agent View לא תמיד נפתח. עדכנו לגרסה האחרונה ובדקו.

## פתרון בעיות נפוצות

### הפקודה claude agents מציגה רק רשימת subagents ויוצאת

הגרסה שלכם ישנה. הריצו `claude update` ונסו שוב. אם זה עדיין לא עובד, בדקו אם Agent View מושבת ב-

```
disableAgentView: true
```

ב-settings או במשתנה הסביבה CLAUDE\_CODE\_DISABLE\_AGENT\_VIEW.

### Sessions מסומנים כ-Failed אחרי הדלקה מחדש

Shutdown או restart של המכונה עוצרים sessions. תיכנסו ל-session, תעשו attach/peek/reply: וה-supervisor יפעיל אותו מחדש מאותה נקודה.

### Session איטי לתגובה אחרי attach

Session ש-idle יותר משעה: ה-supervisor כבה לו את התהליך. כשעושים attach, הוא מתחיל תהליך חדש שלוקח כמה שניות. תפינו עם `Ctrl+T` sessions שצריכים להישאר זמינים.

### .claude/worktrees/ מתמלא

מחיקה דרך Agent View מנקה גם את ה-worktree. אם הגעתם למצב שיש ערוצים מיותרים, `git worktree list` בתוך הפרויקט, ואז

```bash
git worktree remove <path>
```

## לינקים רשמיים

- דוקומנטציה רשמית של Agent View: [code.claude.com/docs/en/agent-view](https://code.claude.com/docs/en/agent-view)
- Claude Code: אתר ראשי: [claude.com/product/claude-code](https://www.claude.com/product/claude-code)
- Run agents in parallel: השוואה ל-subagents ו-worktrees: [code.claude.com/docs/en/agents](https://code.claude.com/docs/en/agents)
- Agent Teams (multi-session coordination): [code.claude.com/docs/en/agent-teams](https://code.claude.com/docs/en/agent-teams)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
