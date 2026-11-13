# Claude Memory System: איך לגרום ל-Claude לזכור הכל לאורך עשרות סשנים

> בכל פעם שאתם פותחים סשן חדש מול Claude, הוא מתחיל מאפס. אין לו מושג מי אתם, מה הפרויקט, מה החלטתם אתמול, ומה תיקנתם שלוש פעמים.

מאת קווין ליאקס. פורסם 2026-06-29. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/claude-memory-system

## מה זה מערכת זיכרון ל-Claude

בכל פעם שאתם פותחים סשן חדש מול Claude, הוא מתחיל מאפס. אין לו מושג מי אתם, מה הפרויקט, מה החלטתם אתמול, ומה תיקנתם שלוש פעמים. אתם חוזרים על אותו הסבר שוב ושוב.

מערכת זיכרון פותרת את זה. במקום להסביר מחדש כל בוקר, אתם בונים קבוצה קטנה של קבצי Markdown ש-Claude קורא בתחילת כל סשן ומעדכן בסופו. התוצאה: הסוכן זוכר את הלקוח, את הסטאק, את ההחלטות והתיקונים, לאורך עשרות סשנים, גם בצ'אט חדש לגמרי.

**המדריך הזה הוא הגרסה המעשית. לא תיאוריה: הקבצים האמיתיים, הפרומפטים המדויקים, ומה בדיוק לכתוב כדי שזה יעבוד אצלכם היום.**

## שני המנגנונים של Claude Code

ל-Claude Code יש שני מנגנוני זיכרון מובנים, ושניהם נטענים אוטומטית בתחילת כל סשן. חשוב להבין את ההבדל לפני שבונים:

| מאפיין | CLAUDE.md | Auto Memory |
|---|---|---|
| **מי כותב** | אתם, ביד | **Claude, לבד** |
| **מה יש בו** | הוראות, חוקים, מבנה הפרויקט | תובנות והעדפות ש-Claude לומד תוך כדי עבודה |
| **מיקום** | `CLAUDE.md בפרויקט` | `~/.claude/projects/.../memory/` |
| **נטען** | במלואו, בכל סשן | 200 שורות ראשונות מ-MEMORY.md בכל סשן |

המדריך הזה בונה מערכת מפורשת מעל `CLAUDE.md`, כי כך יש לכם שליטה מלאה על מה נשמר ואיפה. בסוף נראה גם איך לנצל את ה-Auto Memory המובנה.

## ארבעת קבצי הזיכרון

הלב של המערכת הוא ארבעה קבצים, כל אחד עם תפקיד אחד ברור. הם יושבים בתיקיית `.claude/memory/` בתוך הפרויקט שלכם:

| הקובץ | התפקיד | מה נשמר בו |
|---|---|---|
| `working.md` | **זיכרון עבודה** | מה קורה עכשיו: המשימה הנוכחית, צעדים שנשארו, באג שאתם באמצע |
| `sessions.md` | **סשנים קודמים** | יומן החלטות ותיקונים: מה נבחר, למה, ומה Claude טעה בו ותוקן |
| `facts.md` | **עובדות קבועות** | דברים שלא משתנים: הלקוח, הסטאק הטכנולוגי, העדפות קוד, פקודות build |
| `identity.md` | **זהות** | מי הסוכן, מה התפקיד שלו, מה מותר ומה אסור, איך הוא מדבר |

**ההפרדה הזו היא לא קישוט. כשכל סוג מידע יושב בקובץ נפרד, Claude מושך בתחילת סשן רק את מה שרלוונטי למשימה, ובסוף יודע בדיוק לאיזה קובץ לשמור כל פיסת מידע חדשה.**

## מבנה התיקיות בפרויקט

ככה נראה הפרויקט אחרי שמתקינים את המערכת. שימו לב: הכל קבצי טקסט רגילים, אפשר לפתוח ולערוך אותם ביד בכל רגע.

```
my-project/
├── CLAUDE.md            <- המנצח: טוען ומנהל את הכל
├── .claude/
│   └── memory/
│       ├── working.md   <- זיכרון עבודה (הסשן הנוכחי)
│       ├── sessions.md  <- החלטות ותיקונים מסשנים קודמים
│       ├── facts.md     <- עובדות קבועות (לקוח, סטאק, העדפות)
│       └── identity.md  <- זהות הסוכן
└── ... שאר הפרויקט
```

