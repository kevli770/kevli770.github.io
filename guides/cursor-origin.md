# Cursor Origin: אחסון קוד ו-git בתוך Cursor, מדריך הפעלה צעד אחרי צעד

> זו השאלה המעניינת, והתשובה שהם נותנים היא שהמשחק השתנה. סוכני AI פותחים branches ודוחפים PR-ים בקצב שאף תהליך אנושי לא תוכנן אליו, וקונפליקטים ב-merge הופכים מחריגה לברירת מחדל.

מאת קווין ליאקס. פורסם 2026-08-19. נושא: סוכנים.
העמוד המלא: https://kevli770.github.io/guides/cursor-origin

## מה זה Cursor Origin

ב-17 באוגוסט 2026 Cursor השיקו את Origin, פלטפורמת אחסון קוד ו-git מלאה שיושבת בתוך Cursor עצמו. במקום שהקוד שלכם יחיה ב-GitHub וה-agent יחיה ב-Cursor, שניהם יושבים עכשיו באותו מקום.

**Origin נמצא כרגע ב-early beta ודלוק כברירת מחדל לכל מנוי בתשלום (Pro, Teams, Enterprise). היוצא מן הכלל היחיד: ארגוני Enterprise שהאדמין שלהם ביטל את זה.**

מה שיש בפנים כבר עכשיו: ריפוזיטוריז, pull requests, code review, merge, חיפוש וגלישה בקוד, סנכרון דו-כיווני מול GitHub, ואינטגרציות ל-CI ולדיפלוי.

## למה חברה שבונה עורך קוד בונה תשתית לאחסון קוד

זו השאלה המעניינת, והתשובה שהם נותנים היא שהמשחק השתנה. סוכני AI פותחים branches ודוחפים PR-ים בקצב שאף תהליך אנושי לא תוכנן אליו, וקונפליקטים ב-merge הופכים מחריגה לברירת מחדל.

במאמר הטכני שלהם, Git at any scale, הם מפרסמים את מספרי העומס שהמערכת עומדת בהם על everysphere, המונוריפו הפנימי של Cursor:

| מדד | ערך | הערה |
|---|---|---|
| **Clones לדקה** | **עד כ-296,000** | גדל באופן ליניארי עם מספר ה-replicas |
| **Pushes לדקה** | **כ-16,400** | על S3 Express One Zone |
| **Pushes לשנייה** | **120 עד 300+** | 120 על S3 Standard, מעל 300 על Express |
| **Replicas בבדיקות עומס** | **עד 100** | סקיילינג ליניארי בקריאות, בלי נסיגה בכתיבות |

*המספרים מתוך cursor.com/blog/git-at-any-scale. שימו לב ליחידות: הגרף הרשמי מודד לדקה, לא לשעה.*

## מה צריך לפני שמתחילים

- חשבון Cursor במנוי בתשלום: Pro, Teams או Enterprise
- macOS, Linux או Windows עם WSL
- git מותקן מקומית
- כדי לסנכרן ריפו קיים מ-GitHub: אפליקציית Cursor מחוברת לארגון, והרשאת admin על הריפו

## שלב 1: מתקינים את ה-Origin CLI

שורה אחת בטרמינל:

```bash
curl -fsSL https://downloads.cursor.com/origin/install.sh | sh
```

ההתקנה שמה את הבינארי ב-\~/.local/bin/origin. אם הטרמינל עונה command not found: origin, צריך להוסיף את התיקייה ל-PATH.

### zsh

```
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### bash

```
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

בודקים שזה עלה:

```
origin --version
```

**שימו לב: ה-Origin CLI (origin) הוא כלי נפרד מה-Cursor Agent CLI (agent). שניהם יכולים לחיות אצלכם במקביל.**

## שלב 2: מתחברים לחשבון

```
origin auth login
```

נפתח דפדפן, מאשרים עם החשבון שיש לו גישה ל-Origin, וזהו. ההתחברות גם מגדירה את ה-git credential helper, כך ש-git push ו-git pull מול remote של Origin עובדים אחר כך בלי שום הגדרה נוספת.

## שלב 3: מעבירים את הריפו הראשון

יש שתי דרכים, ובוחרים לפי איפה הקוד יושב היום.

### דרך א: סנכרון מ-GitHub (Mirror)

זו הדרך המומלצת אם הקוד כבר ב-GitHub. Origin מייצר עותק ומחזיק אותו מעודכן, ו-GitHub נשאר מקור האמת.

- נכנסים ל-cursor.com/codebase
- לוחצים Sync from GitHub
- בוחרים את הארגון ואת הריפו
- מאשרים: והסנכרון רץ ברקע

מה מסתנכרן ומה לא:

| נכנס ל-Origin | נשאר רק ב-GitHub |
|---|---|
| **היסטוריית git, branches ו-tags** | **GitHub Issues** |
| **קוד לגלישה וחיפוש** | **GitHub Actions workflows וסודות** |
| **Pull requests: מסתנכרנים לשני הכיוונים** |   |
| **עדכונים שוטפים, כך ש-Origin נשאר טרי** |   |

אפשר לוודא את מצב הסנכרון בכל רגע תחת Settings של הריפו, בטאב General. ריפו מסונכרן מציג את Origin כ-mirror ואת GitHub כ-source.

### דרך ב: דחיפה של פרויקט מקומי

אם הפרויקט יושב רק אצלכם במחשב, יוצרים ריפו ודוחפים אליו ישירות.

יוצרים את הריפו מהטרמינל:

```
origin repo create my-project
```

בלי slash: הריפו נוצר תחת ה-namespace של החשבון שלכם. אפשר גם ליצור אותו מה-UI: נכנסים ל-Codebase, לוחצים New, ונותנים שם.

