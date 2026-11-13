# DaVinci Resolve + AI Assistants: איך מחברים את Claude או Codex ל-DaVinci Resolve ונותנים להם לעשות את העבודה השחורה

> עד עכשיו, מי שרצה ש-AI יעזור לו לערוך וידאו היה צריך להתקין פרויקט קוד פתוח כמו Remotion או HyperFrames, או שרת MCP של מישהו מהקהילה. הפעם החיבור מגיע מ-Blackmagic עצמם: הוא יושב בתפריט של התוכנה, רץ מקומית על המחשב שלכם, ופעיל רק כש-Resolve והעוזר פתוחים יחד.

מאת קווין ליאקס. פורסם 2026-09-15. נושא: עיצוב ווידאו.
העמוד המלא: https://kevli770.github.io/guides/davinci-resolve-mcp

## מה זה ה-MCP של DaVinci Resolve

ב-8 בספטמבר 2026 Blackmagic Design שחררו את DaVinci Resolve 21.1, ובתוכו **אינטגרציה רשמית לעוזרי AI**. Claude, Claude Code ו-ChatGPT Codex מתחברים לתוכנה דרך MCP (Model Context Protocol) ומבצעים בתוכה פעולות אמיתיות: לנתח פרויקט, לארגן מדיה, לשנות הגדרות ולרנדר.

עד עכשיו, מי שרצה ש-AI יעזור לו לערוך וידאו היה צריך להתקין פרויקט קוד פתוח כמו Remotion או HyperFrames, או שרת MCP של מישהו מהקהילה. **הפעם החיבור מגיע מ-Blackmagic עצמם**: הוא יושב בתפריט של התוכנה, רץ מקומית על המחשב שלכם, ופעיל רק כש-Resolve והעוזר פתוחים יחד.