את הקובץ `CLAUDE.md` מומלץ להעלות ל-git כדי שכל הצוות יקבל אותו. את התיקייה `.claude/memory/` אפשר להעלות ל-git (כדי לשתף זיכרון) או להוסיף ל-`.gitignore` (אם הזיכרון אישי). שתי הגישות עובדות.

## הקובץ המרכזי: CLAUDE.md

זה הקובץ ש-Claude קורא אוטומטית בתחילת כל סשן, במלואו. הוא עושה שני דברים: מייבא את ארבעת קבצי הזיכרון, ומגדיר את שני הפרוטוקולים, טעינה בהתחלה ושמירה בסוף.

התחביר `@path` **הוא ייבוא אמיתי של Claude Code: כל קובץ שמסומן ב-@ נטען לתוך ההקשר בתחילת הסשן. נתיבים יחסיים מחושבים ביחס לקובץ ש-CLAUDE.md יושב בו. בפעם הראשונה Claude יבקש אישור לייבוא, מאשרים פעם אחת.**

העתיקו את התוכן הבא לקובץ `CLAUDE.md` בשורש הפרויקט:

```markdown
# Project Memory System

## Memory files (loaded every session)
@.claude/memory/identity.md
@.claude/memory/facts.md
@.claude/memory/sessions.md
@.claude/memory/working.md

## LOAD protocol - run at the START of every session
1. Read identity.md first - know who you are and the rules.
2. Read facts.md - the client, the stack, the preferences.
3. Read working.md - what was in progress.
4. Pull from sessions.md ONLY the decisions relevant
   to the current task. Do not dump the whole log.
5. Briefly confirm to me what you loaded, then start.

## SAVE protocol - run at the END of every session
When I say "save memory" (or the session is ending):
1. Review what happened this session.
2. Extract only what matters for the future.
3. Write each item to the CORRECT file:
   - new permanent fact      -> facts.md
   - a decision or a fix     -> sessions.md (dated)
   - unfinished work / TODO  -> working.md
   - a rule about my agent   -> identity.md
4. Keep each file tight. Delete stale lines in working.md.
5. Tell me in one line what you saved and where.
```

שימו לב לסדר הייבוא: `identity` ראשון, אחר כך `facts`, ואז `sessions` ו-`working`. זה הסדר שבו Claude יקרא אותם, מהיציב ביותר אל המשתנה ביותר.

טיפ: שמרו את CLAUDE.md מתחת ל-200 שורות. הוא נטען בכל סשן וצורך טוקנים, ופחות הוא יותר, דבקות גבוהה יותר בהוראות.

## תוכן מוכן-להעתקה לארבעת הקבצים

אלה תבניות התחלה. החליפו את התוכן בפרטים שלכם, אבל שמרו על המבנה, הכותרות בתוך כל קובץ עוזרות ל-Claude למקם מידע חדש במקום הנכון.

### הקובץ הראשון: identity.md

מי הסוכן, מה התפקיד, ומה הגבולות. זה הקובץ שנקרא ראשון.

```markdown
# Agent Identity

## Who you are
You are my senior engineering partner on this project.
You think before you code, and you flag risks early.

## How you work
- Ask before large refactors. Small fixes: just do them.
- Match the existing code style. Do not introduce new libs
  without asking.
- After every change: run the build and report errors.

## Hard rules
- Never commit secrets or .env files.
- Never push to main directly.
- Speak to me in Hebrew, write code/comments in English.
```

### הקובץ השני: facts.md

עובדות שלא משתנות. זה מה שחוסך לכם את ההסבר החוזר בכל בוקר.

```markdown
# Permanent Facts

## Client / Project
- Client: Acme Ltd, B2B invoicing SaaS.
- Main goal this quarter: ship the billing rewrite.

## Tech stack
- Frontend: Next.js 15, TypeScript, Tailwind v4.
- Backend: Node + Postgres (Prisma).
- Deploy: Vercel. CI on GitHub Actions.

## Commands
- Install: npm install
- Dev:     npm run dev
- Build:   npm run build   (must pass before any commit)
- Test:    npm test

## Preferences
- 2-space indentation. Named exports, not default.
- API handlers live in src/api/handlers/.
```

