# Automated Security Updates: צק ליסט להגדרת עדכוני אבטחה אוטומטיים ובדיקת התלויות בפרויקטים שלכם

> ב-22 באוגוסט 2026 אניל מדהוואפדי, פרופסור למדעי המחשב בקיימברידג' ומתחזק של ספריית cohttp ב-OCaml, שחרר תיקון לחולשת path traversal. הוא עשה בדיוק מה שכולם עושים בקוד פתוח: הכין את התיקון ופתח PR ציבורי כדי שעוד אנשים יוכלו לעבור עליו.

מאת קווין ליאקס. פורסם 2026-08-31. נושא: אבטחה ופרטיות.
העמוד המלא: https://kevli770.github.io/guides/security-updates

## למה השבוע הזה שאתם מחכים כבר לא קיים

ב-22 באוגוסט 2026 אניל מדהוואפדי, פרופסור למדעי המחשב בקיימברידג' ומתחזק של ספריית cohttp ב-OCaml, שחרר תיקון לחולשת path traversal. הוא עשה בדיוק מה שכולם עושים בקוד פתוח: הכין את התיקון ופתח PR ציבורי כדי שעוד אנשים יוכלו לעבור עליו.

**בערך עשר דקות אחר כך השרת החי שלו כבר קיבל בקשות שסורקות בדיוק את סוג החולשה שהוא תיקן, רצפי traversal מקודדים באחוזים. סורקים אוטומטיים עוקבים אחרי ריפוזיטוריז ציבוריים, והתיקון עצמו הוא ההכרזה על החולשה.**

ואז הוא בדק כמה קל להגיע מכנראה יש פה באג ליש פה exploit. הוא נתן לסוכן AI רק כיוון כללי איפה לחפש, בלי לגלות את הבאג עצמו. בתוך פחות מדקה הסוכן בנה פרצה עובדת שמנצלת את הבעיה.

זה החלק שצריך להדליק לכם נורה. פעם תוקף היה צריך לקרוא את ה-PR, להבין את הקוד, למצוא את החולשה ולבנות לה exploit. עכשיו Agent עושה חלק גדול מהעבודה הזאת אוטומטית. מהרגע שתיקון אבטחה הופך לפומבי, השעון רץ.

## וזה לא מקרה אחד

צוות המחקר Unit 42 של Palo Alto Networks בנה מערכת AI אוטונומית בשם NOVA שמחפשת חולשות בקוד פתוח. הנה מה שהיא הספיקה בחודשיים:

| מספר | מה הוא אומר |
|---|---|
| **3,915** | פרויקטי קוד פתוח שנסרקו בחודשיים |
| **14,090** | חולשות מאומתות שנמצאו |
| **99.4%** | מהן לא היו מדווחות בפומבי לפני כן |
| **40%** | סווגו כחומרה גבוהה או קריטית |

**אם אתם בונים אפליקציה עם עשרות ספריות NPM או פייתון, חלק מהן כבר נמצאות בסריקה כזאת. המסקנה המעשית פשוטה: אל תחכו שבוע כדי לראות אם עדכון אבטחה באמת חשוב.**

## הצק ליסט: שישה שלבים

כל שלב לוקח בין דקה לרבע שעה. עשו אותם לפי הסדר.

### שלב 1: להדליק Dependabot alerts

זה מתג בהגדרות הריפו, לא קובץ. הוא סורק את גרף התלויות שלכם מול מסד החולשות של GitHub ופותח התראה על כל ספרייה פגיעה.

- בריפו: Settings ואז Advanced Security
- מדליקים Dependency graph
- מדליקים Dependabot alerts
- בארגון שלם אפשר להדליק את זה על כל הריפוזיטוריז בבת אחת מההגדרות ברמת הארגון

### שלב 2: להדליק Dependabot security updates

זה השלב שהופך התראה ל-PR. ברגע שנפתחת התראה, Dependabot פותח לבד Pull Request שמעדכן את הספרייה לגרסה המתוקנת, כולל תיאור של החולשה וציון תאימות.

- באותו מסך: Dependabot security updates
- כדאי להדליק גם Grouped security updates, מאחד כמה תיקונים ל-PR אחד במקום להציף אתכם

### שלב 3: להוסיף dependabot.yml לעדכוני גרסה שוטפים

עדכוני האבטחה עובדים בלי קובץ. הקובץ הזה נועד לדבר אחר: להשאיר את התלויות מעודכנות באופן שוטף, כדי שכשיגיע תיקון אבטחה הקפיצה תהיה קטנה ולא שדרוג ענק שנתקע.

יוצרים את הקובץ בנתיב .github/dependabot.yml:

