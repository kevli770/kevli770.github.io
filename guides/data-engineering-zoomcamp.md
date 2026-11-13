# Data Engineering Zoomcamp: קורס הנדסת דאטה חינמי של 9 שבועות, מאפס ועד Pipeline אמיתי

> זה קורס חינמי לגמרי בהנדסת דאטה (Data Engineering), שנבנה על ידי הקהילה DataTalksClub של אלכסיי גריגורב (Alexey Grigorev).

מאת קווין ליאקס. פורסם 2026-07-25. נושא: פרודוקטיביות.
העמוד המלא: https://kevli770.github.io/guides/data-engineering-zoomcamp

## מה זה Data Engineering Zoomcamp?

זה קורס חינמי לגמרי בהנדסת דאטה (Data Engineering), שנבנה על ידי הקהילה DataTalksClub של אלכסיי גריגורב (Alexey Grigorev). כל התכנים יושבים ב-repo אחד ב-GitHub שכבר אסף מעל 44 אלף כוכבים, והוא אחד ממאגרי הלימוד הפופולריים בעולם בתחום.

**במקום לשלם מאות ואלפי שקלים על קורס הנדסת דאטה, כאן מקבלים 9 שבועות של תוכן מסודר שבונה איתכם צעד אחר צעד Pipeline אמיתי מקצה לקצה, בדיוק כמו שעובדים בתעשייה.**

היתרון הגדול: לכל שיעור יש גם וידאו בפלייליסט ביוטיוב וגם דוקומנטציה כתובה. אתם באמת רואים איך עושים, ולא רק קוראים.

![ה-repo הרשמי של הקורס ב-GitHub, כל המודולים מסודרים בתיקיות 01 עד 07](https://kevli770.github.io/_astro/01.8py1Rloj.jpg)

*ה-repo הרשמי של הקורס ב-GitHub, כל המודולים מסודרים בתיקיות 01 עד 07*

## מה תלמדו בקורס: 7 המודולים

הקורס בנוי כמסלול הדרגתי. כל מודול מוסיף שכבה על הקודם, עד שיש לכם Pipeline מלא. אלה הכלים המרכזיים שתכירו:

| מודול | נושא | כלים עיקריים |
|---|---|---|
| **1** | Containerization ותשתית כקוד | `Docker, PostgreSQL, Terraform, GCP` |
| **2** | ניהול תזרים עבודה (Orchestration) | `Kestra` |
| **Workshop** | הזרמת נתונים (Data Ingestion) | `dlt (dltHub)` |
| **3** | מחסני נתונים (Data Warehouse) | `BigQuery` |
| **4** | Analytics Engineering | `dbt` |
| **5** | עיבוד Batch | `Apache Spark` |
| **6** | Streaming (זרמים בזמן אמת) | `Kafka, KSQL` |

## איך נראה ה-Pipeline שבונים

הרעיון של הקורס הוא לא ללמד כלי בודד, אלא לחבר את כולם למערכת אחת שזורמת: הנתונים נכנסים, מתוזמנים, נשמרים במחסן, עוברים עיבוד ומודלינג, ומגיעים גם לזרימה בזמן אמת. ככה זה נראה במבט על:

![דיאגרמת ה-Pipeline מתוך הקורס: מ-Docker דרך Kestra, dlt, BigQuery, dbt ו-Spark ועד Kafka](https://kevli770.github.io/_astro/02.CXYu0aTi.jpg)

*דיאגרמת ה-Pipeline מתוך הקורס: מ-Docker דרך Kestra, dlt, BigQuery, dbt ו-Spark ועד Kafka*

## מה צריך להתקין לפני שמתחילים

לפני המודול הראשון כדאי להתקין את הכלים הבאים (כולם חינמיים):

- Docker Desktop: הכלי המרכזי של הקורס, מריץ את כל הסביבות בקונטיינרים
- Python 3.9 ומעלה: מומלץ דרך Anaconda שמגיע עם כל הספריות
- Git: כדי להוריד את ה-repo ולעקוב אחרי העדכונים
- חשבון Google Cloud (GCP): יש שכבת חינם וגם 300 דולר קרדיט למשתמשים חדשים
- Terraform: להקמת התשתית בענן בצורה אוטומטית (מותקן במודול 1)

## התקנה: צעד אחר צעד

### שלב 1: הורדת ה-repo של הקורס

פותחים טרמינל ומריצים (צריך Git מותקן):

```bash
git clone https://github.com/DataTalksClub/data-engineering-zoomcamp.git
cd data-engineering-zoomcamp
```

### שלב 2: הרצת PostgreSQL מקומי עם Docker

במודול הראשון מקימים מסד נתונים Postgres בתוך קונטיינר. זו הפקודה הבסיסית:

```bash
docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:13
```

### שלב 3: הרמת הסביבה המלאה עם Docker Compose

במקום להריץ כל שירות בנפרד, הקורס משתמש ב-docker-compose כדי להרים Postgres ו-pgAdmin יחד בפקודה אחת:

```
docker-compose up -d
```

וזהו: הסביבה עולה, ואפשר להתחיל לטעון את הדאטה של מוניות ניו יורק (NY Taxi) שהקורס עובד איתה.

![שיעור אמיתי מהקורס: הרצת פקודות Docker בטרמינל בתוך VS Code](https://kevli770.github.io/_astro/03.CDC7EsFd.jpg)

*שיעור אמיתי מהקורס: הרצת פקודות Docker בטרמינל בתוך VS Code*

## איך ללמוד נכון

הקורס תוכנן ללמידה עצמאית בקצב שלכם. הדרך המומלצת:

- פותחים את הפלייליסט ביוטיוב וצופים בשיעור של המודול (כל שיעור קצר וממוקד)
- פותחים במקביל את ה-README של אותו מודול ב-GitHub ומריצים את הפקודות בעצמכם
- עושים את שיעורי הבית (Homework) של המודול כדי לוודא שהבנתם
- מי שאוהב טקסט: יש דוקומנטציה כתובה לכל שלב, אז אפשר גם רק לקרוא

![הפלייליסט ביוטיוב: 69 שיעורים עם מעל 1.6 מיליון צפיות](https://kevli770.github.io/_astro/04.CShpYlaf.jpg)

*הפלייליסט ביוטיוב: 69 שיעורים עם מעל 1.6 מיליון צפיות*

## טיפים חשובים

- התחילו לפי הסדר. כל מודול מניח שהבנתם את הקודם, אז אל תדלגו
- עבדו עם ה-repo פתוח ליד הוידאו, למידה אקטיבית שווה פי כמה מלמידה פסיבית
- פתחו חשבון GCP בהתחלה כדי לנצל את ה-300 דולר קרדיט לאורך כל הקורס
- אם נתקעתם: יש קהילת Slack פעילה של הקורס עם אלפי לומדים שעונים
- הקורס נפתח מחדש כמחזור (Cohort) בכל ינואר, אבל אפשר ללמוד לבד מתי שרוצים
- בסוף הקורס יש פרויקט גמר, זה מה שהופך את הידע לתיק עבודות אמיתי לראיונות

## לינקים חשובים

- ה-repo הרשמי (GitHub): [github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)
- הפלייליסט המלא ביוטיוב: [YouTube Playlist](https://www.youtube.com/playlist?list=PL3MmuxUbc_hJed7dXYoJw8DoCuVHhGEQb)
- הרשמה לקורס ולקהילה: [DataTalks.Club](https://datatalks.club/)

---
קווין ליאקס מעביר סדנאות AI, הרצאות והטמעה לארגונים: https://kevli770.github.io/workshops
