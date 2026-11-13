# AGENTS.md: קובץ הוראות אחד לכל סוכני הקוד שלכם

> AGENTS.md הוא קובץ מרקדאון אחד שיושב בשורש הפרויקט ומסביר לסוכן AI איך עובדים אצלכם: איך מריצים את הפרויקט, אילו בדיקות צריך להריץ, מה סגנון הקוד ואילו קבצים אסור לגעת בהם.

מאת קווין ליאקס. פורסם 2026-09-23. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/agents-md

## מה זה AGENTS.md

AGENTS.md הוא קובץ מרקדאון אחד שיושב בשורש הפרויקט ומסביר לסוכן AI איך עובדים אצלכם: איך מריצים את הפרויקט, אילו בדיקות צריך להריץ, מה סגנון הקוד ואילו קבצים אסור לגעת בהם. באתר הרשמי מגדירים אותו בדיוק כך, README לסוכנים.

ההבדל מ-README רגיל הוא הקהל. ה-README נשאר קצר ומיועד לבני אדם, ו-AGENTS.md מחזיק את ההקשר המפורט שסוכן צריך ושבן אדם לא באמת קורא.

**זה לא פורמט של חברה אחת. הוא מנוהל היום תחת ה-Agentic AI Foundation של קרן לינוקס, נוצר בשיתוף של Codex, Amp, Jules, Cursor ו-Factory, וכבר יושב ביותר מ-60 אלף פרויקטים בגיטהאב.**

## מה השתנה בקלוד קוד

עד גרסה 2.1.277 קלוד קוד קרא רק CLAUDE.md. מי שעבד במקביל עם Codex או Cursor החזיק שני קבצים עם אותו תוכן, ובפועל אחד מהם תמיד נשאר מאחור.

מגרסה 2.1.277 קלוד קוד קורא AGENTS.md בעצמו. זה הניסוח המדויק מה-CHANGELOG הרשמי:

```
Added AGENTS.md support: in a project with no CLAUDE.md,
Claude Code reads AGENTS.md instead; change it under
"Project instructions" in /config
```

**שימו לב למילה instead. זו לא הוספה של קובץ שני, זו נפילה אחורה לקובץ אחר.**

## חוק הקדימות: מה קלוד קורא ומתי

זה החלק שרוב האנשים מפספסים. קלוד לא מאחד בין שני הקבצים, ואם שניהם קיימים, הקובץ שלו מנצח.

| מה יש לכם בפרויקט | מה קלוד קורא |
|---|---|
| **רק AGENTS.md** | את AGENTS.md שלכם |
| **גם AGENTS.md וגם CLAUDE.md** | רק את CLAUDE.md: מ-AGENTS.md הוא מתעלם |
| **CLAUDE.md שמייבא את AGENTS.md** | את CLAUDE.md, ובתוכו את AGENTS.md דרך הייבוא |

אילו קבצים נחשבים לבדיקה הזאת? כל CLAUDE.md, כל .claude/CLAUDE.md וכל CLAUDE.local.md בתיקיית העבודה או בכל תיקייה מעליה. לעומת זאת ה-CLAUDE.md האישי שלכם ב-\~/.claude, ההוראות המנוהלות של הארגון וקבצי .claude/rules ממשיכים להיטען לצד AGENTS.md ולא חוסמים אותו.

**מלכודת ששווה לזכור: CLAUDE.local.md נחשב. מספיק שיש לכם קובץ כזה להערות אישיות, וקלוד יפסיק לקרוא את AGENTS.md של הצוות.**

## התבנית: AGENTS.md מוכן להעתקה

זו התבנית שהבטחתי בסרטון. העתיקו אותה לשורש הפרויקט בשם AGENTS.md, מחקו מה שלא רלוונטי ומלאו את מה שנשאר בפקודות האמיתיות שלכם. קצר עדיף על מלא: ההמלצה הרשמית היא מתחת ל-200 שורות.

```markdown
# <project name>

## Project overview
What this repo is, in two sentences. The stack, and the entry point.

## Setup
- Package manager: pnpm (do not use npm or yarn)
- Install: pnpm install
- Dev server: pnpm dev   http://localhost:3000

## Build and test
- Build: pnpm build
- Unit tests: pnpm test
- One test file: pnpm test path/to/file.test.ts
- Lint and types: pnpm lint && pnpm typecheck
Run lint, typecheck and tests before you call a task done.

## Code style
- TypeScript, named exports only, no default exports
- 2-space indent, single quotes, no semicolons
- Components in PascalCase, hooks in useCamelCase
- Never add a comment that restates the line below it

## Project layout
- src/app      routes
- src/lib      shared logic, no UI imports
- src/ui       presentational components only
- prisma/      schema and migrations

## Do not touch
- prisma/migrations/**   generated, never edit by hand
- src/generated/**       generated, never edit by hand
- .env*                  never read, never print, never commit
- src/billing/**         ask before changing anything here

## Before you open a PR
- Branch from main, one concern per PR
- Title: <area>: <what changed, imperative>
- Update the tests that cover the code you touched
- Do not commit lockfile churn you did not intend

## Security
- No secret, token or customer data in code, logs or tests
- New dependency? Name it in the PR description and say why
```

## המעבר בשלושה צעדים

- העבירו את כל התוכן מ-CLAUDE.md לקובץ AGENTS.md בשורש הפרויקט.
- מחקו את CLAUDE.md. בלעדיו קלוד קורא את AGENTS.md לבד, וזה כל הטריק.
- פתחו סשן חדש והריצו /context. תחת Memory files אתם אמורים לראות את AGENTS.md.