ואז, מתוך תיקיית הפרויקט:

```bash
git init -b main
git remote add origin https://origin.cursor.com/<org>/my-project
git add .
git commit -m "Initial commit"
git push -u origin main
```

לשכפול של ריפו שכבר קיים ב-Origin:

```
origin repo clone <org>/my-project
# or straight with git
git clone https://origin.cursor.com/<org>/my-project
```

*השם שאתם נותנים ל-codebase הופך לחלק מה-URL של כל ריפו: cursor.com/codebase/\<השם-שלכם\>*

## שלב 4: עובדים עם Pull Requests

דוחפים branch ופותחים PR מול ה-base branch:

```bash
git checkout -b my-change
git push -u origin my-change
```

אחרי שה-branch נמצא ב-Origin, פותחים PR מהטאב Code של הריפו. לכל pull request יש ארבעה טאבים:

| טאב | מה יש בו |
|---|---|
| **Activity** | פתיחות, תגובות, reviews ושינויי סטטוס לפי סדר |
| **Commits** | הקומיטים שב-PR, אפשר לפתוח כל אחד ולראות את ה-diff שלו |
| **Checks** | ריצות ה-CI והסטטוס של ה-branch |
| **Files Changed** | ה-diff המלא, כולל תגובות על שורות ספציפיות |

אפשר לבקש reviewers, להשאיר review, ולעשות merge כשה-reviews וה-CI מרוצים. Origin מציף קונפליקטים ב-merge לפני שאתם ממזגים, לא אחרי.

**על ריפו מסונכרן מ-GitHub: תגובה שכותבים ב-Cursor מופיעה ב-GitHub, ותגובה או review מ-GitHub מופיעים ב-Cursor תוך שניות. גם review שהוקצה לכם ב-GitHub, אפשר לאשר ולמזג אותו מתוך Cursor.**

## סוכנים ואוטומציות על הריפו

זו הנקודה שבגללה כל זה נבנה. הקוד, ה-PR-ים והסוכנים יושבים באותו מקום, אז אפשר לשאול את Cursor שאלות על קוד שאתם גולשים בו, והוא יכול לענות, לשנות, לעדכן PR או לדחוף branch.

### Automations

אוטומציות מריצות cloud agents לפי לוח זמנים או כשקורה אירוע ב-source control. מכוונים אוטומציה לריפו של Origin בדיוק כמו לריפו מחובר של GitHub או GitLab.

- Push to branch: למשל push ל-main
- Pull request opened או Pull request pushed

יוצרים אותן מ-cursor.com/automations, מחלון ה-Agents, או עם ה-skill בשם automate.

### Cloud agents

סוכני ענן יכולים לעבוד ישירות מול ריפו של Origin: clone, branch, commit, push ופתיחת pull request.

## אינטגרציות: CI ודיפלוי

Origin נבנה עם אקוסיסטם של אפליקציות. מחברים אותן מהטאב Apps של הריפו.

| אינטגרציה | מה היא נותנת |
|---|---|
| **Vercel** | כל PR מקבל preview deployment לבדיקה ולתגובות. אחרי merge זה עולה ל-production |
| **Depot** | CI: מריץ את ה-GitHub Actions workflows הקיימים שלכם |
| **Buildkite** | CI: מריץ גם GitHub Actions וגם pipelines טבעיים של Buildkite |

## טיפים

- התחילו מריפו אחד לא קריטי. זה early beta, ואין סיבה להעביר את המונוריפו של הפרודקשן ביום הראשון
- אם הקוד כבר ב-GitHub, העדיפו Mirror על פני העברה מלאה, GitHub נשאר מקור האמת ואתם לא מסתכנים בכלום
- אחרי origin auth login לא צריך להגדיר credentials ל-git בנפרד, ה-CLI כבר עשה את זה בשבילכם
- האייקונים ליד שם הריפו ברשימה מראים מה מאוחסן ב-Cursor ומה הגיע מ-GitHub, שווה להסתכל לפני שדוחפים
- Issues ו-Actions לא עוברים. אם אתם בונים תהליך סביב Origin, תכננו מראש איפה הם יחיו
- בטאב Settings, תחת Danger Zone, יש Detach from GitHub. זה הופך את Origin למקור האמת, פעולה שכדאי לעשות רק כשאתם בטוחים
- אפשר לבקש מהסוכן להעלות פרויקט ל-Origin והוא יתקין את ה-CLI, יתחבר, ייצור את הריפו, יגדיר remote וידחוף, הכל לבד

## מגבלות שכדאי להכיר

- Early beta: הפיצ'רים והמסכים משתנים. ה-UI של Permissions ושל Rules and Protections בעיצוב מחדש
- רק במנוי בתשלום. ארגוני Enterprise יכולים לבטל את זה ברמת האדמין
- GitHub Issues לא עוברים
- GitHub Actions workflows וסודות לא עוברים, צריך Depot או Buildkite כדי להריץ אותם
- בריפו מסונכרן, push דרך Origin עובר ל-GitHub. GitHub נשאר מקור האמת עד שמנתקים ידנית

## לינקים

- ההכרזה הרשמית: [cursor.com/changelog](https://cursor.com/changelog)
- הדוקומנטציה של Origin: [cursor.com/docs/origin](https://cursor.com/docs/origin)
- התקנת ה-CLI: [cursor.com/docs/origin/cli](https://cursor.com/docs/origin/cli)
- סנכרון מ-GitHub: [cursor.com/docs/origin/mirror-github](https://cursor.com/docs/origin/mirror-github)
- המאמר הטכני עם המספרים: [cursor.com/blog/git-at-any-scale](https://cursor.com/blog/git-at-any-scale)
- הריפוזיטוריז שלכם: [cursor.com/codebase](https://cursor.com/codebase)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
