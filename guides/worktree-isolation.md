# Worktree Isolation: להריץ כמה Claude Code agents במקביל בלי שידרסו אחד את השני

> Worktree Isolation זה פיצ'ר רשמי של Claude Code שמאפשר להריץ כמה agents במקביל באותו ה-repo בלי שהם דורסים אחד לשני את הקבצים.

מאת קווין ליאקס. פורסם 2026-05-23. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/worktree-isolation

## מה זה Worktree Isolation

Worktree Isolation זה פיצ'ר רשמי של Claude Code שמאפשר להריץ **כמה agents במקביל** באותו ה-repo בלי שהם דורסים אחד לשני את הקבצים. מאחורי הקלעים זה עוטף את ה-

`git worktree` הוותיק (קיים בגיט מאז 2015), אבל Claude Code עושה את כל העבודה השחורה אוטומטית, יוצר את התיקייה, יוצר branch חדש, נכנס לסשן בתוכה, וגם מנקה אחרי שסיימת.

הפיצ'ר עברה ליטוש משמעותי בגרסאות 2.1.126-2.1.144 של Claude Code (אפריל, מאי 2026), כולל `.worktreeinclude` להעתקת קבצי `.env` ו-hooks של `WorktreeCreate` / `WorktreeRemove`.

## הבעיה שזה פותר

בלי isolation, שני סוכנים שעובדים על אותה תיקייה זה תאונת דרכים מובטחת:

- סוכן A פותח את src/api.ts, מתחיל לערוך.
- סוכן B פותח באותו רגע את אותו קובץ, עורך משהו אחר.
- שניהם שומרים: האחרון מנצח. העבודה של הראשון נמחקת בשקט.
- שניהם נלחמים על אותו HEAD ועל אותו index של git.

הנזק לא מוצא את עצמו בלוג שגיאות. אתה פשוט מגלה ש-feature שלם נעלם, וצריך לעשות `git reflog` כדי לשחזר.

## הפקודה: בגרסה הקצרה

פותחים טרמינל בתיקיית ה-repo ומריצים:

```bash
claude --worktree feature-auth
```

זהו. Claude יוצר עבורכם:

- תיקייה חדשה ב-`.claude/worktrees/feature-auth/`
- branch חדש בשם `worktree-feature-auth`
- סשן חדש של Claude בתוך התיקייה החדשה, מבודד לחלוטין מהסשנים האחרים.

### 3 דוגמאות שכדאי להכיר

עם שם מותאם (מומלץ: קל לזכור על מה כל סוכן עובד):

```bash
claude --worktree bugfix-123
```

בלי שם: Claude ייצר שם רנדומלי קריא כמו `bright-running-fox`:

```bash
claude --worktree
```

ישירות מ-Pull Request: מבסיס ה-branch של ה-PR:

```bash
claude --worktree "#1234"
```

יש גם flag קצר: `-w` עושה אותו דבר כמו `--worktree`.

## שלב אחר שלב: להפעיל את זה אצלכם

### שלב 1: trust ראשוני

פעם ראשונה בתיקייה: חייבים פשוט להריץ `claude` ולאשר את ה-workspace trust dialog. אחר כך אפשר להשתמש ב-`--worktree`.

```bash
cd ~/projects/my-app
claude
```

(כשמופיע המסך הראשון, מאשרים. סוגרים את הסשן.)

### שלב 2: להוסיף ל-.gitignore

אל תשכחו להוסיף את התיקייה ל-`.gitignore` כדי שתיקיות ה-worktree של Claude לא יופיעו כקבצים לא מסומנים בתוך ה-repo הראשי שלכם:

```
.claude/worktrees/
```

### שלב 3: להגדיר .worktreeinclude לקבצים פרטיים

worktree חדש זה checkout נקי. זה אומר שכל קובץ שנמצא ב-`.gitignore`, כמו `.env`, `.env.local`, או קונפיגים פרטיים, לא יהיה שם. הסוכן ינסה להריץ ויקבל שגיאות.