בסשן אינטראקטיבי תראו גם שורה כזו:

```
no CLAUDE.md found; AGENTS.md loaded: /home/you/repo/AGENTS.md
```

## רוצים להשאיר את שני הקבצים?

יש שתי דרכים, ושתיהן עובדות. הראשונה: להשאיר CLAUDE.md עם שורת ייבוא אחת בראשו, ומתחתיה רק מה שייחודי לקלוד.

```
@AGENTS.md

## Claude Code
Use plan mode for changes under src/billing/.
```

השנייה: לשנות את ההגדרה כך שקלוד יקרא את שניהם. הקלידו /config בסשן ובחרו ערך ל-Project instructions:

| ערך | מה קלוד קורא |
|---|---|
| `claude-md-or-agents-md` | ברירת המחדל: CLAUDE.md, ורק בהיעדרו AGENTS.md |
| `claude-md-and-agents-md` | את שניהם יחד, CLAUDE.md קודם ו-AGENTS.md אחריו |
| `claude-md` | רק CLAUDE.md, כמו פעם |
| `managed-only` | רק ההוראות המנוהלות של הארגון |

אפשר גם לקבע את זה בקובץ הגדרות במקום ב-/config. שימו לב שקלוד מתעלם מההגדרה הזאת בקבצי הגדרות של הפרויקט, אז היא הולכת ל-\~/.claude/settings.json:

```json
{
  "pluginConfigs": {
    "agents-md@builtin": {
      "options": { "instructionFiles": "claude-md-and-agents-md" }
    }
  }
}
```

## מונורפו: קובץ לכל חבילה

בריפו גדול לא צריך קובץ ענק אחד. שימו AGENTS.md בתוך כל חבילה, והסוכן קורא את הקובץ הקרוב ביותר בעץ התיקיות. הריפו של OpenAI מחזיק 88 קבצים כאלה.

בקלוד קוד זה עובד כך: בתחילת הסשן נטענים כל קבצי AGENTS.md מתיקיית העבודה ומהתיקיות שמעליה, ותוך כדי עבודה נטען גם AGENTS.md של תת-תיקייה ברגע שקלוד פותח שם קובץ, בתנאי שאין באותה תת-תיקייה אף אחד משלושת קבצי CLAUDE.md.

מה לא נקרא בכלל: AGENTS.local.md, AGENTS.override.md וכל מה שיושב בתוך תיקיית .agents.

## מה לעשות אם זה לא עובד

יש מצבים שבהם קלוד קורא רק CLAUDE.md, וההגדרה Project instructions אפילו לא מופיעה ב-/config:

- אתם על גרסה ישנה מ-2.1.277. הריצו claude update.
- הסשן לא מושך דגלי פיצרים מאנתרופיק, למשל דרך Amazon Bedrock, Vertex או Foundry, או כשכיביתם טלמטריה.
- זה הסשן הראשון אחרי התקנה או שדרוג. מהסשן הבא זה כבר יעבוד.
- כיביתם את הפלאגין המובנה agents-md דרך /plugin.

בכל המקרים האלה הפתרון זהה: CLAUDE.md קטן שמכיל שורה אחת עם @AGENTS.md. הייבוא לא גורם לקלוד לקרוא את הקובץ פעמיים, בכל ערך של ההגדרה.

## טיפים

- קצר מנצח ארוך. היעד הרשמי הוא מתחת ל-200 שורות לקובץ. קובץ ארוך אוכל הקשר ודווקא מוריד את שיעור הציות.
- כתבו רק חוקים שאפשר לאמת. במקום לעצב את הקוד יפה, הזחה של שני רווחים. במקום לבדוק שינויים, הריצו pnpm test לפני קומיט.
- שני חוקים שסותרים זה את זה גורמים לסוכן לבחור אחד מהם באקראי. עברו על הקובץ מדי פעם ומחקו מה שכבר לא נכון.
- אין לכם קובץ בכלל? בקשו מהסוכן בדיוק את זה: תסרוק את הפרויקט ותכתוב AGENTS.md עם פקודות ההרצה, מוסכמות הקוד והקבצים שאסור לשנות.
- בקלוד קוד אפשר גם להריץ /init. הוא סורק את הקוד, קורא גם כללי Cursor ו-Copilot קיימים, ומייצר קובץ פתיחה.
- מי שהחזיק עד היום CLAUDE.md עם שורת @AGENTS.md בלבד יכול להשאיר אותו. הוא לא מזיק, והוא מציל אתכם בסשנים שבהם התמיכה הישירה לא זמינה.
- סימלינק במקום ייבוא עובד, אבל לא על ווינדוס: גיט מוציא שם סימלינק מקומיט כקובץ טקסט של שורה אחת, ואתם נשארים בלי הוראות.
- הוראה שהיא נוהל רב-שלבי, או שרלוונטית רק לחלק אחד מהקוד, לא שייכת לקובץ הראשי. העבירו אותה לסקיל או לכלל ממוקד-נתיב.

## לינקים

- התקן הרשמי: [agents.md](https://agents.md)
- התיעוד של קלוד קוד: [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory)
- ההכרזה של צוות קלוד קוד: [x.com/trq212](https://x.com/trq212/status/2101009392611278961)
- ה-CHANGELOG הרשמי: [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
