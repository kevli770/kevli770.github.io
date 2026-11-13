# GPT-6 Astra: תשעה פרומפטים שמנצלים את היכולות החדשות

> המודל החדש של OpenAI, שיצא בתחילת ספטמבר 2026 ומגיע למנויי ChatGPT בתשלום. הוא לא עוד שדרוג של איכות התשובות, הוא נבנה סביב משימות שלמות: מחקר, גלישה, קוד, עבודה על המחשב, מסמכים וגיליונות.

מאת קווין ליאקס. פורסם 2026-09-05. נושא: ChatGPT.
העמוד המלא: https://kevli770.github.io/guides/gpt-astra-prompts

## מה זה GPT-6 Astra

המודל החדש של OpenAI, שיצא בתחילת ספטמבר 2026 ומגיע למנויי ChatGPT בתשלום. הוא לא עוד שדרוג של איכות התשובות, הוא נבנה סביב משימות שלמות: מחקר, גלישה, קוד, עבודה על המחשב, מסמכים וגיליונות.

הדרך הכי מהירה להבין את זה היא להסתכל על ההדגמות שאוקיי איי עצמם פרסמו יחד עם ההכרזה: מילוי טופס מס שלם בדפדפן, בניית סצנת עיר ב-Unity, בניית דשבורד ב-Power BI, פתרון אתגר מודלים פיננסיים באקסל, וניתוב לוח מעגלים מודפס ב-KiCad. כל אחת מהן היא הקלטת מסך של המודל עובד, לא צילום של תשובה.

**כלומר: אם אתם משתמשים בו רק כדי לשאול שאלות, אתם משלמים על היכולת ולא מפעילים אותה.**

## מה מוסיף GPT Work

היכולות האלה נפתחות באמת כשמריצים את המודל בתוך GPT Work ולא בצ'אט הרגיל. ההבדל הוא לא בתשובה: הוא בסביבה שהמודל יושב בתוכה.

| מה יש ב-Work | למה זה משנה |
|---|---|
| **סביבת קוד עם אינטרנט** | הוא יכול להתקין חבילות, למשוך ריפו ולדבר עם ממשקי API. בצ'אט הרגיל הגישה הזאת חסומה. |
| **דפדפן מלא** | כרום אמיתי שהוא מפעיל: טוען אתרים, ממלא טפסים ומצלם מסך. זה מה שמאפשר לו לפעול באתרים במקומכם. |
| **קבצים שנשארים בין סשנים** | לכל שיחה יש תיקייה משלה, והתיקיות נשמרות. אפשר לחזור לקבצים משיחה קודמת במקום להסביר מחדש. |
| **סוכני משנה** | הוא יכול לפצל עבודה למספר סוכנים שרצים במקביל. פיצ'ר למשתמשים מתקדמים, ולא קיים בצ'אט. |

## הכלל שמאחורי כל הפרומפטים

כל תשעת הפרומפטים כאן בנויים על אותו שינוי אחד: מתארים תוצאה, לא מבקשים תשובה.

במקום לבקש ממנו לכתוב מייל, אומרים לו מה המצב שרוצים להגיע אליו ונותנים לו להחליט אילו צעדים לעשות. זו הסיבה שהפרומפטים ארוכים: הם מגדירים מטרה, אילוצים וצורת פלט.

**לפני:** תכתוב לי מייל כדי להוריד את חשבון האינטרנט

**אחרי:** תעבור על החשבונות שלי, תבדוק במה אני כבר לא משתמש, ותנסה להוריד לי את המחיר

שלושה דברים שכל פרומפט טוב כאן כולל: מטרה מדידה, גבולות ברורים למה מותר לו לעשות, ופורמט פלט שאתם רוצים לקבל.

## תשעת הפרומפטים

*הפרומפטים באנגלית במכוון: כך הם נוסחו במקור, וכך הם עובדים הכי טוב. מה שבסוגריים מרובעים הוא מה שאתם מחליפים בשלכם.*

### 1. מנהל המשא ומתן על החשבונות

הפרומפט הכי קל להתחיל איתו, כי התוצאה שלו נמדדת בכסף. אסטרה נכנס לצ'אט התמיכה של כל ספק בעצמו, מנהל את השיחה, ומחזיר לכם מה ירד ומה בוטל. אתם לא כותבים מייל אחד.

**הפרומפט להעתקה:**

```
Go through my internet, phone, and software bills, jump into each provider's chat support,
and negotiate them down or cancel what I'm not using.
```

### 2. להפוך שירות לתוכנה

פירוק של עסק שירות קיים לשלבים, כלים, קלטים ופלטים, ואז אפיון של המוצר הכי פשוט שיכול להחליף את הגרסה הראשונה שלו. שימושי במיוחד אם אתם מחפשים רעיון למוצר ולא לרעיון.

**הפרומפט להעתקה:**

```
Pick one service business in [niche] and reverse-engineer the exact workflow they sell to
clients. Break it into steps, tools used, inputs, outputs, human judgment points, and places
where the work gets slow or expensive. Then design the simplest AI product that could
replace the first version of that service and charge $500-$5,000/month.
```

### 3. הצייד של המציאות ביד שנייה

ניטור רציף של לוחות מודעות. זה לא חיפוש חד פעמי אלא משימה שרצה ברקע ומודיעה לכם ברגע שמשהו מתומחר נמוך מדי.

**הפרומפט להעתקה:**

```
Watch Facebook Marketplace and Craigslist in my city for [cameras / furniture / bikes]
listed way under market, and text me the second one's mispriced with the link.
```

