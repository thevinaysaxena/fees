# फीस सारथी – FeeSarthi (School Fees & Vahan Fees Manager)

Ek hi `index.html` mein poori app hai: Sign up/Login, Admin-Clerk, student, fees, raseed (signature + UPI QR),
reports, class sheet (Excel/PDF), mobile-OTP suraksha, backup/restore, naya satra (Promote).

## Folder mein kya hai
| File | Kaam |
|---|---|
| `index.html` | Poori app (yahi host karni hai) |
| `firestore.rules` | Firebase database ke suraksha niyam (Publish karna zaroori) |
| `firebase.json` | Firebase Hosting + rules ki setting |
| `manifest.json`, `icon*.png`, `icon.svg` | Phone par "Home screen" icon aur naam |
| `sw.js` | Chrome mein "Install app" aur offline ke liye |

## A) Abhi turant test karna (Firebase ke bina) – LOCAL MODE
1. `index.html` ko Chrome mein kholein (computer: double-click; phone: Files app -> Open with Chrome).
2. Upar bengani patti "डेमो मोड" dikhegi. Data sirf usi Chrome mein rehta hai.
3. Naya school banayein -> Settings -> "नमूना डेटा भरें" -> sab features dekhein.
4. Demo mein OTP SMS nahi jata, screen par dikhta hai. WhatsApp photo share sirf https link par chalta hai.

## B) Asli istemal (sab phones par ek jaisa data) – Firebase
### 1. Firebase project (free, ~10 minute)
1. https://console.firebase.google.com -> **Add project**.
2. **Authentication -> Sign-in method**: **Email/Password** = Enable, **Phone** = Enable (OTP ke liye).
3. **Firestore Database -> Create database** (Production mode, region `asia-south1` Mumbai).
4. **Project settings -> Your apps -> Web (</>)** -> register -> `firebaseConfig` ki values copy karein.
5. `index.html` mein upar `window.FIREBASE_CONFIG = { ... }` mein `PASTE_...` ki jagah apni values daalein.
   (Values daalte hi app local se asli mode mein chali jaati hai.)

### 2. Rules lagayein (bahut zaroori)
Firestore -> **Rules** -> `firestore.rules` ka poora text paste karein -> **Publish**.

### 3. Website host karein (free) – koi ek
- **Firebase Hosting:** `npm i -g firebase-tools` -> `firebase login` -> `firebase use --add` -> `firebase deploy --only hosting,firestore:rules`
  Link: `https://<project-id>.web.app`
- **Netlify Drop (aasan):** https://app.netlify.com/drop par poora folder drag-drop karein.
- **GitHub Pages (phone se bhi):** repo banayein -> sab files upload -> Settings -> Pages -> main / root.
Hosting ke baad Firebase -> Authentication -> Settings -> **Authorized domains** mein apna domain jodein
(Firebase Hosting ka domain apne-aap hota hai).

### 4. Pehli baar kholein
"स्वागत है" screen par school ka naam, theme, apna naam, **mobile**, email, password dein -> aap pehle **Admin** ban jaate hain.
Settings mein UPI ID daalein. Staff do tarah se judte hain:
- Staff khud **Sign up** karein -> Admin ko Settings mein "⏳ manzoori baaki" dikhega -> Admin **Clerk** chunta hai -> tab login chalta hai.
- Ya Admin Settings -> "नया यूज़र जोड़ें" se seedha bana de.

## Roles
- **Clerk:** student jodna/edit, fees, raseed, reports, class sheet.
- **Admin:** Clerk ka sab + student delete, raseed cancel, users/approval, UPI, backup/restore, Promote.

## OTP suraksha
Student delete ya raseed cancel par Admin ke registered mobile par OTP jata hai; sahi OTP par hi kaam hota hai.
- Pehli baar OTP sahi hone par mobile us khate se jud jaata hai.
- Ye jaanch app ke andar hai. Server par bhi pakka lagana ho to Cloud Functions chahiye (alag kaam).
- Firebase SMS par seema/shulk lag sakta hai (kuch free quota, uske baad Blaze plan). Pehle Firebase ki current sharten dekh lein.
- Free testing ke liye Firebase Console -> Authentication -> Phone -> "Phone numbers for testing" mein test number daal sakte hain.

