# Whistle Time Security Audit

This repository contains the results and methodology of a comprehensive **static** and **dynamic** security analysis of the Whistle Time Android app.

## 📂 Repository Structure

  ```sql
/
├── README.md ← This file
├── apk-analysis/
│ ├── wt-apktool/ ← Decompiled APK (apktool output)
│ │ ├── AndroidManifest.xml
│ │ ├── res/
│ │ ├── smali/
│ │ └── lib/
│ └── wt-jadx/ ← Decompiled Java/Smali (jadx output)
├── report/
│ ├── screenshots/ ← All annotated screenshots
│ └── analysis-report.md ← Full audit report in Markdown
└── pentest-report.pdf ← Dynamic testing (Burp Suite) report
```

pgsql
Copiar
Editar

## 🔧 How to Reproduce

1. **Build or obtain the release APK**  
   ```bash
   flutter build apk --release
   cp build/app/outputs/flutter-apk/app-release.apk WhistleTime-release.apk
Static decompilation with apktool

apktool d WhistleTime-release.apk -o apk-analysis/wt-apktool
Java/Smali extraction with jadx

jadx -d apk-analysis/wt-jadx WhistleTime-release.apk
Search for sensitive strings

grep -R "SharedPreferences" -n apk-analysis/wt-jadx
grep -R "AIza"             -n apk-analysis/wt-apktool/res/values/strings.xml
Extract Flutter AOT literals

strings apk-analysis/wt-apktool/lib/arm64-v8a/libapp.so \
  | grep "https://"
📑 Audit Report
See report/analysis-report.md for:

1. Permissions review & risks

2. Exported components (activities, services, receivers)

3. Providers & FileProviders

4. Network security config & cleartext policy

5. Firebase config (google_api_key, etc.)

6. SharedPreferences usage & recommendations

7. Hardcoded endpoints & keys

8. Dynamic validation of Cloud Run endpoint

9. Summary & next steps

✅ Key Findings & Recommendations
Permissions: RECEIVE_BOOT_COMPLETED may be unnecessary—remove if not used.

Network config: Move dev-login.miapi.com cleartext exception to debug only.

Firebase keys: Restrict in Google Cloud Console.

SharedPreferences: Migrate sensitive storage to encrypted or secure storage.

Endpoint: Protect https://chattdp-service-.../generate with auth & rate limits.

Flutter layer: Obfuscate or dynamically load production URLs.

📖 Pentest
Dynamic testing details and findings are in pentest-report.pdf (Burp Suite).

Feel free to customize, extend, or re-run any step. Contributions and feedback welcome! 🚀
