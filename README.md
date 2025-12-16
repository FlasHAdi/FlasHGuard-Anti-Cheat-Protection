# FlasHGuard - Anti-Cheat Protection System

## 🇬🇧 English Documentation

### Overview
FlasHGuard is a comprehensive anti-cheat and game protection system designed to prevent unauthorized modifications, debugging, and cheating in the game client. It implements multiple layers of security checks and monitoring systems.

### Features

#### 1. **Access Control & Authority Management**
- Sets maximum authority privileges for process protection
- Prevents unauthorized process access and memory manipulation
- Blocks external processes from reading or writing to game memory
- Implements WriteProcessMemory hooks to detect injection attempts

#### 2. **Anti-Debugging Protection**
- Detects if a debugger is attached to the process
- Monitors PEB (Process Environment Block) for debugging flags
- Checks NtGlobalFlag for heap debugging indicators
- Disables debug privileges to prevent debugging tools
- Performs timing checks to detect debugger-induced delays

#### 3. **Hook Detection & Prevention**
- Monitors critical game functions for hooks:
  - `CEterPackManager::Get`
  - `CEterPackManager::GetFromPack`
  - `CPythonApplication::Process`
  - `CPythonApplication::UpdateGame`
  - `CPythonApplication::RenderGame`
- Detects various hooking techniques:
  - JMP hooks (0xE9)
  - RET hooks (0xC3)
  - INT3 breakpoints (0xCC)
  - PUSH/CALL redirections
  - Memory protection violations
- Prevents Python runtime hooks (PyRun_SimpleFile, PyRun_SimpleString)
- Runtime hook scanning every 5 seconds

#### 4. **File Integrity Verification**
- Validates client files in the game directory
- Whitelist system for legitimate files
- Detects unknown or suspicious files
- Blocks files with suspicious naming patterns
- Runtime file checking every 6 seconds

#### 5. **Window & Process Name Monitoring**
- Scans for blacklisted cheat programs and tools
- Detects suspicious window titles and process names
- Identifies pattern-based suspicious naming
- Compares window titles with actual process names
- Checks for special character abuse in names
- Whitelist system for trusted applications
- Runtime scanning every 7 seconds

#### 6. **DLL Integrity Checks**
- Validates critical DLL files:
  - `python27.dll` - Expected size: 2,512,896 bytes
  - `devil.dll` - Expected size: 269,312 bytes
- MD5 hash verification:
  - `python27.dll`: `da8b71b282bb2c3e0ac3e0465e592e5d`
  - `devil.dll`: `8df4d4324e5755f1a0567db3c5be4c58`
- Prevents DLL injection attacks
- Detects M2Bob (botting tool)
- Checks for memory breaking tools
- Runtime DLL checking every 10 seconds

#### 7. **Memory Protection (CShield)**
- Monitors thread integrity
- Validates thread start addresses
- Checks memory allocation types
- Detects suspicious memory regions
- Protects against memory manipulation

#### 8. **Logging & Reporting**
- Creates detailed logs in `logs/FlasHGuard.txt`
- Discord webhook integration for real-time alerts
- Context information in logs:
  - Player name
  - Computer name
  - Error type and details
<img width="791" height="443" alt="image" src="https://github.com/user-attachments/assets/67fedbe4-231e-497d-87ad-553c4e62359b" />

### Configuration Options

```cpp
#define ENABLE_DEBUG_MODE FALSE              // Enable/disable debug mode (recommended: FALSE)
#define SET_ACCESS_AUTHORITY TRUE            // Set maximum process authority
#define SET_DEBUG_BLOCKERS TRUE              // Enable anti-debugging
#define SET_HOOK_BLOCKERS TRUE               // Enable hook detection
#define SET_HOOK_BLOCKERS_RUNTIME TRUE       // Enable runtime hook scanning
#define SET_HOOK_BLOCKERS_TIMER 5            // Hook check interval (seconds)
#define CHECK_CLIENT_FILES TRUE              // Enable file integrity checks
#define CHECK_CLIENT_FILES_RUNTIME TRUE      // Enable runtime file checking
#define CHECK_CLIENT_FILES_TIMER 6           // File check interval (seconds)
#define CHECK_WINDOW_PROCESS_NAMES TRUE      // Enable process name monitoring
#define CHECK_WINDOW_PROCESS_NAMES_RUNTIME TRUE  // Enable runtime process scanning
#define CHECK_WINDOW_PROCESS_NAMES_TIMER 7   // Process check interval (seconds)
#define CHECK_DLL_FILES TRUE                 // Enable DLL integrity checks
#define CHECK_DLL_FILES_RUNTIME TRUE         // Enable runtime DLL checking
#define CHECK_DLL_FILES_TIMER 10             // DLL check interval (seconds)
```

### Blacklisted Programs
The system detects and blocks:
- Cheat Engine and variants
- Damage meters and hack tools
- Macro programs (TinyTask, etc.)
- Farm bots and automation tools
- Injection tools
- Memory editors
- Python loaders
- KinMaster, CulMaster and similar cheating tools

