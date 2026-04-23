**Changes**
Fix dummy chapter handling for videos with a single chapter or short duration.

1. Change the condition that triggers dummy chapter creation from `chapters.Length == 0` to `chapters.Length <= 1` in `FFProbeVideoInfo`. A video with only one chapter spanning its entire duration is effectively unchaptered and should trigger dummy chapter generation.

2. In `CreateDummyChapters`, use `Math.Max(1, ...)` to guarantee at least one chapter is always generated. Previously videos shorter than `DummyChapterDuration` got no dummy chapters at all.

3. In `ChapterManager.RefreshChapterImages`, only apply the average-duration threshold check when there are 2+ chapters. A single chapter has no "average duration between chapters", so the check was incorrectly returning 0 and disabling chapter image extraction with a misleading log message.

**Issues**
Fixes #14478
