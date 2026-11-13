# Claude Code ELI5 Skill: הסקיל שהצוות של אנתרופיק משתמש בו הכי הרבה, וכל הקובץ מאחוריו הוא שלוש שורות

> ELI5 הוא סקיל ל-Claude Code. השם הוא ראשי תיבות של Explain Like I'm 5, תסביר לי כאילו אני בן חמש.

מאת קווין ליאקס. פורסם 2026-08-22. נושא: Claude ו-Claude Code.
העמוד המלא: https://kevli770.github.io/guides/claude-eli5-skill

## מה זה ELI5

ELI5 הוא סקיל ל-Claude Code. השם הוא ראשי תיבות של **Explain Like I'm 5**, תסביר לי כאילו אני בן חמש.

במקום שקלוד יחזיר לכם קיר של טקסט טכני, הוא בונה עמוד HTML שלם עם ציורים גדולים ומעט מאוד מילים. אתם מקבלים הסבר ויזואלי לכל נושא שלא הבנתם, בתוך הצ'אט.

### מי בנה את זה

הסקיל פורסם על ידי **Thariq Shihipar**, מהנדס בצוות של Claude Code באנתרופיק. לטענתו זה הסקיל שהכי משתמשים בו אצלם בפנים.

החלק המעניין: הוא לא מגיע מותקן ואתם לא תמצאו אותו בתפריט הרגיל של Claude Code. הוא יושב במרקטפלייס הקהילתי של אנתרופיק, ונכון לכתיבת המדריך הזה הוא עדיין לא הוסב לפלאגין רשמי.