### Whitelisted Applications
Trusted programs that won't trigger detection:
- Discord
- Notepad
- Adobe products
- Visual Studio
- Compression tools (WinRAR, 7-Zip)
- EasyAntiCheat components
- SelfishNet (traffic shaper)

### Error Codes
The system uses specific error codes for different detection types:
- `SET_ACCESS_AUTHORITY_FAILED_X`: Authority setting failures
- `FAILED_TO_SET_DEBUG_BLOCKERS_X`: Debug blocker failures
- `HOOK_DETECTED_X`: Hook detection alerts
- `UNKNOWN_FILE_DETECTED_X`: Suspicious file detection
- `UNKNOWN_WINDOW_PROCESS_NAME_DETECTED_X`: Suspicious process detection
- `DLL_CHECK_FAILED_X`: DLL integrity failures
- `CSHIELD_HANDLER_ERROR_X`: Memory protection errors

### How to Use
1. Compile with `FH_ENABLE_GUARD` defined
2. Ensure `flashguard.jpg` exists in the game directory
3. The system initializes automatically via `MetaInitialize()`
4. All checks run in background threads
5. Violations result in immediate game termination

### Technical Details
- Uses Windows API for process and memory inspection
- Implements GDI+ for protection image display
- SHA256 hashing for memory integrity
- Multi-threaded monitoring system
- Assembly-level anti-debugging techniques

---

## 🇷🇴 Documentație în Română

### Prezentare Generală
FlasHGuard este un sistem comprehensiv de protecție anti-cheat conceput pentru a preveni modificările neautorizate, debugging-ul și cheat-urile în clientul de joc. Implementează multiple niveluri de verificări de securitate și sisteme de monitorizare.

### Funcționalități

#### 1. **Control Acces & Gestionare Autoritate**
- Stabilește privilegii maxime de autoritate pentru protecția procesului
- Previne accesul neautorizat la proces și manipularea memoriei
- Blochează procesele externe de la citirea sau scrierea în memoria jocului
- Implementează hook-uri WriteProcessMemory pentru detectarea tentativelor de injectare

#### 2. **Protecție Anti-Debugging**
- Detectează dacă un debugger este atașat la proces
- Monitorizează PEB (Process Environment Block) pentru flag-uri de debugging
- Verifică NtGlobalFlag pentru indicatori de debugging heap
- Dezactivează privilegiile de debug pentru a preveni tool-urile de debugging
- Efectuează verificări de timing pentru a detecta întârzierile cauzate de debugger

#### 3. **Detectare & Prevenire Hook-uri**
- Monitorizează funcțiile critice ale jocului pentru hook-uri:
  - `CEterPackManager::Get`
  - `CEterPackManager::GetFromPack`
  - `CPythonApplication::Process`
  - `CPythonApplication::UpdateGame`
  - `CPythonApplication::RenderGame`
- Detectează diverse tehnici de hooking:
  - Hook-uri JMP (0xE9)
  - Hook-uri RET (0xC3)
  - Breakpoint-uri INT3 (0xCC)
  - Redirecționări PUSH/CALL
  - Violări ale protecției memoriei
- Previne hook-urile Python runtime (PyRun_SimpleFile, PyRun_SimpleString)
- Scanare hook-uri în timp real la fiecare 5 secunde

#### 4. **Verificare Integritate Fișiere**
- Validează fișierele clientului în directorul jocului
- Sistem de whitelist pentru fișiere legitime
- Detectează fișiere necunoscute sau suspecte
- Blochează fișiere cu modele de denumire suspicioase
- Verificare fișiere în timp real la fiecare 6 secunde

#### 5. **Monitorizare Nume Ferestre & Procese**
- Scanează pentru programe și tool-uri de cheat din lista neagră
- Detectează titluri de ferestre și nume de procese suspicioase
- Identifică denumiri suspicioase bazate pe pattern-uri
- Compară titlurile ferestrelor cu numele reale ale proceselor
- Verifică abuzul de caractere speciale în nume
- Sistem de whitelist pentru aplicații de încredere
- Scanare în timp real la fiecare 7 secunde

#### 6. **Verificări Integritate DLL**
- Validează fișiere DLL critice:
  - `python27.dll` - Dimensiune așteptată: 2,512,896 bytes
  - `devil.dll` - Dimensiune așteptată: 269,312 bytes
- Verificare hash MD5:
  - `python27.dll`: `da8b71b282bb2c3e0ac3e0465e592e5d`
  - `devil.dll`: `8df4d4324e5755f1a0567db3c5be4c58`
- Previne atacuri de injectare DLL
- Detectează M2Bob (tool de botting)
- Verifică pentru tool-uri de memory breaking
- Verificare DLL în timp real la fiecare 10 secunde

#### 7. **Protecție Memorie (CShield)**
- Monitorizează integritatea thread-urilor
- Validează adresele de start ale thread-urilor
- Verifică tipurile de alocare a memoriei
- Detectează regiuni de memorie suspicioase
- Protejează împotriva manipulării memoriei

