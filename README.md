## Clinical Modules (JSON Specification)

This repository contains the JSON-based clinical modules used by the mobile app
(iOS + Android). Each module follows a strict 8‑part structure that mirrors the
clinical + engineering template used across all cardiovascular topics.

Each module includes:

1. Purpose  
2. User Flow (screen-level)  
3. Input Data Model  
4. Logic Engine  
5. Output Model  
6. Crisis Rules  
7. Next-Step Routing  
8. Tags (metadata)

Modules are stored as standalone JSON files and delivered to the app via the
jsDelivr CDN.

---

## 📁 Repository Structure

```
clinical-modules/
   modules/
      htn.json
      out_of_office_bp.json
      lipids.json
      heart_failure.json
      arrhythmia.json
      ascvd.json
   schema/
      module_schema.json
   version.json
```

- `modules/` contains one JSON file per clinical module  
- `schema/` contains the JSON schema describing the 8‑part structure  
- `version.json` tracks module versions for cache invalidation  

---

## 🧩 Module Format (8-Part Template)

Each JSON file in `modules/` follows the same structure:

```
{
  "module_name": "",
  "purpose": { ... },
  "user_flow": [ ... ],
  "input_model": { ... },
  "logic_engine": { ... },
  "output_model": { ... },
  "crisis_rules": { ... },
  "next_step_routing": { ... },
  "tags": { ... }
}
```

This structure maps directly to the clinical template used internally.

---

## 🚀 Loading Modules via jsDelivr CDN

Every JSON file in this repo is automatically available through the jsDelivr CDN.

**CDN URL format:**

```
https://cdn.jsdelivr.net/gh/<username>/<repo>/<path-to-file>
```

**Example:**

```
https://cdn.jsdelivr.net/gh/keyhan/clinical-modules/modules/htn.json
```

This URL is used by the mobile app to fetch the latest module definitions.

---

## 📱 iOS Example (Swift)

```swift
let url = URL(string: "https://cdn.jsdelivr.net/gh/keyhan/clinical-modules/modules/htn.json")!
let (data, _) = try await URLSession.shared.data(from: url)
let module = try JSONDecoder().decode(Module.self, from: data)
```

---

## 🤖 Android Example (Kotlin)

```kotlin
val url = URL("https://cdn.jsdelivr.net/gh/keyhan/clinical-modules/modules/htn.json")
val json = url.readText()
val module = jsonAdapter.fromJson(json)
```

---

## 🛡️ Offline Fallback (Recommended)

Bundle a local copy of each module inside the app to ensure offline functionality.

**Swift:**

```swift
func loadLocalJSON(_ name: String) -> Data? {
    guard let path = Bundle.main.path(forResource: name, ofType: "json") else { return nil }
    return try? Data(contentsOf: URL(fileURLWithPath: path))
}
```

---

## 🔄 Updating Modules

1. Edit the JSON file in `modules/`
2. Commit and push
3. App automatically loads the new version via CDN  
   (no App Store update required)

---

## 🧪 Versioning

`version.json` tracks the current module versions and can be used by the app to
determine whether to refresh cached modules.

---

## 📜 License

Internal clinical logic modules for mobile app use.
