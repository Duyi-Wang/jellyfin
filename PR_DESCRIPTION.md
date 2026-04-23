**Changes**
Fix dummy chapter generation for videos with a single chapter or short duration.

Two issues existed in `FFProbeVideoInfo.cs`:

1. Videos with only one chapter (spanning the entire duration) never triggered dummy chapter generation, because the condition checked `chapters.Length == 0`. A single meaningless chapter is effectively the same as no chapter at all, so this is changed to `chapters.Length <= 1`.

2. `CreateDummyChapters` returned an empty array when the video runtime was shorter than or equal to the configured `DummyChapterDuration`, meaning short videos could never get a dummy chapter. This is fixed by using `Math.Max(1, ...)` to ensure at least one chapter is always generated.

**Issues**
Fixes #14478
