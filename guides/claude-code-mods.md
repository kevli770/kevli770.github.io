# Claude Code Mods: שלושת המודים מהסרטון ועוד שבעה, קישורים, למי כל אחד מתאים ומה לבדוק לפני התקנה

> מוד הוא תוסף שמשנה חלק מהממשק או מההתנהגות בתוך Claude Code. קצת כמו תוסף לדפדפן, רק בתוך סביבת העבודה שבה אתם בונים: הוא יכול לצייר שורה מעל שורת הפקודה, להוסיף פקודות סלאש ולעצור פעולה לפני שהיא רצה.

מאת קווין ליאקס. פורסם 2026-11-09. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/claude-code-mods

## מה זה מוד

מוד הוא תוסף שמשנה חלק מהממשק או מההתנהגות בתוך Claude Code. קצת כמו תוסף לדפדפן, רק בתוך סביבת העבודה שבה אתם בונים: הוא יכול לצייר שורה מעל שורת הפקודה, להוסיף פקודות סלאש ולעצור פעולה לפני שהיא רצה.

זה לא תוסף לצ'אט הרגיל באתר Claude. מודים דורשים Claude Code בגרסה 2.1.287 ומעלה, ו-Anthropic פותחת אותם לחשבונות בהדרגה. בדיקה מהירה: הריצו claude plugin test בתיקייה כלשהי. אם כתוב served off, המודים עוד לא פתוחים אצלכם.

**כלל אחד לפני הכול: מוד הוא קוד של מישהו אחר שרץ בהרשאות שלכם. מתקינים קודם בפרויקט דמה, ובודקים מי כתב אותו ומה הוא יכול לעשות.**

## שלושת המודים מהסרטון

### 1. Token Weather: כמה מקום נשאר בשיחה

מציג אחרי כל סבב כמה מחלון הקונטקסט תפוס, בשורה אחת מעל שורת הפקודה: אייקון מזג אוויר, אחוז, וכמות הטוקנים מתוך החלון.

| תפוסה | מה מוצג |
|---|---|
| **מתחת ל-25%** | Clear |
| **25-49%** | Cloudy |
| **50-74%** | Showers |
| **75-89%** | Storm |
| **90% ומעלה** | Compact soon |

חשוב לדעת: האחוז מחושב מתוך החלון המלא, ולכן הוא לא זהה לסף הדחיסה האוטומטית של Claude Code. זה לא מד שאומר "בעוד דקה הוא ישכח". זה סימן שכדאי לעצור ולתכנן מעבר לשיחה חדשה.

**מה עושים כשמתקרבים לגבול:**

בקשו מ-Claude: "שמור בקובץ סיכום את המטרה, ההחלטות, הקבצים ששינינו, מה לא לשנות והצעד הבא. אל תכלול סודות." קראו את הסיכום, ורק אז פתחו שיחה חדשה שמתחילה ממנו. ככה לא מתחילים מאפס.

**התקנה:**

```bash
git clone https://github.com/anthropics/claude-code-playground.git
cd claude-code-playground/claude-code/mods
claude plugin validate ./token-weather
claude plugin marketplace add ./
claude plugin install token-weather@claude-code-playground-mods --scope user
```

אחר כך פותחים שיחה חדשה. Token Weather הוא דוגמה מתוך מאגר ה-playground של Anthropic, לא מוצר רשמי נתמך.

