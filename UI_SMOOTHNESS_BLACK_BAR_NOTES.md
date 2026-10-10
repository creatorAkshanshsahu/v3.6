# AKTV v3.6 — UI smoothness and home black-bar fix

Changes made on top of the v3.5 Shaka/GeckoView project:

- Removed vertical fading-edge rendering from the home list and channel-guide lists; these edge fades can appear as dark/black bars while scrolling.
- Disabled scrollbars and overscroll effects on the primary and guide lists.
- Disabled nested scrolling for the vertical home list, horizontal channel rows, and guide lists.
- Added RecyclerView item-view caching to reduce repeated layout/bind work.
- Reused each row's TileAdapter when the same row is rebound, reducing repeated adapter creation and logo reload churn during vertical scrolling.
- Kept the existing UI design, focus navigation, channel layout, and playback implementation unchanged.

Validation: source-level changes only. An Android/TV build and visual test were not run in this environment.
