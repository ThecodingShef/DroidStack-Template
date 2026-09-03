# 🎯 Vibe Coding Guide — Bina Coding Jaane, Production-Ready App Banao

Ye guide un logo ke liye hai jinhe coding nahi aati, lekin apna app idea real banana hai — Claude Code ya Antigravity ki madad se, is DroidStack template ka use karke.

**Rule of thumb**: Aap **kya chahiye** batao (plain English/Hindi mein), AI agent **kaise banana hai** wo decide karega. Aapka kaam hai — clear instructions dena aur har step test karna.

---

## Step 0: Pehle Setup Kar Lo

Agar app abhi tak run nahi kiya hai, pehle [BEGINNER_GUIDE.md](BEGINNER_GUIDE.md) follow karo — Android Studio install, project clone, aur emulator/phone pe run karna.

Fir apna AI assistant kholo:
- **Claude Code**: Terminal mein project folder ke andar `claude` type karo.
- **Antigravity**: Project folder ko Antigravity mein open karo.

Dono automatically [AGENTS.md](AGENTS.md) padh lenge — ye file batati hai ki code kaise likhna hai (kahan file banani hai, hardcoded strings mana hai, etc.). Aapko ye file edit karne ki zaroorat nahi.

---

## Step 1: Apna Idea Ek Page Pe Likho

Coding shuru karne se pehle, apna app idea simple bullet points mein likho. Example:

```
App: Expense Tracker
- User apna daily kharcha add kar sake (amount, category, date, note)
- Ek list dikhe sare expenses ki, sabse naye upar
- Total kharcha is month ka dikhe upar
- User kisi bhi expense ko delete kar sake
- Categories: Food, Travel, Shopping, Bills, Other
```

Jitna specific likhoge, AI utna hi sahi banayega. Vague idea ("expense app banao") se result bhi vague aayega.

---

## Step 2: App ki Identity Set Karo (Ek Baar Ka Kaam)

Ye pehla prompt hai jo aap AI ko doge — package name aur app naam personalize karne ke liye:

```
Following AGENTS.md, personalize this template for my app:

- App name: [Aapka App Naam]
- Package name: com.yourcompany.yourappname
- Primary color: [color hex ya "blue", "purple" jo bhi chahiye]

Replace com.template.app everywhere with the new package name,
update app_name in strings.xml, and update the theme color.
```

AI folder structure, `AndroidManifest.xml`, `build.gradle.kts`, aur saare imports khud update kar dega.

**Test karo**: App ko run karo, dekho naam aur color badal gaya.

---

## Step 3: Ek-Ek Feature Banao (Sabse Zaroori Rule)

**Kabhi bhi ek prompt mein poora app mat mangwao.** Ek feature = ek prompt = ek test. Ye AGENTS.md ka bhi rule hai.

Har feature ke liye ye format use karo:

```
Following AGENTS.md, implement [feature name].

User should be able to:
- [action 1]
- [action 2]
- [action 3]

Constraints:
- Modify only files needed for this feature
- Do not refactor unrelated code
- After changes, list: files changed + how to test
```

Example (Expense Tracker ke first feature ke liye):

```
Following AGENTS.md, implement "Add Expense" screen.

User should be able to:
- Enter amount, pick a category, add a date, add an optional note
- Tap "Save" to store the expense
- See a confirmation and return to the home screen

Constraints:
- Modify only files needed for this feature
- Do not refactor unrelated code
- After changes, list: files changed + how to test
```

AI response ke end mein "Files Changed" aur "How to Test" milega — usi ko follow karke app run karo aur check karo.

**Loop yahi hai**: prompt do → run karo → check karo → agla feature.

---

## Step 4: Bug Mile Toh Aise Fix Karwao

Jab bhi kuch tootta hai ya galat dikhta hai, error/screenshot copy karke ye format use karo:

```
Following AGENTS.md, fix this error only.

Error:
[ERROR MESSAGE YAHAN PASTE KARO, YA BEHAVIOR DESCRIBE KARO]

Constraints:
- Fix only the root cause
- Do not change unrelated files
- Explain what was wrong in simple words
```

Agar error message nahi hai (jaise "button click nahi ho raha" ya "app crash ho gaya"), toh screenshot ke sath jo dikh raha hai wahi describe karo. AI khud logs/code dekh kar root cause dhoondega.

---

## Step 5: Har State Handle Ho Raha Hai, Ye Check Karo

Har screen mein 5 states honi chahiye (ye template mein already built-in hai): Loading, Success, Error, Empty, Idle. Jab naya feature banwao, ye bhi poochho:

```
Make sure this screen shows:
- A loading indicator while data loads
- The empty state when there's no data yet
- An error message with a retry button if something fails
```

Ye chhota sa addition app ko "demo" se "real app" jaisa feel deta hai.

---

## Step 6: Polish — App Ko "Finished" Dikhne Do

Sabhi features ban jaane ke baad, ye cheezein cover karo:

1. **App Icon**: Android Studio mein right-click `res` folder → New → Image Asset → apna logo/icon upload karo.
2. **Splash Screen**: `presentation/screens/splash/SplashScreen.kt` mein already setup hai — bas color/logo AI se update karwa lo.
3. **Saare strings check karo**: AI se poochho — `"Check strings.xml, make sure no text is hardcoded in any screen."`
4. **Empty screens ka copy** (jaise "No expenses yet, add your first one!") — generic "No data" jaisa na ho.

---

## Step 7: Production Ke Liye Taiyaar Karo

Ye part app ko Play Store pe daalne layak banata hai.

