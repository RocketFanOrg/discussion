# 🚀 RocketFan 🚀

## Project Overview

Potentially a tab bar application with the following views:

- [ ] Launches
- [ ] Missions
- [ ] Info: vehicles, pads, acronyms
- [ ] Settings:

    *App Settings*
    - [ ] Distance
    - [ ] Use Dark Theme

    *About*
    - [ ] Company Info
    - [ ] API Info
    - [ ] Source Link
    - [ ] Contributors
    - [ ] Report issue?

### Tooling

- [ ] [Bitrise](https://www.bitrise.io): continuous integration
- [ ] [Danger](https://danger.systems/swift/): for automatically checking rules for pull requests
- [ ] [SwiftFormat](https://github.com/nicklockwood/SwiftFormat): automatically format code in pull requests, so contributors don't have to be concerned if their style doesn't match
- [ ] [SwiftLint](https://github.com/realm/SwiftLint): for linting code to ensure it fits the style guide
- [ ] [Slack Workspace](https://rocketfan.slack.com/): for easier discussions


### Project Setup

- Organise by features. See [Unwrap App](https://github.com/twostraws/Unwrap).
- Use Coordinators
- API should go in a static module
- Favour [Dependency Injection](https://www.swiftbysundell.com/posts/different-flavors-of-dependency-injection-in-swift)
- Encourage testing. Decodable models are *required* to be tested, controllers ideally should be, views will not be. Can always do snapshot tests later
- Avoid 3rd party dependencies
- CONTRIBUTING.md
- [Changelog](https://keepachangelog.com/en/1.0.0/) - initially keep to major changes.
- Semantic versioning
- Pull Request early - as soon as you make your first commit, open a pull request, as this lets others see what is being worked on

## Endpoints

### Documentation

- [SpaceX API](https://docs.spacexdata.com)

### Capsules

Available options are: all, one, upcoming, past

### Cores

Available options are: all, one, upcoming, past

### Dragons

Available options are: all, one

### History

Available options are: all, one

### Info

Available options are: company info, API info

### Landing Pads

Available options are: all, one

### Launches

Available options are: all, one, past, upcoming, latest, next. Of particular interest is the `timeline` dictionary. Would be nice to display a timeline in the local time?

### Launch Pads

Available options are: all, one

### Missions

Available options are: all, one

### Payloads

Available options are: all, one

### Rockets

Available options are: all, one

### Roadster

The current information for the roadster

### Ships

Available options are: all, one

## Metadata

### Locations

Locations are given with latitude and longitude values.  Use [`MKMapView`](https://developer.apple.com/documentation/mapkit/mkmapview) available in [MapKit](https://developer.apple.com/documentation/mapkit).

### Dates

It seems dates are given as [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601#Dates). Launches have additional dates:

```
"launch_date_unix": 1532238600,
"launch_date_utc": "2018-07-22T05:50:00.000Z",
"launch_date_local": "2018-07-22T01:50:00-04:00",
```

### Acronyms

The API is full of acronyms, which unless you're familiar are fairly inaccessible. Helpfully there is an acronyms wiki on [Reddit](https://www.reddit.com/r/spacex/wiki/acronyms). I suggest we implement this into the app. Perhaps like so:

```swift
extension String {
    internal func expandAcronym() -> String {
        switch self {
        case "AOS": return "Acquisition of Signal"
        case "ACS": return "Altitude Control System"
        default: return self
        }
    }
}

let test = "AOS".expandAcronym() // Aquisition of Signal
```

Some of the acronyms have descriptions, so maybe a better option would be something that follows this signature:

```swift
struct Acronym {
  let compact: String      // AOS
  let expanded: String     // Acquisition of Signal
  let description: String? // A phrase used during launch when communication with the vehicle is re-estabilished after Loss of Signal
}
```

This would be an ideal candidate for an `Acronyms.json` file, ideally with a `AcronymFactory` struct that loads the JSON and looks for a key in the JSON.

These acronyms could be displayed in their own view controller, ideally with a `UISearchBar` to quickly filter. It would also be nice to force touch on an acronym in any view to be presented with the acronym description.

### Images

Many endpoints contain images. Present thumbnails using a collection view, and tap to expand.

### Videos

Hosted on YouTube, could be presented in a [`WKWebView`](https://developer.apple.com/documentation/webkit/wkwebview), provided by [WebKit](https://developer.apple.com/documentation/webkit/wkwebview).

### Links

Links should open in [`SFSafariViewController`](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller), provided by [SafariServices](https://developer.apple.com/documentation/safariservices).

### Errors

Errors seem to be returned as so:

```
{
    "error": "Not Found"
}
```

A good idea would be for the error to be bubbled up to the coordinator, where it can decide how to handle it, for example, by presenting a toast notification.

## Settings

### Distances

Distances are available in miles and kilometers, this could be user settable, or automatic with:

```swift
NSLocale.current.usesMetricSystem ? "km" : "mi"
```