מקור: [README של Token Weather](https://github.com/anthropics/claude-code-playground/blob/main/claude-code/mods/token-weather/README.md)

### 2. Cache Keeper: מתי משתמשים שוב בעיבוד שכבר נעשה

כשתחילת השיחה נשארת זהה, המערכת יכולה להשתמש בעיבוד שכבר נעשה במקום לעבד הכול מחדש. לזה קוראים prompt cache, מטמון. לפי ה-README, קריאה מהמטמון עולה עשירית ממחיר קלט רגיל, וכשהמטמון פג (אחרי 5 דקות או שעה, לפי ההגדרה) ההודעה הבאה כותבת את כל השיחה מחדש.

Cache Keeper מציג מעל שורת הפקודה שורה כזאת:

```
● cache warm 42m │ ctx 412k │ rewrite ≈ $3.30 │ session $5.12
```

ומוסיף פקודות:

| `/cache` | מצב המטמון וההגדרות |
|---|---|
| `/board` | כל הצ'אטים המקומיים בחלונית אחת |
| `/keepwarm` | שומר את המטמון חם עם בקשות קטנות |
| `/handoff` | שומר סיכום ומעביר אותו לצ'אט חדש |

**שלוש הבחנות שחשוב לא לבלבל:**

- מטמון הוא לא קונטקסט. הוא לא מוסיף זיכרון ל-Claude. מטמון שפג לא מוחק את השיחה.
- keepwarm שולח בקשות למודל, וגם הן צורכות שימוש. לא מפעילים אותו רק כי כתוב חיסכון.
- במנוי חודשי, אומדן הדולרים הוא לא החשבונית שלכם. הוא מופיע כשימוש מול המגבלות.

איך מתחילים: מעקב בלבד, עם הפקודות cache ו-board. אם רוצים לבדוק את keepwarm, מודדים שימוש כולל במשימת דמה עם ובלי, ומכבים כשאין צורך.

### 3. Recording Mode: מה נחשף כשאתם מקליטים מסך

המוד ממסך בתצוגה מפתחות API, ערכים מקובצי env, מיילים, שמות, טלפונים וסכומים, ונועל פתיחה של קבצים רגישים כמו קובצי env, מפתחות ומסמכי כספים. נורה אדומה REC מופיעה כל עוד הוא פעיל.

| `/rec` | מפעילים לפני שלוחצים הקלטה |
|---|---|
| `/rec strict` | ממסך גם כל מספר ואחוז גדולים |
| `/rec off` | מכבה בכל הצ'אטים |
| `/rec config` | שמות שתמיד יוסתרו, השם שלכם שיישאר גלוי, ותיקיות שאסור לפתוח |

**המגבלות, במילים של ה-README:**

- הוא משנה מה מוצג על המסך ומה מותר לפתוח, לא מה נשמר. התמליל המקורי לא נמחק.
- מיסוך לפי תבניות לא תופס הכול, במיוחד פרטים שנכתבו במילים חופשיות.
- כותרות השיחות בסרגל הצד של אפליקציית Desktop לא ממוסכות.

איך בודקים: פרויקט דמה ונתוני דמה בלבד. מקליטים עשר שניות, צופים בהקלטה ובודקים גם את הסרגל הצדדי, ורק אז מפרסמים. לא מכניסים סוד אמיתי כדי "לבדוק שההסתרה עובדת".

**התקנת Cache Keeper ו-Recording Mode (שניהם של Nate Herk):**

```bash
claude plugin marketplace add nateherkai/claude-code-mods
claude plugin install cache-keeper@nateherk-mods
claude plugin install recording-mode@nateherk-mods
```

פותחים שיחה חדשה. מוד נטען כשהצ'אט מתחיל, וטעינה מחדש של פלאגינים באמצע שיחה לא טוענת אותו.

מקור: [README של claude-code-mods](https://github.com/nateherkai/claude-code-mods)

## עוד שבעה: בוחרים לפי צורך

לא מתקינים הכול. כל אחד מהם פותר בעיה אחרת, וחלקם מתחרים על אותו אזור במסך. אחד בכל פעם.

| מוד | למי זה מתאים | מה לא להניח | מקור |
|---|---|---|---|
| **Goal Meter** | מי שעובד עם פקודת goal ורוצה לראות התקדמות וזמן משוער | האחוז לא מוכיח שהפתרון נכון | [Nate Herk](https://github.com/nateherkai/claude-code-mods) |
| **Filetree** | מי שרוצה לראות אילו קבצים נקראים ונערכים | תצוגת קבצים היא לא code review | [data-goblin](https://github.com/data-goblin/claude-code-filetree) |
| **Claude Skins** | מי שרוצה להתאים את מראה סביבת העבודה | צבעים לא משפרים את המודל | [hellosverre](https://github.com/hellosverre/claude-skins) |
| **Terminal Browser** | מי שרוצה דפדפן ליד העבודה בטרמינל | דורש דפדפן וטרמינל תואמים | [zenbu-labs](https://github.com/zenbu-labs/terminal-browser/blob/main/claude-code-plugin/README.md) |
| **Replay Theater** | מי שרוצה לעבור על העריכות של הסבב האחרון | זו לא היסטוריית Git | [Anthropic](https://github.com/anthropics/claude-code-playground/blob/main/claude-code/mods/replay-theater/README.md) |
| **Collision Guard** | מי שמריץ כמה צ'אטים על אותו פרויקט | לא מונע כל התנגשות | [Nate Herk](https://github.com/nateherkai/claude-code-mods) |
| **Ribbon** | מי שרוצה לראות שימוש ומועדי איפוס | אומדנים הם לא חשבונית | [robonuggets](https://github.com/robonuggets/ribbon) |

### התקנות נוספות: אחת בכל פעם

**Goal Meter ו-Collision Guard (אחרי שהוספתם את ה-marketplace של Nate):**

```bash
claude plugin install goal-meter@nateherk-mods
claude plugin install collision-guard@nateherk-mods
```

פותחים שיחה חדשה ומריצים (Goal Meter צריך מטרה פעילה):

```
/goals    /guard
```

**Filetree (לפי המקור: Claude Code 2.1.306 ומעלה, ו-Git):**

```bash
claude plugin marketplace add data-goblin/claude-code-filetree
claude plugin install filetree@claude-code-filetree
```

**Claude Skins (מתוך Claude Code):**

```
/plugin marketplace add hellosverre/claude-skins
/plugin install skins@hellosverre-mods
```

הפעלה מחדש, ואז:

```
/skin
```

**Terminal Browser (רק אחרי שעמדתם בדרישות ה-README):**

```bash
claude plugin marketplace add zenbu-labs/terminal-browser
claude plugin install terminal-browser@terminal-browser
```

שיחה חדשה, ואז (ייתכן שיידרשו הגדרות נוספות):

```
/browser
```

**Replay Theater (מתוך תיקיית ה-mods ששכפלתם בשביל Token Weather):**

```bash
claude plugin install replay-theater@claude-code-playground-mods --scope user
```

שיחה חדשה, שינוי בקובץ דמה, ואז:

```
/replay
```

**Ribbon (מתוך Claude Code):**

```
/plugin install ribbon --marketplace robonuggets/ribbon
```

מאשרים את המקור רק אחרי שבדקתם אותו, ופותחים שיחה חדשה.

## מה לבדוק לפני כל התקנה

- המקור: מי כתב את המוד, ושהקישור הוא לריפו של היוצר עצמו.
- ה-README: מה המוד עושה, מה הוא שולח, ומה המגבלות שהיוצר עצמו כותב.
- הרשאות: לאילו קבצים, משתני סביבה וכתובות רשת הוא ניגש.
- התאמה לגרסה: claude --version מול הדרישה ב-README.
- פרויקט דמה קודם. לא נותנים למוד לא מוכר גישה לפרויקט אמיתי בלי להבין מה הוא עושה.

**פרומפט להעתקה, לפני שמתקינים:**

"הנה הקישור למוד. תעבור על הקוד וה-README ותגיד לי: מה הוא עושה, לאילו קבצים, משתני סביבה וכתובות רשת הוא ניגש, אם הוא מתאים לגרסה שלי, ומה הסיכון. אל תתקין כלום."

מוד הסתרה הוא לא תחליף לניהול סודות, ואף אחד מהמודים כאן לא מבטיח חיסכון, זמינות בחשבון או תאימות שלא בדקתם בעצמכם.

## לינקים

- Token Weather: [github.com/anthropics/claude-code-playground](https://github.com/anthropics/claude-code-playground/blob/main/claude-code/mods/token-weather/README.md)
- Cache Keeper, Recording Mode, Goal Meter, Collision Guard: [github.com/nateherkai/claude-code-mods](https://github.com/nateherkai/claude-code-mods)
- תיעוד המודים: [code.claude.com/docs/en/plugins/mods](https://code.claude.com/docs/en/plugins/mods/overview)
- Prompt caching: [platform.claude.com](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