### 4. הדשבורד של חברת איש אחד

הפרומפט שהוזכר בסרטון. הוא לוקח את המסמכים, הפגישות והנתונים שכבר קיימים אצלכם ובונה מהם דוח שבועי שאומר מה מכניס כסף, מה מבזבז זמן, ומה שלושת הדברים שכדאי לעשות עכשיו.

**הפרומפט להעתקה:**

```
Look at my docs, notes, Stripe exports, analytics, customer calls, and project list, then
build a weekly operator dashboard. I want to know what is making money, what is wasting
time, what customers are asking for, what I should stop doing, and the three
highest-leverage actions for next week. Be blunt and show your work.
```

### 5. איפה סוכן מחליף גיוס

מיפוי של המשימות בעסק שכדאי להעביר לסוכן לפני שמגייסים עוד אדם, עם הערכה של זמן, עלות טעויות וקושי אוטומציה לכל אחת. התוצאה היא סדר עדיפויות, לא רשימת רעיונות.

**הפרומפט להעתקה:**

```
Look at how this business works and find the tasks we should give to agents before hiring
another person. For each task, estimate the current human time, the cost of mistakes, the
tools involved, the difficulty of automating it, and the first safe version we could deploy.
Prioritize things that save money or create revenue within 30 days.
```

### 6. מפעיל הדפדפן

כאן משתמשים בדפדפן שיש לאסטרה בפועל. הוא עובר את התהליך באתרים האמיתיים, ממלא טפסים, ובסוף מחזיר גם את התוצאה וגם נוהל עבודה חוזר שאפשר להפוך לאוטומציה.

**הפרומפט להעתקה:**

```
Use the browser to complete this workflow: [workflow]. As you go, click through the actual
sites, collect the data, fill the forms where appropriate, and keep notes on what broke or
slowed you down. When you're done, give me the output, the repeatable SOP, and the
automation plan so this can become an agent.
```

### 7. צוות הבדיקות הלילי

בדיקה חוזרת של המסלולים הקריטיים באפליקציה שלכם, כל לילה, עם צילומי מסך של כל דבר שנשבר או מבלבל. זה הפרומפט שהכי משתלם להריץ כמשימה מתוזמנת.

**הפרומפט להעתקה:**

```
Every night, open my app on a real phone, go through signup, checkout, and the main flows,
and screenshot anything that's broken or confusing.
```

### 8. המרגל אחרי המתחרים

הדוגמה השנייה מהסרטון. אסטרה נרשם למתחרים, יושב בתוך המוצר והמיילים שלהם, ושולח דוח חודשי על כל פיצ'ר חדש ושינוי מחיר. שימו לב לתנאי השימוש של המתחרים לפני שאתם מריצים את זה.

**הפרומפט להעתקה:**

```
Sign up for my top 3 competitors, sit inside their product and their emails, and send me a
monthly report on every new feature, price change, and thing they do better than us.
```

### 9. משחק שהוא לד מגנט

בניית משחק דפדפן שלם סביב מכניקה אחת, כולל התקדמות, ניקוד ואיסוף לידים. זה הפרומפט שמנצל הכי הרבה את יכולות הקוד של אסטרה.

**הפרומפט להעתקה:**

```
Build a browser game around this mechanic: [mechanic]. Don't just make a cute demo; add
progression, tension, scoring, failure, polish, and one reason someone would send it to a
friend. Then add lead capture (email/sms), it needs to tie into my core product which sells
XYZ.
```

## טיפים

- תריצו פרומפט אחד בכל פעם. תשע משימות במקביל זה הדרך המהירה ביותר לקבל תשע תוצאות בינוניות.
- תגידו לו במפורש מה אסור לו לעשות. במיוחד בפרומפטים שנוגעים בחשבונות, בכסף או בשליחת הודעות.
- תבקשו שיראה את העבודה. הפרומפט הרביעי אומר את זה במפורש, וכדאי להוסיף את זה גם לשאר.
- כל דבר שחוזר על עצמו, תהפכו למשימה מתוזמנת במקום להריץ ידנית.
- בפעם הראשונה תריצו על נתונים לא רגישים, כדי לראות איך הוא עובד לפני שנותנים לו גישה אמיתית.
- אם התוצאה רדודה, כמעט תמיד הבעיה היא שלא הגדרתם פורמט פלט. תגידו לו בדיוק איך התוצר צריך להיראות.

## מגבלות ששווה להכיר

- פעולות שמשנות משהו בעולם: שליחת הודעות, ביטול מנויים, מילוי טפסים, מחייבות אישור שלכם. אל תפעילו אותן על טייס אוטומטי.
- כניסה לחשבונות דורשת התחברות שלכם. אל תדביקו סיסמאות לתוך הפרומפט.
- רישום לשירותים של מתחרים או גרידה של אתרים עלול לסתור את תנאי השימוש שלהם. זו האחריות שלכם.
- משימות ארוכות עולות יותר. תתחילו בגרסה מצומצמת של הפרומפט ותרחיבו אחרי שראיתם שהכיוון נכון.

## לינקים

- ההכרזה הרשמית של GPT-6 Astra: [openai.com/index/gpt-6-astra](https://openai.com/index/gpt-6-astra/)
- הפוסט המקורי עם תשעת הפרומפטים: [x.com/gregisenberg](https://x.com/gregisenberg/status/2095854071580156338)
- פירוק מפורט של מה יש ב-ChatGPT Work: [simonwillison.net](https://simonwillison.net/2026/Aug/30/understanding-chatgpt-work/)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
