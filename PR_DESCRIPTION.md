**Changes**
Fix dummy chapter handling for videos with a single chapter in `FFProbeVideoInfo.cs`:

1. Change the condition that triggers dummy chapter creation from `chapters.Length == 0` to `chapters.Length <= 1`, so that videos with only one chapter (spanning the entire duration) also attempt dummy chapter generation.
2. Add an early return in `CreateDummyChapters` when the calculated `chapterCount <= 1`. Previously, if a video's runtime was only slightly longer than `DummyChapterDuration` (e.g. 11 min runtime with 10 min interval), integer division produced `chapterCount = 1`, resulting in a single dummy chapter. This single chapter then caused `GetAverageDurationBetweenChapters` to return 0, triggering the misleading log "average chapter duration 0 was lower than the minimum threshold" and skipping chapter image extraction entirely. By returning an empty array when `chapterCount <= 1`, we avoid generating a useless single-chapter result that breaks downstream processing.

**Issues**
Fixes #14478
