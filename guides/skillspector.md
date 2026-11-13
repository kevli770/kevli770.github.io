# SkillSpector: לסרוק כל Skill לפני שמתקינים אותו

> SkillSpector הוא כלי קוד-פתוח של NVIDIA שסורק Skills של סוכני AI (כמו אלה של Claude Code, Codex ו-Gemini CLI) ומאתר בהם פרצות אבטחה, קוד זדוני ודפוסים מסוכנים, עוד לפני שאתם מתקינים אותם.

מאת קווין ליאקס. פורסם 2026-07-07. נושא: אבטחה ופרטיות.
העמוד המלא: https://kevli770.github.io/guides/skillspector

## מה זה SkillSpector?

SkillSpector הוא כלי קוד-פתוח של NVIDIA שסורק Skills של סוכני AI (כמו אלה של Claude Code, Codex ו-Gemini CLI) ומאתר בהם פרצות אבטחה, קוד זדוני ודפוסים מסוכנים, עוד לפני שאתם מתקינים אותם.

**Skill הוא לא סתם תוסף תמים. זה קוד שהסוכן שלכם מריץ עם ההרשאות שלכם, הגישה לקבצים שלכם ולמפתחות ה-API שלכם. NVIDIA בדקו 42,447 סקילים ממרקטים שונים וגילו ש-26% מהם מכילים לפחות פרצת אבטחה אחת, ו-5% ממש זדוניים.**

![הריפו הרשמי: github.com/NVIDIA/SkillSpector](https://kevli770.github.io/_astro/01.D_jVikpu.png)

*הריפו הרשמי: github.com/NVIDIA/SkillSpector*

## למה זה קריטי

כשאתם מתקינים Skill, אתם נותנים לו גישה ל:

- ההרשאות שלכם: כל מה שהסוכן יכול לעשות, גם ה-Skill יכול
- הקבצים שלכם: קריאה וכתיבה בתיקיות שהסוכן ניגש אליהן
- המפתחות והסודות שלכם: API keys, tokens, credentials

שלושת האיומים הנפוצים: גניבת מפתחות, משיכת קוד מרוחק, והוראות נסתרות שמשתלטות על הסוכן מבפנים (prompt injection). זה לא פאניקה, זו היגיינה בסיסית, בדיוק כמו שלא מריצים קובץ אקראי מהאינטרנט.

## איך מתקינים את SkillSpector

הדרך המהירה: פקודה אחת עם uv (דורש Python 3.12 ומעלה):

```bash
uv tool install git+https://github.com/NVIDIA/skillspector.git
```

![איך מתקינים את SkillSpector](https://kevli770.github.io/_astro/02.CGu5eSWq.png)

לעדכון בעתיד:

```bash
uv tool update skillspector
```

אפשר גם דרך Docker, בלי Python בכלל:

```bash
docker run --rm -v "$PWD:/scan" skillspector scan ./my-skill/
```

## איך סורקים Skill

לפני כל Skill חדש שאתם מתקינים, מריצים עליו סריקה. SkillSpector מקבל תיקייה מקומית, קובץ בודד, לינק ל-GitHub או קובץ ZIP:

```
skillspector scan ./my-skill/
skillspector scan ./SKILL.md
skillspector scan https://github.com/user/my-skill
```

רוצים דוח מסודר לשמירה? אפשר לייצא ל-Markdown, JSON או SARIF:

```
skillspector scan ./my-skill/ --format markdown --output report.md
```

## איך מבינים את ציון הסיכון

SkillSpector נותן לכל Skill ציון סיכון מ-0 עד 100. ככל שהציון גבוה יותר, כך ה-Skill מסוכן יותר. הכלל הפשוט: מעל 51, פשוט לא מתקינים.

| ציון | רמה | המלצה |
|---|---|---|
| `0 - 20` | **LOW** | בטוח להתקנה |
| `21 - 50` | **MEDIUM** | זהירות: כדאי לבדוק ידנית |
| `51 - 80` | **HIGH** | לא להתקין |
| `81 - 100` | **CRITICAL** | מסוכן: בהחלט לא |

## מה SkillSpector מזהה

הכלי בודק 68 דפוסי פגיעות ב-17 קטגוריות שונות, ביניהן:

- Prompt Injection: הזרקת הוראות והוראות נסתרות בטקסט
- Data Exfiltration: הדלפת מידע וסודות החוצה
- Privilege Escalation: הסלמת הרשאות וגישה למפתחות
- Supply Chain: תלויות לא בטוחות וקוד מעורפל
- Excessive Agency: גישה בלתי מוגבלת לכלים ולמשאבים

![טבלת הדפוסים המלאה מתוך התיעוד הרשמי](https://kevli770.github.io/_astro/03.B-yrSLU2.png)

*טבלת הדפוסים המלאה מתוך התיעוד הרשמי*

## שלב מתקדם: ניתוח סמנטי עם LLM

מעבר לסריקה הסטטית המהירה, SkillSpector יכול להפעיל גם ניתוח חכם עם מודל שפה (Claude, GPT ואחרים) שבודק את הכוונה מאחורי הקוד ומסנן התראות שווא. רוצים סריקה מהירה ופרטית לגמרי, בלי לשלוח שום דבר לענן? הוסיפו no-llm:

```
skillspector scan ./my-skill/ --no-llm
```

## טיפים

- הפכו את הסריקה להרגל: לפני כל Skill חדש, סרקו אותו קודם
- מעל ציון 51: אל תתקינו, גם אם ה-Skill נראה שימושי
- אפשר לסרוק ישר מהלינק ב-GitHub בלי להוריד כלום
- השתמשו ב-no-llm לסריקה מהירה ופרטית שלא שולחת קוד החוצה
- שמרו baseline כדי שסריקות חוזרות יראו רק בעיות חדשות

## לינקים

- הריפו ב-GitHub: [github.com/NVIDIA/SkillSpector](https://github.com/NVIDIA/SkillSpector)
- התיעוד הרשמי: [docs.nvidia.com/skills](https://docs.nvidia.com/skills/scanning-agent-skills)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