![החיבור davinci-resolve בתוך Claude Desktop, מתוך סרטון ההשקה של Blackmagic](https://kevli770.github.io/_astro/01.CEndN2Go.jpg)

*החיבור davinci-resolve בתוך Claude Desktop, מתוך סרטון ההשקה של Blackmagic*

## מה צריך לפני שמתחילים

| דרישה | פירוט |
|---|---|
| **Resolve Studio 21.1** | הגרסה בתשלום (רישיון חד פעמי, $295). בגרסה החינמית של Resolve האינטגרציה לא קיימת. |
| **עוזר AI** | Claude Desktop, Claude Code או ChatGPT Codex. אצל חלק מהספקים נדרש מנוי בתשלום. |
| **אותו מחשב** | השרת רץ מקומית, על המחשב שבו פתוח Resolve. |
| **פרויקט בדיקה** | ה-AI מבצע שינויים אמיתיים בפרויקט. מתחילים על עותק או על פרויקט ניסיון. |

## איך מחברים, צעד אחר צעד

1. מעדכנים ל-DaVinci Resolve Studio 21.1 ומוודאים את מספר הגרסה בחלון About.
2. פותחים את התפריט File ובוחרים Setup AI Assistants.

![Setup AI Assistants נמצא בתחתית תפריט File](https://kevli770.github.io/_astro/02.Cap-ht0y.jpg)

*Setup AI Assistants נמצא בתחתית תפריט File*

3. בוחרים את העוזר שלכם (Claude, Claude Code או Codex) ומאשרים את החיבור. ב-Claude Desktop הוא מופיע בשם davinci-resolve עם מתג Enabled.
4. משאירים את Resolve ואת העוזר פתוחים יחד. השרת פעיל רק כששניהם רצים.
5. פותחים שיחה חדשה ושולחים פקודת בדיקה שרק קוראת מהפרויקט ולא משנה בו כלום:

```
Give me the clip count and total duration of the current media.
Report any missing media as well as marker positions and names.
```

אם חזרה תשובה עם מספר הקליפים, המשך הטיימליין והמרקרים, החיבור עובד.

*התשובה של Claude לפקודת הבדיקה, בתוך Resolve*

### אם העוזר לא רואה את Resolve

- סוגרים ופותחים מחדש את אפליקציית ה-AI אחרי שאישרתם את החיבור.
- מוודאים ששני הצדדים פתוחים באותו רגע, על אותו מחשב.
- נכנסים ל-Preferences, לשונית System ואז General, ובודקים שהאפשרות External scripting using לא מכובה (None).
- סוגרים פרויקטים אחרים ומשאירים פתוח רק את הפרויקט שעליו עובדים.

## מה אפשר לבקש ממנו

כל הפקודות בטבלה לקוחות מסרטון ההשקה הרשמי של Blackmagic. אפשר לכתוב אותן גם בעברית, אבל באנגלית קל יותר לבדוק מה בדיוק ביקשתם.

| משימה | פקודה לדוגמה |
|---|---|
| **לארגן ולבדוק מדיה** | `Give me the clip count and total duration of the current media. Report any missing media as well as marker positions and names.` |
| **להסיר קטעים לא רצויים** | `Remove any edits that are shorter than 1 second and then render an H.265 video of this timeline.` |
| **עריכת Highlights** | `Take the existing clip in the DaVinci Resolve media pool and make a three minute highlight edit timeline based on words.` |
| **לעבוד לפי מרקרים** | `Ripple delete all the clips with red markers from this timeline. Then move all the clips with green markers to V2.` |
| **כמה גרסאות** | `Render out the current timeline as H.264, H.265 and ProRes422.` |

![הבקשה להסיר את כל הקטעים הקצרים משנייה ולרנדר H.265](https://kevli770.github.io/_astro/03.jL6CyodP.jpg)

*הבקשה להסיר את כל הקטעים הקצרים משנייה ולרנדר H.265*

![שלוש משימות רינדור שנוצרו מפקודה אחת: H.264, H.265 ו-ProRes 422](https://kevli770.github.io/_astro/04.q5SnhReu.jpg)

*שלוש משימות רינדור שנוצרו מפקודה אחת: H.264, H.265 ו-ProRes 422*

## דוגמה מלאה: משלוש שעות של חומר לעריכה ראשונה

חזרתם מצילום עם הרבה חומר גלם. כך נראה סבב עבודה מלא, פקודה אחרי פקודה:

1. מייבאים את כל הקבצים ל-Media Pool ופותחים טיימליין חדש.
2. מבקשים דוח: כמה קליפים יש, מה האורך הכולל והאם חסרה מדיה.
3. מסמנים במרקר אדום את מה שלא רוצים, ומבקשים למחוק את כל הקליפים המסומנים באדום.
4. מבקשים לעבוד על עותק ולבנות ממנו עריכת Highlights של שלוש דקות:

```
Duplicate the current timeline and work only on the copy.
Make a three minute highlight edit based on words.
```

5. מבקשים להסיר כל קטע שקצר משנייה, כדי שהעריכה לא תקפוץ.
6. מבקשים לרנדר כמה גרסאות, למשל H.264 לרשתות ו-ProRes 422 לארכיון.
7. צופים בתוצאה בעצמכם ומחליטים מה נשאר. זה החלק שה-AI לא עושה בשבילכם.

![מרקרים בצבעים הם דרך נוחה לתת לסוכן הוראות מדויקות](https://kevli770.github.io/_astro/05.ByySfizk.jpg)

*מרקרים בצבעים הם דרך נוחה לתת לסוכן הוראות מדויקות*

## טיפים

- תמיד מתחילים בפקודה שרק קוראת מהפרויקט, ורק אחר כך עוברים לשינויים.
- עובדים על עותק של הטיימליין. אם משהו השתבש, המקור נשאר כמו שהיה.
- משימה אחת בכל פקודה. קל יותר לבדוק מה השתנה.
- בסוף כל משימה מבקשים סיכום קצר של כל מה שהשתנה.
- משתמשים במרקרים בצבעים כהוראות: אדום למחיקה, ירוק להעברה.
- בודקים כל קובץ שיוצא מה-Render Queue לפני שמפרסמים אותו.

## מגבלות

- זמין רק ב-DaVinci Resolve Studio 21.1, לא בגרסה החינמית.
- ה-AI לא יודע איזה סיפור אתם רוצים לספר. הוא חוסך את העבודה השחורה, וההחלטות היצירתיות נשארות אצלכם.
- הוא מבצע שינויים אמיתיים בפרויקט, ולכן כדאי לשמור גיבוי לפני כל סבב.
- לפי דיווחים מהקהילה, חלק מהפעולות עדיין לא נתמכות, למשל הזזה של קליפ קיים למקום אחר בטיימליין.
- השימוש בעוזר עצמו כפוף למגבלות ולמנוי אצל הספק (Anthropic או OpenAI).

## לינקים

- ההכרזה הרשמית של Blackmagic: [blackmagicdesign.com/media/release/20260908-03](https://www.blackmagicdesign.com/media/release/20260908-03)
- מה חדש ב-DaVinci Resolve: [blackmagicdesign.com/products/davinciresolve/whatsnew](https://www.blackmagicdesign.com/products/davinciresolve/whatsnew)
- סרטון ההשקה הרשמי: [youtube.com/watch?v=Wi44XLKQ3YA](https://www.youtube.com/watch?v=Wi44XLKQ3YA)
- סקירה מפורטת ב-CineD: [cined.com](https://www.cined.com/davinci-resolve-21-1-released-ai-assistant-integration-via-mcp-individual-hdr-trims-and-python-scripting-moves-to-studio/)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
