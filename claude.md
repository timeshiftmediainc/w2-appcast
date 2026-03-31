# Appcast XML Version Management

## What Are These Files?

These two XML files are **appcasts** — they tell the Flutter `upgrader` package what the latest version of each app is. When a user opens the app, it checks this file and prompts them to update if their version is outdated.

| File | App |
|---|---|
| `appcast_yoga.xml` | Yoga Anytime (Android + iOS) |
| `appcast_pilates.xml` | Pilates Anytime (Android + iOS) |

---

## ⚠️ Version Format Rule

Always use **dotted semver format** for both version fields:

```xml
sparkle:version="40.1.8"
sparkle:shortVersionString="40.1.8"
```

**Never use** plain integers like `sparkle:version="40108"` — the Flutter `upgrader` package will misparse them as `40108.0.0`.

`minimumSystemVersion` refers to the **minimum OS version** required to run the app — not the app's own version. It is set independently and only applies to **iOS items**. Android handles this via `flutter.minSdkVersion` in `build.gradle`, so it should be **omitted from Android items**.

```xml
<!-- iOS only -->
<sparkle:minimumSystemVersion>13.0</sparkle:minimumSystemVersion>
```

The current iOS minimum deployment target is **13.0**, which matches the Xcode project setting.

---

## How to Update a Version

When asking Claude to update a version, say something like:

> "Update appcast_yoga.xml — Android is now 40.2.0, iOS is now 40.2.1"

Or if both platforms have the same version:

> "Update appcast_pilates.xml — both Android and iOS are now 40.2.0"

Claude will update all of the following fields together:

1. `<title>` — the human-readable item title
2. `<pubDate>` — set to today's date in RFC 2822 format
3. `sparkle:version` — the version used for comparison by the upgrader package
4. `sparkle:shortVersionString` — the version shown to users
5. `<sparkle:minimumSystemVersion>` — **iOS items only**, the minimum iOS OS version required (currently `13.0`). Only change this if the Xcode minimum deployment target changes.

---

## Current Versions

### appcast_yoga.xml — Yoga Anytime
| Platform | Version |
|---|---|
| Android | 40.2.9 |
| iOS | 40.2.9 |

### appcast_pilates.xml — Pilates Anytime
| Platform | Version |
|---|---|
| Android | 40.1.8 |
| iOS | 40.1.9 |

**iOS minimum OS version:** `13.0` (both apps — only update if Xcode deployment target changes)

> Keep this table in sync whenever you update a version.

---

## Full File Structure Reference

```xml
<?xml version="1.0" encoding="utf-8"?>
<rss version="2.0"
    xmlns:sparkle="http://www.andymatuschak.org/xml-namespaces/sparkle">
    <channel>
        <title>App Name Updates</title>
        <description>Latest releases for App Name</description>

        <!-- App Name - Android -->
        <item>
            <title>App Name - Android {VERSION}</title>
            <pubDate>{DATE e.g. Tue, 31 Mar 2026 10:00:00 +0000}</pubDate>
            <enclosure
                url="{PLAY_STORE_URL}"
                sparkle:version="{VERSION}"
                sparkle:shortVersionString="{VERSION}"
                sparkle:os="android"
                length="0"
                type="text/html" />
        </item>

        <!-- App Name - iOS -->
        <item>
            <title>App Name - iOS {VERSION}</title>
            <pubDate>{DATE}</pubDate>
            <sparkle:minimumSystemVersion>13.0</sparkle:minimumSystemVersion>
            <enclosure
                url="{APP_STORE_URL}"
                sparkle:version="{VERSION}"
                sparkle:shortVersionString="{VERSION}"
                sparkle:os="ios"
                length="0"
                type="text/html" />
        </item>
    </channel>
</rss>
```

---

## Store URLs (do not change these)

| App | Platform | URL |
|---|---|---|
| Yoga Anytime | Android | `https://play.google.com/store/apps/details?id=com.yogaanytime.yogaanytime` |
| Yoga Anytime | iOS | `https://apps.apple.com/app/id1030917132` |
| Pilates Anytime | Android | `https://play.google.com/store/apps/details?id=com.pilatesanytime.pilatesanytime` |
| Pilates Anytime | iOS | `https://apps.apple.com/app/id646991927` |