הפתרון: יוצרים קובץ `.worktreeinclude` בשורש ה-repo עם הקבצים שאתם רוצים להעתיק (תחביר זהה ל-.gitignore):

```
.env
.env.local
.env.development
config/local.json
```

חשוב: רק קבצים שגם ב-.gitignore וגם ב-.worktreeinclude יועתקו. זה מנגנון הגנה כדי שלא תעתיקו בטעות קוד פרטי לכל worktree.

### שלב 4: להריץ את הסוכן הראשון

```bash
claude --worktree feature-auth
```

Claude קופץ ישר לסשן חדש בתוך התיקייה החדשה. עובד רגיל: מתאר משימה, הסוכן יוצר commits ל-branch שלו.

### שלב 5: שני סוכנים במקביל

בטרמינל שני (חלון נפרד, לא tab באותו טרמינל), בתוך תיקיית ה-repo הראשי:

```bash
claude --worktree bugfix-payment
```

כל סוכן רואה רק את ההעתק שלו, ב-branch משלו. אפס התנגשויות.

### שלב 6: התקנת תלויות בכל worktree

worktree חדש זה תיקייה חדשה, אז אין שם `node_modules` / `.venv` / מה שלא יהיה. אם הסוכן צריך להריץ קוד, הוא חייב להתקין קודם:

```bash
npm install
# או
uv pip install -e .
```

### שלב 7: סקירה ומיזוג

שהסוכן סיים, חוזרים ל-repo הראשי ובודקים את ה-diff:

```bash
git diff main...worktree-feature-auth
```

טוב? ממזגים:

```bash
git merge worktree-feature-auth
```

רע? זורקים:

```bash
git worktree remove .claude/worktrees/feature-auth
git branch -D worktree-feature-auth
```

## טבלת דגלים ופקודות

| פקודה | מה היא עושה |
|---|---|
| `claude --worktree <name>` | יוצר worktree בשם הנתון, branch worktree-\<name\>, ופותח סשן בתוכו. |
| `claude -w <name>` | זהה לקודם, רק קצר יותר. |
| `claude --worktree "#1234"` | יוצר worktree מבוסס ה-branch של Pull Request #1234. |
| `claude --worktree` | שם אוטומטי כמו bright-running-fox: שימושי לסשנים מהירים שלא תזכרו את השם שלהם בכל מקרה. |
| `git worktree list` | מציג את כל ה-worktrees הפעילים ב-repo, איזה branch בכל אחד, איפה הם, ומה התקיעות שלהם. |
| `git worktree remove <path>` | מוחק worktree. עובד רק אם נקי: להוסיף --force אם יש שינויים לא ממוזגים. |

## הגדרות מתקדמות

### worktree.baseRef

כברירת מחדל Claude יוצר את ה-worktree מ-`origin/HEAD` (כלומר נקי מהשרת). אם אתם רוצים שזה יסתעף מה-HEAD המקומי שלכם (כדי לגרור איתכם שינויים שעוד לא דחפתם), הוסיפו לקובץ ה-settings:

```json
{
  "worktree": {
    "baseRef": "head"
  }
}
```

### Hooks: WorktreeCreate ו-WorktreeRemove

Claude מציע 2 hooks שרצים אוטומטית כשנוצר או נמחק worktree. שימושי להתקין תלויות אוטומטית, להעתיק קבצים מורכבים שדורשים סקריפט, או לשגר התראה ל-Slack. דוגמה ל-WorktreeCreate שמתקין dependencies:

```json
{
  "hooks": {
    "WorktreeCreate": "npm install"
  }
}
```

גם זה נכנס לאותו קובץ `~/.claude/settings.json` או `.claude/settings.json` של הפרויקט.

## דוגמה מהחיים

אתם בונים feature חדש של אימות משתמשים. במקביל יש באג קריטי בפרודקשן שצריך לתקן. שני דברים שונים, ועדיף שלא יתנגשו:

### טרמינל 1: feature חדש

```bash
cd ~/projects/saas-app
claude --worktree auth-magic-link
```

בתוך הסשן: "תוסיף ל-API endpoint של login תמיכה ב-magic link, כולל טבלת tokens במיגרציה."

### טרמינל 2: תיקון פרודקשן

```bash
cd ~/projects/saas-app
claude --worktree hotfix-payment-timeout
```

בתוך הסשן: "ב-Stripe webhook handler, להוסיף retry עם backoff על timeout בקריאה ל-DB."

### אחרי שעה

רואים מה כל אחד עשה:

```bash
git worktree list
```

מקבלים פלט כזה:

```
/Users/me/projects/saas-app                    abc1234 [main]
/Users/me/projects/saas-app/.claude/worktrees/auth-magic-link    def5678 [worktree-auth-magic-link]
/Users/me/projects/saas-app/.claude/worktrees/hotfix-payment-timeout    9012abc [worktree-hotfix-payment-timeout]
```

ה-hotfix דוחף לפרודקשן מיד. ה-feature ממשיך עד שאתם מרוצים. אפס התנגשויות, אפס "מי דרס למי".

## טיפים מהשטח

- שמות תיאוריים: לא agent-1 ו-agent-2 אלא feature-auth, fix-stripe-timeout. עוד שבוע לא תזכרו מה היה ב-agent-1.
- תמיד טרמינל נפרד לכל סוכן, לא tab. tabs באותו טרמינל לפעמים חולקים environment variables ובלגנים.
- אחרי שמיזגתם, מנקים את ה-worktree באותו רגע. הם תופסים מקום ונערמים מהר.
- אם עובדים עם monorepo, .worktreeinclude חייב לכלול גם קונפיגים של ה-workspace (lerna, nx, turbo), לא רק .env.
- ה-baseRef ברירת המחדל זה origin/HEAD. אם אתם מסתעפים מ-feature branch שלכם שעוד לא דחוף, החליפו ל-head.
- ה-EnterWorktree tool עובד גם בתוך סשן קיים. אם התחלתם בסשן רגיל וגיליתם שצריך isolation, פשוט תגידו ל-Claude "transfer this to a worktree".
- סשנים לא אינטראקטיביים (claude -p) לא מנקים את ה-worktree אוטומטית. לזכור להריץ git worktree remove ידנית.

## מה זה לא פותר

- תלויות (node\_modules, .venv): יש להתקין מחדש בכל worktree, או להגדיר WorktreeCreate hook.
- תהליכים שמאזינים לאותו port: אם שני סוכנים מריצים npm run dev על פורט 3000, רק אחד יצליח.
- DB משותף: שני סוכנים שעושים מיגרציות על אותו DB מקומי יתנגשו. השתמשו ב-DB לכל worktree או ב-snapshots.
- תהליכי build משותפים (cache של webpack/vite ב-/tmp), לפעמים יוצרים race conditions עדינים.
- Mac/Linux בלבד: Windows native לא נתמך מלא, אבל WSL2 עובד מצוין.

## לינקים

- דף הפיצ'ר הרשמי של Claude Code: [code.claude.com/docs/en/worktrees](https://code.claude.com/docs/en/worktrees)
- Common workflows: הקטע על parallel sessions: [code.claude.com/docs/en/common-workflows](https://code.claude.com/docs/en/common-workflows#run-parallel-sessions-with-worktrees)
- Hooks (WorktreeCreate / WorktreeRemove): [code.claude.com/docs/en/hooks](https://code.claude.com/docs/en/hooks#worktreecreate)
- Settings (worktree.baseRef): [code.claude.com/docs/en/settings](https://code.claude.com/docs/en/settings#worktree-settings)
- git worktree (התשתית מתחת): [git-scm.com/docs/git-worktree](https://git-scm.com/docs/git-worktree)
- Claude Code CHANGELOG (כדי לראות אילו refinements נכנסו בכל גרסה): [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