![המרקטפלייס הקהילתי של אנתרופיק, ובתוכו התיקייה של הסקיל](https://kevli770.github.io/_astro/01.BVYQmORS.png)

*המרקטפלייס הקהילתי של אנתרופיק, ובתוכו התיקייה של הסקיל*

## מה הוא עושה בפועל

ההבדל הוא לא באיכות התשובה אלא בצורה שלה. זו טבלה שמסכמת את זה:

| בלי הסקיל | עם /eli5 |
|---|---|
| פסקאות ארוכות של טקסט | עמוד HTML עם ציורים גדולים |
| מונחים טכניים ברמת מהנדס | שפה של ילד בן חמש |
| צריך לקרוא הכל כדי להבין | מבינים במבט אחד |
| קשה להראות למישהו אחר | Artifact שאפשר לשתף |

ככה נראה פלט אמיתי של הסקיל על השאלה how does DNS work:

![עמוד אחד, אייקונים גדולים, חצים, ומעט מילים. זה כל מה שהסקיל מייצר](https://kevli770.github.io/_astro/02.BY2pu0Ut.png)

*עמוד אחד, אייקונים גדולים, חצים, ומעט מילים. זה כל מה שהסקיל מייצר*

## הקובץ המלא: שלוש שורות

זה החלק שהכי מפתיע אנשים. כל הסקיל הוא קובץ אחד בשם `SKILL.md` שיש בו עשר שורות סך הכל, מתוכן שלוש שורות תוכן אמיתי. זה הקובץ במלואו:

```markdown
---
name: eli5
description: Explain a topic like I'm a 5 year old. Use when the user
  types /eli5 <topic> or asks for a dead-simple picture explainer of
  how something works.
---

# eli5

Explain like I'm someone who knows nothing about this topic, using a
HTML artifact with big pictures and few words.

Topic: $ARGUMENTS
```

![הקובץ כפי שהוא נראה בגיטהאב, עשר שורות סך הכל, מתוכן שלוש שורות תוכן](https://kevli770.github.io/_astro/03.BrKNQ4d2.png)

*הקובץ כפי שהוא נראה בגיטהאב, עשר שורות סך הכל, מתוכן שלוש שורות תוכן*

### איך לקרוא את הקובץ

- השורות בין שני קווי ה--- נקראות frontmatter. הן המטא-דאטה של הסקיל
- name הוא שם הסקיל, וממנו נגזרת הפקודה שתקלידו
- description היא מה שקלוד קורא כדי להחליט מתי להפעיל את הסקיל לבד, בלי שתבקשו
- הגוף של הקובץ הוא ההוראה עצמה. זו שורה אחת באנגלית פשוטה
- $ARGUMENTS הוא מציין מיקום. כל מה שתכתבו אחרי הפקודה נכנס לשם

## התקנה: שתי פקודות

ההתקנה היא בשני שלבים: קודם מוסיפים את המרקטפלייס הקהילתי, ואז מתקינים ממנו את הסקיל. יש שתי דרכים לעשות את זה, בחרו אחת.

### דרך א: מתוך Claude Code

פותחים את Claude Code ומקלידים את שתי הפקודות האלה, אחת אחרי השנייה:

```
/plugin marketplace add anthropics/claude-plugins-community
/plugin install eli5@claude-community
```

שימו לב ש-`claude-community` הוא שם המרקטפלייס, והוא שונה משם הריפו בגיטהאב. אל תחליפו ביניהם.

### דרך ב: מהטרמינל

אם אתם מעדיפים לא לפתוח סשן, אותן שתי פקודות עובדות גם ישירות מהטרמינל:

```bash
claude plugin marketplace add anthropics/claude-plugins-community
claude plugin install eli5@claude-community
```

ההתקנה מהטרמינל נכנסת לתוקף בפעם הבאה שתפעילו את Claude Code.

### לוודא שזה נכנס

אחרי ההתקנה, אם אתם בתוך סשן פתוח, הריצו:

```
/reload-plugins
```

ואז תבדקו שהסקיל מופיע ברשימה:

```
/plugin list
```

## איך משתמשים

כותבים סלאש, את שם הסקיל, ואחריו כל דבר שלא הבנתם:

```
/eli5 <topic>
```

*במקום topic כותבים את הנושא עצמו, בעברית או באנגלית.*

אם הפקודה הקצרה לא מופיעה בהשלמה האוטומטית, השתמשו בצורה המלאה עם שם הפלאגין:

```
/eli5:eli5 <topic>
```

קלוד יחזיר Artifact של HTML. אפשר לפתוח אותו, לשמור אותו, ולשלוח אותו למישהו אחר.

## שלוש דוגמאות לנסות עכשיו

הסבר על מושג טכני שתמיד התביישתם לשאול עליו:

```
/eli5 what is a DNS record
```

הסבר על קוד בפרויקט שאתם לא מכירים:

```
/eli5 explain the auth flow in this repo
```

הסבר שאתם צריכים להעביר הלאה למישהו לא טכני:

```
/eli5 why we need a staging environment
```

## לבנות סקיל כזה בעצמכם

אם הקובץ הוא שלוש שורות, אין שום סיבה לא לבנות אחד משלכם. פותחים תיקייה חדשה בתוך תיקיית הסקילים האישית שלכם:

```
~/.claude/skills/<skill-name>/SKILL.md
```

ובתוך הקובץ כותבים בדיוק את אותו מבנה:

```markdown
---
name: my-skill
description: when Claude should use this skill
---

The instruction itself, in one or two plain sentences.

Topic: $ARGUMENTS
```

לחלופין, יש פקודה שמייצרת את השלד בשבילכם:

```bash
claude plugin init my-tool
```

**השורה החשובה ביותר היא ה-description. היא זו שקובעת מתי קלוד יפעיל את הסקיל לבד, בלי שתקלידו את הפקודה. תכתבו אותה כמו הוראה למישהו אחר, לא כמו כותרת.**

## טיפים

- תנו נושא ספציפי ולא כללי. eli5 kubernetes יחזיר משהו רדוד, eli5 how a kubernetes pod gets an IP יחזיר משהו שימושי
- הסקיל עובד מצוין גם על הקוד שלכם, לא רק על מושגים כלליים. בקשו ממנו להסביר מודול שאתם לא מכירים
- אפשר לבקש הסבר בעברית. פשוט תוסיפו את זה לתוך הפרומפט אחרי הפקודה
- ה-Artifact הוא HTML רגיל. אפשר לשמור אותו ולהשתמש בו כשקופית להסבר לצוות
- אם הסקיל לא מופיע אחרי ההתקנה, הריצו reload-plugins/ ואם עדיין לא, אתחלו את Claude Code
- המרקטפלייס הקהילתי מכיל אלפי פלאגינים נוספים. אחרי שהוספתם אותו פעם אחת, שווה לפתוח plugin/ ולעבור על הלשונית Discover
- לפני שמתקינים פלאגין קהילתי כלשהו, תסתכלו על הריפו שלו. פלאגין יכול להריץ קוד על המחשב שלכם

## מגבלות ודברים שכדאי לדעת

- הסקיל לא רשמי. הוא עבר את הבדיקה האוטומטית של אנתרופיק אבל הוא לא חלק מהמרקטפלייס הרשמי
- הוא לא מגיע מותקן, ולכן צריך להתקין אותו מחדש בכל סביבה חדשה, אלא אם התקנתם ב-user scope
- התוצאה היא הפשטה. להסבר מדויק ומלא של נושא מורכב, זה לא תחליף לתיעוד הרשמי
- כמו כל סקיל, הוא מוסיף מעט הקשר לכל תור. זה זניח כאן כי הקובץ קטן

## לינקים

- הסקיל בגיטהאב: [github.com/anthropics/claude-plugins-community/tree/main/eli5](https://github.com/anthropics/claude-plugins-community/tree/main/eli5)
- הקובץ SKILL.md עצמו: [SKILL.md](https://github.com/anthropics/claude-plugins-community/blob/main/eli5/skills/eli5/SKILL.md)
- המרקטפלייס הקהילתי: [anthropics/claude-plugins-community](https://github.com/anthropics/claude-plugins-community)
- תיעוד רשמי על התקנת פלאגינים: [code.claude.com/docs/en/discover-plugins](https://code.claude.com/docs/en/discover-plugins)
- תיעוד רשמי על כתיבת סקילים: [code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