#### 8. **Logging & Raportare**
- Creează log-uri detaliate în `logs/FlasHGuard.txt`
- Integrare webhook Discord pentru alerte în timp real
- Informații de context în log-uri:
  - Numele jucătorului
  - Numele computerului
  - Tipul de eroare și detalii
<img width="791" height="443" alt="image" src="https://github.com/user-attachments/assets/67fedbe4-231e-497d-87ad-553c4e62359b" />

### Opțiuni de Configurare

```cpp
#define ENABLE_DEBUG_MODE FALSE              // Activează/dezactivează modul debug (recomandat: FALSE)
#define SET_ACCESS_AUTHORITY TRUE            // Setează autoritate maximă pentru proces
#define SET_DEBUG_BLOCKERS TRUE              // Activează anti-debugging
#define SET_HOOK_BLOCKERS TRUE               // Activează detectarea hook-urilor
#define SET_HOOK_BLOCKERS_RUNTIME TRUE       // Activează scanarea hook-urilor în timp real
#define SET_HOOK_BLOCKERS_TIMER 5            // Interval verificare hook-uri (secunde)
#define CHECK_CLIENT_FILES TRUE              // Activează verificările de integritate fișiere
#define CHECK_CLIENT_FILES_RUNTIME TRUE      // Activează verificarea fișierelor în timp real
#define CHECK_CLIENT_FILES_TIMER 6           // Interval verificare fișiere (secunde)
#define CHECK_WINDOW_PROCESS_NAMES TRUE      // Activează monitorizarea numelor de procese
#define CHECK_WINDOW_PROCESS_NAMES_RUNTIME TRUE  // Activează scanarea proceselor în timp real
#define CHECK_WINDOW_PROCESS_NAMES_TIMER 7   // Interval verificare procese (secunde)
#define CHECK_DLL_FILES TRUE                 // Activează verificările de integritate DLL
#define CHECK_DLL_FILES_RUNTIME TRUE         // Activează verificarea DLL-urilor în timp real
#define CHECK_DLL_FILES_TIMER 10             // Interval verificare DLL (secunde)
```

### Programe din Lista Neagră
Sistemul detectează și blochează:
- Cheat Engine și variante
- Damage meters și tool-uri de hack
- Programe macro (TinyTask, etc.)
- Farm bots și tool-uri de automatizare
- Tool-uri de injectare
- Editoare de memorie
- Python loaders
- KinMaster, CulMaster și tool-uri similare de cheat

### Aplicații din Lista Albă
Programe de încredere care nu vor declanșa detectarea:
- Discord
- Notepad
- Produse Adobe
- Visual Studio
- Tool-uri de compresie (WinRAR, 7-Zip)
- Componente EasyAntiCheat
- SelfishNet (traffic shaper)

### Coduri de Eroare
Sistemul folosește coduri de eroare specifice pentru diferite tipuri de detectare:
- `SET_ACCESS_AUTHORITY_FAILED_X`: Eșecuri la setarea autorității
- `FAILED_TO_SET_DEBUG_BLOCKERS_X`: Eșecuri la blocarea debug-ului
- `HOOK_DETECTED_X`: Alerte de detectare hook-uri
- `UNKNOWN_FILE_DETECTED_X`: Detectare fișiere suspicioase
- `UNKNOWN_WINDOW_PROCESS_NAME_DETECTED_X`: Detectare procese suspicioase
- `DLL_CHECK_FAILED_X`: Eșecuri de integritate DLL
- `CSHIELD_HANDLER_ERROR_X`: Erori de protecție memorie

### Cum se Folosește
1. Compilează cu `FH_ENABLE_GUARD` definit
2. Asigură-te că `flashguard.jpg` există în directorul jocului
3. Sistemul se inițializează automat prin `MetaInitialize()`
4. Toate verificările rulează în thread-uri de fundal
5. Violările rezultă în terminarea imediată a jocului

### Detalii Tehnice
- Folosește Windows API pentru inspecția proceselor și memoriei
- Implementează GDI+ pentru afișarea imaginii de protecție
- Hashing SHA256 pentru integritatea memoriei
- Sistem de monitorizare multi-threaded
- Tehnici anti-debugging la nivel de assembly

---

## System Requirements / Cerințe de Sistem
- Windows 7 or higher / Windows 7 sau mai nou
- Visual Studio 2017 or higher / Visual Studio 2017 sau mai nou
- Administrator privileges / Privilegii de administrator

## Notes / Note
- For production use, always set `ENABLE_DEBUG_MODE` to `FALSE`
- The system terminates the game immediately upon detecting violations
- All checks run in separate threads to minimize performance impact
- Discord webhook integration requires proper configuration

- Pentru utilizare în producție, setează întotdeauna `ENABLE_DEBUG_MODE` la `FALSE`
- Sistemul termină jocul imediat la detectarea violărilor
- Toate verificările rulează în thread-uri separate pentru a minimiza impactul asupra performanței
- Integrarea webhook Discord necesită configurare adecvată
