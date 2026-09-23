
README.md
100%
# Japan Trip App

## Production App

כתובת קבועה למשתמשים:

https://rongoldner.github.io/japan26/

הכתובת מפנה ל-Google Apps Script Production Web App.

Direct Apps Script URL:

https://script.google.com/macros/s/AKfycbxnoAkWqYBgrLCEY5JfZDXJZFs08B0lHJ11iZz8_Wl5mphxr6uT19lAIf8ehi1YQac/exec

אפליקציית Web זוגית לטיול יפן 2026, מבוססת Google Apps Script + Google Sheets.

## קבצי הפרויקט

- `Code.gs` — צד השרת של Google Apps Script.
- `index.html` — ממשק האפליקציה, מותאם בעיקר לטלפון.
- `appsscript.json` — Manifest של פרויקט Apps Script.
- `README.md` — תיעוד הפרויקט.
- `Update_Workflow.md` — סדר העבודה הקבוע לעדכוני Production, Drive ו-GitHub.
- `WebApp_URL.txt` — כתובת ה-Production הישירה.
- `Japan Trip App.url` — קיצור דרך לכתובת הקבועה `https://rongoldner.github.io/japan26/`.

## בקרת גישה

האפליקציה מוגנת בצד השרת ומאפשרת שימוש רק לחשבונות Google מורשים.

ברירת המחדל בקוד:

- `ron.goldner@gmail.com`
- `tali.becker@gmail.com`

`doGet()` בודק את המשתמש לפני הצגת האפליקציה, וכל פונקציות השרת הציבוריות מוגנות גם הן באמצעות בדיקת הרשאה.

ה-Web App מוגדר:

- `executeAs: USER_ACCESSING`
- `access: ANYONE`

כך Google מזהה את החשבון שנכנס, והקוד מצמצם בפועל את הגישה לרשימת המשתמשים המורשים.

ניתן להוסיף בעתיד משתמשים נוספים בלי לשנות קוד באמצעות Script Property:

`ADDITIONAL_ALLOWED_USER_EMAILS`

אפשר להזין בו כתובות מופרדות בפסיקים, נקודה-פסיק או שורות חדשות.

הקישור הציבורי `https://rongoldner.github.io/japan26/` הוא Redirect בלבד ואינו עוקף את בקרת הגישה.

## נתונים

האפליקציה משתמשת ב-Google Sheet בשם `Japan Trip App`.

Sheets עיקריים:

- `Itinerary` — מסלול הטיול והתחנות.
- `Places` — נקודות מפה, קואורדינטות וקישורי Google Maps.
- `Expenses` — הוצאות.
- `Settings` — הגדרות, כולל שער `USD_JPY_RATE`.

### Itinerary

לכל תחנה יש `Stop ID` קבוע ו-`Stop Order` בתוך אותו תאריך.
מחיקת תחנה מבצעת מספור מחדש של התחנות באותו יום.

### Places

נשמרים שם Lat/Lng וקישור Google Maps.
מחיקת תחנה מוחקת את שורת ה-Place רק אם אין תחנה נוספת שמשתמשת באותו מקום.

## מפת הטיול

המפה הראשית משתמשת ב-Google Maps JavaScript API.

- התחנות מוצגות כסמנים ממוספרים.
- קו מחבר את נקודות הטיול לפי סדר המסלול.
- `Map` ממקד את המפה הפנימית על התחנה.
- `Google Maps` פותח את דף המקום ב-Google Maps.
- `Transit` פותח Google Maps למסלול תחבורה ציבורית אל התחנה.
- `Route` מציג בתוך האפליקציה מסלול מהתחנה הקודמת לתחנה הנוכחית.

Google Maps נטען עם `language=en` ו-`region=JP`.
ביפן Google עשויה עדיין להציג בחלק מהמקומות גם תוויות ביפנית לצד אנגלית.

## Route / Google Maps Embed

`Route` משתמש ב-Google Maps Embed API ומציג מסלול בתוך האפליקציה.

Modes:

- Transit
- Walking
- Driving
- Bicycling

ניתן גם לפתוח את אותו מסלול ב-Google Maps המלא.

## הוספת תחנה

חיפוש מקום מתבצע דרך Apps Script `Maps.newGeocoder()`.