### הקובץ השלישי: sessions.md

יומן החלטות ותיקונים, עם תאריך. כל שורה כאן היא משהו ש-Claude לא יצטרך ללמוד פעמיים.

```markdown
# Session Log (decisions & fixes)

## 2026-06-20
- DECISION: chose Prisma over raw SQL for the billing module
  (team familiarity). Do not suggest switching.
- FIX: Kevin corrected me - dates are stored in UTC,
  convert to local only in the UI layer.

## 2026-06-22
- DECISION: invoice PDFs generated server-side, not client.
- FIX: the staging URL is staging.acme.dev, not the prod one.
```

### הקובץ הרביעי: working.md

מה קורה עכשיו. הקובץ הזה הוא היחיד שמתנקה ומתעדכן כל הזמן, מה שהסתיים נמחק.

```markdown
# Working Memory (current state)

## In progress
- Refactoring the invoice total calculation in
  src/api/handlers/invoice.ts.

## Next steps
1. Add tax rounding (banker's rounding) - half done.
2. Wire the new total into the PDF generator.
3. Write a test for the 0% tax edge case.

## Open question for Kevin
- Should credit notes use negative totals or a separate flag?
```

## שני הפרומפטים המדויקים

הפרוטוקול כבר כתוב בתוך CLAUDE.md, אבל אלה הפרומפטים שאתם מקלידים ביד כדי להפעיל אותו. העתיקו אותם כמו שהם.

### פרומפט טעינה: בתחילת כל סשן

בדרך כלל Claude טוען לבד דרך הייבוא ב-CLAUDE.md, אבל אם פתחתם צ'אט והוא לא התייחס לזיכרון, הדביקו את זה:

```
Load my memory before we start.
Read identity.md, then facts.md, then working.md.
From sessions.md pull only what's relevant to this task:
"""<תיאור המשימה של היום בשורה אחת>"""
Then tell me in 3 lines what you remember, and we begin.
```

### פרומפט שמירה: בסוף כל סשן

זה הפרומפט הכי חשוב במערכת. בלי שמירה: אין זיכרון. הריצו אותו לפני שאתם סוגרים:

```
Save memory now.
Review everything we did this session, then update the files:
- Any new permanent fact   -> facts.md
- Any decision I made or a mistake you fixed -> sessions.md
  (add today's date as a heading)
- Unfinished work and next steps -> working.md
  (and delete what's already done)
- Any new rule about how you should work -> identity.md
Keep each file tight. Then list what you saved, one line each.
```

טיפ זהב: אפשר להפוך את פקודת השמירה לקבועה. הקלידו בצ'אט `# always run the SAVE protocol when I say "save"` **וזה ייכנס לזיכרון של Claude Code עצמו. סימן ה-# הוא קיצור מובנה שמוסיף שורה ל-CLAUDE.md.**

## יום עבודה עם המערכת: צעד אחרי צעד

ככה נראה יום שלם, מהפתיחה ועד הסגירה:

- 1. פותחים את הפרויקט ומריצים claude. הקובץ CLAUDE.md נטען לבד, ואיתו ארבעת קבצי הזיכרון.
- 2. Claude מסכם לכם בכמה שורות מה הוא זוכר: הלקוח, הסטאק, ומה הייתם באמצע. אתם מאשרים ומתחילים.
- 3. עובדים כרגיל. במהלך העבודה, כשמתקבלת החלטה חשובה, אומרים לו "תרשום את זה ל-sessions".
- 4. לקראת הסוף, מריצים את פרומפט השמירה. Claude עובר על מה שקרה, ומחלק את המידע לקבצים הנכונים.
- 5. סוגרים. מחר, או בעוד שבועיים, בצ'אט חדש לגמרי, הוא ימשיך בדיוק מאיפה שעצרתם.

**הכלל היחיד שאסור לשבור: תמיד להריץ שמירה לפני שסוגרים סשן משמעותי. סשן בלי שמירה הוא סשן שאבד.**

## הקסם: סשן 1 לעומת סשן 50

זאת הדוגמה מהסרטון, עכשיו עם הקבצים האמיתיים מאחורי הקלעים.

### סשן 1: יום שני

אתם אומרים ל-Claude אגב, באמצע שיחה על קוד:

```
"by the way, our client Acme stores all dates in UTC - never local time"
```

בסוף הסשן אתם מריצים שמירה. Claude כותב ל-`facts.md` שורה אחת:

```
- All dates are stored in UTC, never local time. (Acme rule)
```

### סשן 50: חמישה שבועות אחר כך

צ'אט חדש לגמרי. מחשב אחר, אולי. אתם שואלים:

```
"how should I store the new reminder timestamps?"
```

Claude עונה מיד, בלי שחזרתם אפילו על מילה:

```
"In UTC, like all our dates - convert to local only in the UI. (per the Acme rule in facts.md)"
```

לא קסם. בתחילת הסשן Claude טען את `facts.md` **דרך הייבוא ב-CLAUDE.md, וראה את השורה שכתב לפני 49 סשנים. זה כל הסוד: וזה עובד כל עוד אתם שומרים בסוף.**

## בונוס: ה-Auto Memory המובנה

מגרסה 2.1.59 ל-Claude Code יש זיכרון אוטומטי משלו. הוא כותב לעצמו הערות תוך כדי עבודה, פקודות build, תיקונים שלמד מהתיקונים שלכם, העדפות, ושומר אותן ב-`~/.claude/projects/.../memory/MEMORY.md`.

- הוא דולק כברירת מחדל. לכבות או להדליק: דרך הפקודה /memory.
- כדי לראות מה הוא שמר ומה נטען בסשן הנוכחי, הריצו /memory, היא מציגה את כל קבצי הזיכרון הפעילים.
- אפשר לבקש ממנו ישירות: "remember that we use pnpm, not npm", והוא ישמור לעצמו.

**אז למה בכל זאת לבנות מערכת ידנית? כי המערכת הידנית נותנת לכם שליטה: אתם מחליטים בדיוק מה נשמר, באיזה קובץ, ובאיזה מבנה. ה-Auto Memory הוא משלים מצוין, תנו לו לתפוס את הקטנות, ותשמרו את ההחלטות הגדולות בקבצים שלכם.**

## טיפים מהשטח

- קובץ אחד, תפקיד אחד. אל תערבבו עובדות עם משימות פתוחות, זה מה שמבלבל את Claude בטעינה.
- working.md תמיד נקי. מה שהסתיים: נמחק. הקובץ הזה אמור לשקף רק את ההווה.
- תארכו כל רשומה ב-sessions.md. התאריך עוזר ל-Claude לדעת מה עדכני ומה ישן.
- שמרו את CLAUDE.md קצר (מתחת ל-200 שורות). הוא נטען בכל סשן, וקובץ עמוס פוגע בדבקות בהוראות.
- רשמו תיקונים, לא רק החלטות. כשתיקנתם את Claude: זה בדיוק מה שחייב להישמר כדי שלא יחזור על הטעות.
- הריצו /memory מדי פעם כדי לוודא שהקבצים באמת נטענים. אם קובץ לא מופיע ברשימה, Claude לא רואה אותו.
- אל תסמכו על הזיכרון של Claude לשמור לבד. פרומפט השמירה המפורש בסוף הוא מה שמבטיח שכלום לא הולך לאיבוד.
- סקרו את הקבצים פעם בשבוע. מחקו שורות מיותרות וסותרות: הוראות סותרות גורמות ל-Claude לבחור באקראי.

## מגבלות שכדאי להכיר

- זיכרון הוא הקשר, לא חוק ברזל. Claude קורא את הקבצים ומשתדל לציית, אבל אין ערובה לציות מוחלט. להוראה קריטית שחייבת לרוץ תמיד, השתמשו ב-hook.
- ייבוא @ מוגבל ל-4 רמות עומק. אל תבנו שרשרת ייבוא עמוקה מדי.
- קבצים שמיובאים נטענים במלואם בכל סשן וצורכים טוקנים. שמרו אותם רזים.
- ה-Auto Memory הוא מקומי למחשב ולא מסונכרן בין מכונות. המערכת הידנית, אם תעלו אותה ל-git, כן עוברת בין מכונות וחברי צוות.

## לינקים

- התיעוד הרשמי של זיכרון Claude Code: [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory)
- הורדה והתקנה של Claude Code: [claude.com/claude-code](https://www.claude.com/product/claude-code)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
