# Usage counts and how to read them

The app keeps one Firestore document per day under `usage/{YYYY-MM-DD}` with only counters:

| field | meaning |
|---|---|
| `devices` | devices that opened the app that day (each device once) |
| `opens` | app opens and returns to the app |
| `starts`, `startsByLevel.{level}` | boards started |
| `wins`, `winsByLevel.{level}` | boards finished |
| `families` | hashed family codes active that day (the code itself is never sent) |

No names, boards or device identifiers are sent. Counts queue on the device and flush when online.

## Firestore rules

Add this inside `match /databases/{database}/documents { … }` next to the `families` rule, then Publish:

```
match /usage/{day} {
  allow read: if true;
  allow create, update: if day.matches('^20[0-9]{2}-[0-9]{2}-[0-9]{2}$')
    && request.resource.data.keys().hasOnly(['day', 'devices', 'opens', 'starts', 'wins', 'startsByLevel', 'winsByLevel', 'families'])
    && (!('families' in request.resource.data) || request.resource.data.families.size() <= 2000);
  allow delete: if false;
}
```

Problem reports go to `reports/{id}`. Add this rule as well:

```
match /reports/{id} {
  allow read: if true;
  allow create: if request.resource.data.keys().hasOnly(['at', 'build', 'text', 'context', 'errors'])
    && request.resource.data.text is string && request.resource.data.text.size() <= 1500
    && request.resource.data.context is string && request.resource.data.context.size() <= 6000
    && request.resource.data.errors is string && request.resource.data.errors.size() <= 2500;
  allow update, delete: if false;
}
```

## If a command prints `jq: parse error`

The body was not JSON, so look at it raw first:

```bash
curl -s -X POST \
  'https://firestore.googleapis.com/v1/projects/playmaths-6a181/databases/(default)/documents:runQuery?key=AIzaSyCgG83-cqWqDuzOWrIQwqLBMF7sTwOkbWA' \
  -H 'Content-Type: application/json' \
  -d '{"structuredQuery":{"from":[{"collectionId":"usage"}],"limit":3}}' | head -c 600; echo
```

A JSON array starting `[{"document"` means data; `[{"readTime"` alone means the collection is empty so far; `{"error"` with PERMISSION_DENIED means the usage rule is not published yet. Anything else (HTML, a line of text) is a network or shell problem: paste the URL in single quotes exactly as above, because the `(default)` and `?key=` parts break without the quotes.

## Reading the counts

**Problem reports** sent from the app (menu → Report a problem) appear on the same Usage page with an Open this board link, and in the console under `reports`.

**In the app**: open Tools & settings from the menu and tap the build stamp five times. A Usage row appears in the menu with tiles, a 30-day chart and a per-day table. Five more taps hide it again.

**In the console**: https://console.firebase.google.com → project `playmaths-6a181` → Firestore Database → collection `usage`. Each document is one day.

**From a terminal** (the API key is the public web key already in the app):

Last 30 days, newest first:

```bash
curl -s -X POST \
  'https://firestore.googleapis.com/v1/projects/playmaths-6a181/databases/(default)/documents:runQuery?key=AIzaSyCgG83-cqWqDuzOWrIQwqLBMF7sTwOkbWA' \
  -H 'Content-Type: application/json' \
  -d '{"structuredQuery":{"from":[{"collectionId":"usage"}],"orderBy":[{"field":{"fieldPath":"__name__"},"direction":"DESCENDING"}],"limit":30}}' \
  | jq -r '.[] | .document.fields | "\(.day.stringValue)  devices=\(.devices.integerValue // 0)  opens=\(.opens.integerValue // 0)  started=\(.starts.integerValue // 0)  finished=\(.wins.integerValue // 0)"'
```

One day:

```bash
curl -s 'https://firestore.googleapis.com/v1/projects/playmaths-6a181/databases/(default)/documents/usage/2026-11-11?key=AIzaSyCgG83-cqWqDuzOWrIQwqLBMF7sTwOkbWA' | jq .fields
```

Family activity (who synced and when), one line per family code:

```bash
curl -s 'https://firestore.googleapis.com/v1/projects/playmaths-6a181/databases/(default)/documents/families?pageSize=100&mask.fieldPaths=savedAt&key=AIzaSyCgG83-cqWqDuzOWrIQwqLBMF7sTwOkbWA' \
  | jq -r '.documents[] | "\(.name | split("/") | last)  last saved \(.fields.savedAt.integerValue | tonumber / 1000 | todate)"'
```

Games per player inside one family (replace the code):

```bash
curl -s 'https://firestore.googleapis.com/v1/projects/playmaths-6a181/databases/(default)/documents/families/K7PM-3QX2?key=AIzaSyCgG83-cqWqDuzOWrIQwqLBMF7sTwOkbWA' \
  | jq -r '.fields.data.stringValue | fromjson | .profiles[] | select(.removed != true) | "\(.icon) \(.name): \(.history | length) games, last \(.history | map(.date) | max // "none")"'
```
