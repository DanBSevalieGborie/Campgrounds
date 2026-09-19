# Campground Explorer

**AND102 — Intermediate Android Development, Unit 4 Lab**

An Android app that fetches campgrounds from the National Park Service API, lists them in
a RecyclerView with their photo, name, location and description, and opens a detail screen
for the campground that was tapped.

## Required Features

The following **required** functionality is completed:

- [x] **Campgrounds are displayed using the RecyclerView**
- [x] **Can navigate to the Campground Details screen**
- [x] **Campground images are downloaded and displayed using Glide**

The following **stretch** functionality is completed:

- [x] **View elements are styled in the `.xml` files**

The following **additional** features are implemented:

- [x] Rows are Material cards with rounded corners, elevation and a touch ripple
- [x] Full light and dark theme support, driven by a shared set of semantic colors
- [x] The detail screen scrolls, so long NPS descriptions are readable in full
- [x] Fixed a starter bug where the dark theme was named `Theme.ArticleSearch` and never applied

## Video Walkthrough

Here's a walkthrough of implemented features:

<img src='walkthrough.gif' title='Video Walkthrough' width='' alt='Video Walkthrough' />

<!--
  TODO: Record your own walkthrough and drop the file in this project as `walkthrough.gif`.

  Record it:
    - Android Studio -> Running Devices window -> the camera/record icon, or
    - `adb shell screenrecord /sdcard/demo.mp4` then `adb pull /sdcard/demo.mp4`

  Convert it to a GIF with Kap (macOS), ScreenToGif (Windows), LiceCap, or ezgif.com.

  Show: the list of campgrounds loading, scrolling through a few rows, tapping one,
  and the detail screen with its image, name, lat/long and description.
-->

GIF created with [Kap](https://getkap.co/) / [ScreenToGif](https://www.screentogif.com/).

## Setup

1. Open `apikey.properties` in the project root and replace the placeholder with your own
   National Park Service API key:

   ```
   API_KEY = <YOUR API KEY HERE>
   ```

   Get a key at https://www.nps.gov/subjects/developer/get-started.htm

2. In Android Studio: `Build` -> `Clean Project`, then `Build` -> `Assemble Project`. The
   key is read from `apikey.properties` at build time into `BuildConfig.API_KEY`, so the
   project has to be rebuilt after you change it.

3. Run on an emulator or device with an internet connection.

`apikey.properties` is listed in `.gitignore` so your key does not get pushed to GitHub.

## What was implemented

| File | What it does |
| --- | --- |
| `Campground.kt` | `CampgroundResponse` (maps the top-level `data` array), `Campground` (`name`, `description`, `latLong`, `images`, plus the `imageUrl` convenience property), and `CampgroundImage` (`url`, `title`) |
| `CampgroundAdapter.kt` | Takes the campgrounds list, holds the row's views, binds name/description/latLong, loads the photo with Glide, and on tap builds an `Intent` to `DetailActivity` carrying the campground |
| `MainActivity.kt` | Holds the campgrounds list, wires up the adapter, decodes the JSON response with Kotlin Serialization, appends the results, and calls `notifyDataSetChanged()` |
| `DetailActivity.kt` | Finds its views, reads the campground out of the Intent with `getSerializableExtra`, fills in name/description/latLong, and loads the image with Glide |

### Styling (stretch feature)

No sizes, colors or text appearances are hard-coded in the layouts — each view points at a
named resource, so the whole look can be changed from one place.

| File | What it holds |
| --- | --- |
| `values/colors.xml` | The brand palette (forest green, sand) plus *semantic* names — `app_background`, `card_surface`, `text_primary`, `text_secondary`, `text_accent`, `image_placeholder` — which are what the layouts actually reference |
| `values-night/colors.xml` | Dark-mode values for those same semantic names, so every screen follows the system theme with no layout changes |
| `values/dimens.xml` | Spacing scale (`spacing_xs` … `spacing_xl`), card corner radius and elevation, image heights, and the type scale |
| `values/styles.xml` | `TextAppearance.Campgrounds.*` for the title / location / body text, and `Widget.Campgrounds.*` for the card, images and RecyclerView |
| `values/themes.xml`, `values-night/themes.xml` | `Theme.Campgrounds` in light and dark, wired to the palette above |
| `values/strings.xml` | Content descriptions and layout-preview placeholder text, extracted from the layouts |

The layouts were reworked on top of that: `item_campground.xml` is now a
`MaterialCardView` with a ripple, and `activity_detail.xml` is wrapped in a `ScrollView`
with a full-width hero image. Because the rows are cards with their own margins, the
starter's `DividerItemDecoration` was removed from `MainActivity`.

## Notes

The starter's `Campground.kt` imported `android.support.annotation.Keep`, which is the old
support-library package and does not resolve in an AndroidX project (this project sets
`android.useAndroidX=true` with no Jetifier). It has been changed to the AndroidX
equivalent, `androidx.annotation.Keep`, which does the same thing.

## License

    Copyright 2026 Dan Sevalie Gborie

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