- מינימום 3 תווים.
- Debounce לחיפוש.
- Region: Japan.
- בחירת תוצאה מציגה Preview על המפה.
- שמירה מוסיפה רשומה ל-Itinerary ול-Places.

## הוצאות

האפליקציה תומכת ב:

- הוספת הוצאה.
- עריכת הוצאה.
- מחיקת הוצאה.
- JPY / USD.
- Paid By: Ron / Tali.
- Place / Business.
- צילום או בחירת קבלה.
- Total / Ron / Tali / balance.
- תצוגת סיכום גם ב-JPY וגם בהמרה משוערת ל-USD לפי `USD_JPY_RATE`.

### קבלות

קבצי הקבלות נשמרים בתיקיית Drive הייעודית `Receipts`.

ב-Google Sheet נשמר URL בלבד.

בעת מחיקת הוצאה:
- שורת ההוצאה נמחקת.
- קובץ הקבלה ב-Google Drive **אינו נמחק**.

### צילום קבלה בטלפון

`Take receipt photo` משתמש במנגנון המצלמה המקורי של Android/Browser באמצעות:

`capture="environment"`

זו בקשה להשתמש במצלמה האחורית, אך Android הוא שקובע בפועל איזו מצלמה תיפתח.
אם נפתחת מצלמת סלפי, יש להשתמש בכפתור החלפת המצלמה של אפליקציית המצלמה.

`Choose from gallery` מאפשר לבחור תמונה קיימת.

## Mobile Help

האפליקציה מתוכננת בעיקר לשימוש בטלפון.

ליד פעולות שדורשות הסבר מופיע בטלפון כפתור `ⓘ` קטן.
לחיצה עליו פותחת חלונית הסבר קצרה בלי להפעיל את הפעולה.

במחשב קיימים גם Tooltips במעבר עכבר.

## Script Properties

המפתחות נשמרים ב-Apps Script Script Properties ואינם כתובים בקוד.

השמות הנוכחיים:

- `MAPS_JS_API_KEY`
- `EMBED_API_KEY`
- `ROUTES_API_KEY`
- `ADDITIONAL_ALLOWED_USER_EMAILS` — אופציונלי, למשתמשים מורשים נוספים.

אין לשמור את ערכי המפתחות ב-README או בקבצי המקור.

## Google Cloud

Project:

`Japan Trip Maps 2026`

APIs שהוגדרו:

- Maps JavaScript API
- Maps Embed API
- Routes API

הוגדרו API Keys נפרדים ומוגבלים לכל שירות.

## Routes API

Routes API הוגדר ונבדק.

בבדיקה Tokyo Station → Senso-ji:
- Walking ו-Driving החזירו מסלול.
- Transit לא החזיר מסלול.

לכן מסלולי Transit באפליקציה מוצגים כרגע באמצעות Maps Embed API.

## תאריכים

תצוגת התאריכים באפליקציה היא:

`dd/mm/yyyy`

Timezone של Apps Script:

`Asia/Tokyo`

## פריסה ועדכון

סדר העבודה הקבוע:

`Change → Save → Test → Deploy → Test Production → Drive backup → GitHub commit`

פרסום גרסה חדשה נעשה על אותו Deployment:

1. Save.
2. Deploy → Manage deployments.
3. Edit.
4. New version.
5. Deploy.
6. בדיקה דרך `https://rongoldner.github.io/japan26/`.
7. לאחר בדיקה מוצלחת בלבד — עדכון Drive ו-GitHub.

אם נוצר Deployment חדש וכתובת `/exec` משתנה, יש לעדכן:
- `japan26/index.html`
- `WebApp_URL.txt`
- `Japan Trip App.url`
- קישור ה-Production ב-README.

## גיבוי

קבצי המקור בתיקיית `APP` ב-Google Drive משמשים כגיבוי של גרסת ה-Production האחרונה שנבדקה:

- `Code.gs`
- `index.html`
- `appsscript.json`
- `README.md`
- `Update_Workflow.md`
- `WebApp_URL.txt`
- `Japan Trip App.url`

GitHub הפרטי:

`rongoldner/japan-trip-app`

משמש כהיסטוריית גרסאות של קוד האפליקציה.

GitHub הציבורי:

`rongoldner/japan26`

מכיל Redirect בלבד ל-Production ואינו מכיל את קוד האפליקציה.

עודכן: 23/09/2026
Displaying README.md.