## Pehle din ka test (10 minute)
1. Admin se login -> student jodein -> fees jama (signature ke saath) -> raseed no. wahi jo aapne setup mein chuna (jaise `1000`).
2. Staff ko Sign up karwayein -> Admin se approve karein -> doosre phone par login, wahi student dikhe.
3. Clerk ko Settings aur delete button na dikhe.
4. Student delete / raseed cancel par OTP aaye.
5. Class sheet (Print / Save PDF / Excel), reports, backup dekhein.
Koi step fail ho to error ka screenshot bhejein.

## Jaanne layak baatein
- Raseed kaatne ke liye internet zaroori hai (taaki raseed number kabhi dohraye nahi). Baaki kaam thoda offline bhi chalta hai.
- Bakaya = chaalu mahine tak ka. Purane satra ka bakaya naye satra mein apne-aap nahi jaata.
- Pehle ka local data naye Firebase mein lana ho: local Settings -> Backup se text copy -> asli app Settings -> Restore.
- Hafte mein ek baar Backup copy karke WhatsApp/Drive mein rakhein.

## APK (Android app) chahiye?
`feesarthi-android-apk.zip` alag se diya gaya hai. Online "website to APK" converters mein Save PDF / Download / WhatsApp
buttons nahi chalte; wo project in buttons ke liye native bridge ke saath hai.


## Raseed number aur signature
- Pehli baar setup karte waqt chunein ki raseed number kahan se shuru ho (jaise 1000 -> 1000, 1001, 1002 ...). Baad mein Admin Settings -> "Receipt Number" se agla number badal sakta hai (purane number se peeche nahi ja sakta).
- Teen mahine ek saath jama karne par ek hi raseed banti hai, aur wahi number teeno mahine mein dikhta hai.
- Raseed page par teen option: WhatsApp (photo ke saath), PDF download, raseed (photo) download.
- Fees jama karte waqt ungli se sign sahi na aaye to "hastakshar ki photo upload karein" dabayein: safed kagaz par pen se sign ki photo chunein; background apne-aap hat jata hai aur wahi har raseed par lagta hai.

- **Fee ke mahine:** school banate waqt chunein kitne mahine ki fees lagti hai (12, ya 11 jun chhodkar, ya apne hisaab se mahine chunein). Sirf wahi mahine fee table, Add Fees, class sheet aur bakaya mein aate hain. Baad mein Settings -> "Fee Months" se badal sakte hain (jis mahine ki raseed ban chuki ho use hata nahi sakte).

- **Login / Sign up (naya, saaf tareeka):** pehle ek simple page aata hai. Naya school ho to "Naya school shuru karein" (3 chhote kadam); school pehle se ho to "Login karein" ya "Naya khata banayein" (staff). Sign up ke baad Admin ki manzoori chahiye.

- **Logo:** app ke logo mein Maa Saraswati ki tasveer hai (veena, lotus aur halo ke saath). Apne school ka logo lagana ho to school banate waqt (kadam 1) ya Settings -> "School ka logo" se photo chunein; wapas Saraswati logo bhi laga sakte hain.


- **Install app:** hosted https link ko Chrome mein kholkar menu -> "Install app". Isse PDF, download, photo upload aur share sab chalte hain.

- **Kaksha (class) range:** school banate waqt chunein ki kaksha 1 se kaha tak hai (1 se 12 mein koi bhi). Settings -> "Classes" se baad mein badal sakte hain (jis kaksha mein bacche hain usse kam nahi kar sakte).
- **Profile:** Dashboard par "Profile" button (sabke liye) aur Settings ke upar bhi profile dikhti hai.

- **Phone number par tap:** bacche ke number par dabate hi "Call" ya "WhatsApp" ka option aata hai; jo chunein wahi hota hai.
- **Paise ke rang:** deya/fee = neela, jama = hara, bakaya = laal, chhoot = baingani, aaj ka sangrah = narangi (taaki ek nazar mein samajh aaye).