```
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 5
    groups:
      minor-and-patch:
        update-types: ["minor", "patch"]

  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"

  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

*שימו לב ל-github-actions בסוף: ה-Actions שלכם הן תלות לכל דבר, ולרבות מהן יש הרשאות כתיבה לריפו.*

### שלב 4: בדיקת תלויות ב-CI

Dependabot בודק את מה שהוא מכיר. בדיקה מקומית ב-CI תופסת גם דברים שנכנסו דרך תלויות עקיפות, ומכשילה את הבילד לפני שהוא מגיע לפרודקשן.

ל-Node:

```bash
npm audit --audit-level=high
```

לפייתון:

```bash
pip install pip-audit
pip-audit
```

לכל שפה, מבוסס מסד ה-CVE הפתוח של Google:

```
osv-scanner scan source -r .
```

### שלב 5: לקצר את הזמן בין Patch ל-Deployment

זה הסעיף החשוב ביותר במסמך הזה. תיקון שיושב שבוע ב-PR פתוח לא מגן על אף אחד. הדרך הבטוחה לקצר את הזמן היא auto-merge מותנה: רק PR של Dependabot, רק patch ו-minor, ורק אחרי שכל הבדיקות עברו.

קובץ .github/workflows/dependabot-automerge.yml:

```
name: dependabot auto-merge
on: pull_request
permissions:
  contents: write
  pull-requests: write
jobs:
  automerge:
    if: github.actor == 'dependabot[bot]'
    runs-on: ubuntu-latest
    steps:
      - uses: dependabot/fetch-metadata@v2
        id: meta
      - if: steps.meta.outputs.update-type != 'version-update:semver-major'
        run: gh pr merge --auto --squash "$PR_URL"
        env:
          PR_URL: ${{ github.event.pull_request.html_url }}
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**צריך גם להדליק Allow auto-merge בהגדרות הריפו, ולהגדיר branch protection שדורש שה-CI יעבור. בלי זה auto-merge פשוט ידחוף קוד לא בדוק.**

### שלב 6: ניטור CVE מעבר לריפו

Dependabot מכסה את התלויות בריפו. מה שהוא לא מכסה: כלים שהתקנתם ידנית, תמונות דוקר, ותשתית. הוסיפו מקור התראות חיצוני אחד לפחות.

- GitHub Advisory Database: אפשר לעקוב אחרי אקוסיסטם ספציפי
- OSV.dev: מסד חולשות פתוח עם API, מכסה עשרות אקוסיסטמים
- CISA KEV: רשימת החולשות שכבר מנוצלות בשטח, זה הפילטר הכי מעשי לתעדוף
- Trivy או Grype לסריקת תמונות דוקר ב-CI

## טבלת סיכום: מה מדליקים ואיפה

| מה | איפה | למה |
|---|---|---|
| **Dependabot alerts** | הגדרות הריפו | מודיע לכם שיש חולשה בתלות |
| **Security updates** | הגדרות הריפו | פותח PR שמתקן אותה לבד |
| **dependabot.yml** | .github/dependabot.yml | שומר על התלויות עדכניות כל הזמן |
| **npm audit / pip-audit** | צעד ב-CI | מכשיל בילד עם חולשה חמורה |
| **auto-merge** | GitHub Action | מקצר את הזמן מתיקון לפרודקשן |
| **CISA KEV** | מקור התראות חיצוני | מסמן מה כבר מנוצל בשטח |

## טיפים

- תעדפו לפי ניצול בפועל, לא לפי ציון CVSS. חולשה בציון 7 שכבר מנוצלת מסוכנת יותר מ-9.8 תיאורטית
- grouped updates זה מה שמונע מכם להתעלם מ-Dependabot אחרי שבועיים של הצפה
- אל תשאירו auto-merge בלי branch protection, זה הופך אוטומציה נוחה לצינור לפרודקשן בלי בקרה
- העדכונים השוטפים הם מה שהופך תיקון אבטחה לקפיצת patch קטנה במקום שדרוג major שנתקע חודש
- אם אתם מתחזקים קוד פתוח: קחו בחשבון שה-PR הציבורי הוא ההכרזה. שקלו לשחרר תיקון וגרסה באותו יום
- מדדו את הזמן מרגע שתיקון אבטחה נכנס ועד שהוא בפרודקשן. אם אתם לא יודעים מה המספר, הוא גרוע

## מגבלות: מה זה לא פותר

- Dependabot לא מוצא חולשות בקוד שאתם כתבתם, רק בתלויות. לזה צריך code scanning
- התיקון קיים רק אם המתחזק שחרר גרסה מתוקנת. בספרייה נטושה תצטרכו fork או תחליף
- עדכון אוטומטי יכול לשבור לכם משהו. בלי בדיקות אמיתיות ב-CI, auto-merge זו סכנה ולא הגנה

## לינקים

- הפוסט המקורי של אניל מדהוואפדי: [anil.recoil.org/notes/rumour-is-the-exploit](https://anil.recoil.org/notes/rumour-is-the-exploit)
- ה-PR שהתחיל את הכל: [github.com/mirage/ocaml-cohttp/pull/1145](https://github.com/mirage/ocaml-cohttp/pull/1145)
- המחקר של Unit 42: [unit42.paloaltonetworks.com/frontier-ai-vulnerability-burst](https://unit42.paloaltonetworks.com/frontier-ai-vulnerability-burst/)
- תיעוד Dependabot security updates: [docs.github.com, dependabot security updates](https://docs.github.com/en/code-security/dependabot/dependabot-security-updates/about-dependabot-security-updates)
- כל אפשרויות dependabot.yml: [docs.github.com, dependabot options reference](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference)
- מסד החולשות הפתוח: [osv.dev](https://osv.dev/)
- רשימת החולשות שמנוצלות בשטח: [CISA Known Exploited Vulnerabilities](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
