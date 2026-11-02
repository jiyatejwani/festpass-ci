# FestPass CI – Evidence (Jiya)

Job: `festpass-ci` · Pipeline script from SCM · `git://127.0.0.1:9418/festpass-java` · branch `*/main` · `Jenkinsfile` · agent `student-laptop`
Trigger: `triggers { pollSCM('* * * * *') }` committed in the Jenkinsfile (commit `8103b6b`).
Codes: class.code `001`, challenge.code `00013`.

## Builds

| | Red (automatic) | Green (automatic) |
|---|---|---|
| Build | #4 | #5 |
| Commit | `0881c6e` Personalize my help introduction | `8ce53b7` Fix cancel so it frees the booked seat |
| Cause | Started by an SCM change | Started by an SCM change |
| Tests | 28 tests, 3 failures | 28 tests, 0 failures |
| Build JAR / Smoke test JAR / Save artifact | Skipped (earlier failure) | Ran and passed |
| JAR published | No (`NOT_PUBLISHED`) | Yes – `festpass.jar`, SHA-256 `8afa9c00…32bd60` |
| Evidence CSV | `build-4/submission-evidence.csv` | `build-5/submission-evidence.csv` |

## Failing assertion (build #4)

- `cancellationsSurviveReload`: **"Cancellation was lost"**
- `cancellationFreesSeat`: booking after a cancellation threw **"SOLD OUT. Even the developer needs a ticket."**
- `cliWorkflowPersistsAcrossCommands`: "List does not reflect persisted cancellation"

## What I changed and why

Business rule: cancelling a booking must free that seat for someone else.
`BookingService.cancel()` checked that the student had a booking but never removed them, so the seat stayed occupied, the event stayed full, and the cancellation was not saved.
Fix (one line, tests unchanged): `event.students.remove(studentId);` at the end of `cancel()`.

## Final JAR output

```
> java -jar festpass.jar help
FestPass - campus event registration (personalized by Jiya)
Usage: java -jar build/festpass.jar [--data FILE] COMMAND
...
```
Smoke test: "PASS: seven real-JAR commands; persistence, sold-out rejection, cancellation, and seat reuse verified."