### 7a. App Signing (AI se manually mat karwao — ye password/keystore involve karta hai)
1. Android Studio mein: **Build → Generate Signed Bundle / APK**.
2. **Android App Bundle** select karo → Next.
3. **Create new...** click karke keystore banao — ek strong password set karo aur **safe jagah save karo** (ye kabhi bhi share nahi karna, isse app update karne ka access milta hai).
4. Release build select karo → Finish.

> ⚠️ Keystore password ya keystore file AI ko kabhi mat do — ye sirf aapke paas rehni chahiye.

### 7b. Version Number
Naya build banane se pehle AI se ye poochho:
```
Bump versionCode and versionName in app/build.gradle.kts for a new release.
```

### 7c. Release Build Test Karo
Signed bundle banne ke baad, ek real device pe install karke poora app dobara test karo (release build mein kabhi kabhi debug build se different behavior hota hai, jaise ProGuard/R8 se kuch tut sakta hai).

---

## Step 8: Play Store Pe Publish Karo

1. [Google Play Console](https://play.google.com/console) pe developer account banao (one-time $25 fee).
2. Naya app create karo, apna **App Bundle (.aab)** upload karo.
3. Store listing bharo: app description, screenshots (emulator/phone se le sakte ho), icon, feature graphic.
4. **Privacy Policy** link chahiye hoga — agar app user data collect karta hai (jaise login, location), toh ek simple privacy policy page banwao (AI se HTML page bhi bana sakte ho).
5. Content rating questionnaire fill karo.
6. Review ke liye submit karo — Google usually 1-3 din lete hain approve karne mein.

---

## Step 9: Launch Ke Baad

- User feedback/crashes track karne ke liye Firebase Crashlytics add karne ke baare mein sochein (AI se pucho, "Add Firebase Crashlytics for crash reporting" — ye AGENTS.md ke "Do Not Ever" list mein hai, toh explicitly allow karna padega).
- Naye features same Step 3 wale loop se add karte raho: idea → prompt → test.

---

## Reusable Prompt Library — Copy-Paste End-to-End

In sab prompts ko `[bracket]` waali jagah apni values daal ke seedha copy-paste kar sakte ho. Order wahi hai jaisa ek real app banate waqt use hoga.

### 1. Personalize the template (once, at the start)
```
Following AGENTS.md, personalize this template for my app:
- App name: [App Name]
- Package name: com.[yourcompany].[appname]
- Primary color: [color name or hex]

Replace com.template.app everywhere with the new package name,
update app_name in strings.xml, and update the theme color.
```

### 2. Add a new data entity (Room table)
```
Following AGENTS.md, add a new "[EntityName]" entity to the data layer.

Fields: [field1: type, field2: type, field3: type...]

Constraints:
- Follow the existing Placeholder* files as the pattern (entity, DAO, repository interface + impl)
- Wire it into AppDatabase and the Hilt modules
- Do not touch unrelated screens
- After changes, list: files changed + how to test
```

### 3. Build a new screen/feature
```
Following AGENTS.md, implement [feature name] screen.

User should be able to:
- [action 1]
- [action 2]
- [action 3]

Constraints:
- Modify only files needed for this feature
- Do not refactor unrelated code
- Handle all 4 UiState cases (Loading, Success, Error, Empty)
- Add the new screen to Screen.kt and AppNavGraph.kt
- After changes, list: files changed + how to test
```

### 4. Add navigation between two screens
```
Following AGENTS.md, add navigation from [Screen A] to [Screen B].

Trigger: [e.g. "tapping a list item", "tapping a FAB"]
Data to pass (if any): [e.g. "the item's id"]

Constraints:
- Update Screen.kt and AppNavGraph.kt only
- Do not change unrelated screens
```

### 5. Bug fix
```
Following AGENTS.md, fix this error only.

Error:
[PASTE ERROR MESSAGE, OR DESCRIBE WHAT'S WRONG]

Constraints:
- Fix only the root cause
- Do not change unrelated files
- Explain what was wrong in simple words
```

### 6. Polish a screen's states
```
Following AGENTS.md, review [screen name] and make sure it correctly
shows a loading indicator while data loads, the empty state when
there's no data, and an error message with a retry option when
something fails.
```

### 7. Clean up hardcoded strings/colors
```
Check [screen name / whole app] for any hardcoded strings or colors.
Move strings to strings.xml and colors to the theme, following
AGENTS.md's "No Hardcoded Strings" and "No Hardcoded Colors" rules.
```

### 8. Add app icon / branding
```
I've added my app icon via Android Studio's Image Asset tool. Update
the splash screen and theme colors to match this icon's color palette:
[describe colors, or attach the icon]
```

### 9. Version bump (before every release build)
```
Bump versionCode and versionName in app/build.gradle.kts for a new
release. New versionName: [e.g. "1.1.0"].
```

### 10. Add a library/feature outside the default stack (explicit opt-in)
```
Following AGENTS.md, I explicitly want to add [Retrofit / Firebase
Crashlytics / other]. Add it and wire up [specific use case], even
though it's in the "Do Not Ever" list by default.
```

### 11. Generate a privacy policy page (for Play Store listing)
```
Write a simple, plain-language privacy policy page (as HTML) for an
app called [App Name] that collects: [list what you collect, e.g.
"nothing", "email for login", "location for X feature"]. No legal
jargon, keep it short.
```

### 12. Pre-release sanity check
```
Following AGENTS.md, review the app for anything still using
placeholder/example code (like the Placeholder entity or example
screens) that should be removed or replaced before release. List
what you find, don't change anything yet.
```

**Golden rule**: Chhote steps mein kaam karo, har step test karo, aur jo cheez samajh na aaye AI se plain words mein explain karne ko kaho — koi bhi "confirm ho gaya" tab tak mat maano jab tak apni aankhon se app mein chal ke na dekh lo.
