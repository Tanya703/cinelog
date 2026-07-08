# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

I used Claude for Comment 2 to explain how deduplication works in add_to_collection() to apply the same logic to add_to_watchlist(). I also used Cluaude to write a curl command to verify if the add_to_watchlist will throw an error for duplicates.

## Comment 1 — Rename
**What I did:** Renamed save_to_watchlist() to add_to_watchlist() in services/watchlist_service.py. Updated all references to use the new function name, including the call in routes/watchlist/watchlist.py.
**How I verified:** Confirmed that a project-wide search returned no remaining occurrences of save_to_watchlist(). Ran pytest on tests/test_collection.py

## Comment 2 — Deduplication
**What I did:** added AlreadyInWatchlistError: If the film is already in the user's watchlist. Added a condition to check if the film is already in the watchlist.

**How I verified:** I used curl command

```powershell
# Get the ID of the first film
$FID = (Invoke-RestMethod "http://127.0.0.1:5001/films/")[0].id

# Use an existing user ID (replace with a valid UUID if required)
$U = "550e8400-e29b-41d4-a716-446655440000"

# Request body
$body = @{ film_id = $FID } | ConvertTo-Json

Write-Host "--- 1st add (expect 201) ---"
try {
    (Invoke-WebRequest "http://127.0.0.1:5001/collection/$U/add" `
        -Method POST `
        -ContentType "application/json" `
        -Body $body).StatusCode
}
catch {
    Write-Host ("HTTP " + $_.Exception.Response.StatusCode.value__)
}

Write-Host "--- 2nd add (expect 409 Conflict) ---"
try {
    (Invoke-WebRequest "http://127.0.0.1:5001/collection/$U/add" `
        -Method POST `
        -ContentType "application/json" `
        -Body $body).StatusCode
}
catch {
    Write-Host ("HTTP " + $_.Exception.Response.StatusCode.value__)
    $_.ErrorDetails.Message
}
```
For Comment 3 i used github copilot autocomplete to adjust test_collection.py file content to create similar tests for test_watchlist.py. I also used claude when i got an import error when running pytest to spot a missing import instance.

For Comment 4 i used prompt "What counterargument would a careful code reviewer raise against this position? What tradeoff am I not acknowledging?" to challenge my position

## Comment 3 — Missing test
**What I did:** Created a test_watchlist.py with two tests test_add_to_watchlist_duplicate_raises and test_add_to_watchlist_nonexistent_film_raises that test if the error is raised when added a duplicate to the watchlist and if adding nonexistent film raises an error respectively.
**How I verified:** I ran pytest for services/test_watchlist.py

## Comment 4 — Default visibility
**My position:** The watchlist should remain public by default
**Reasoning:**
A watchlist that's private-by-default in a social film-logging app means the network-effect feature (discovery, "what are my friends watching") starts empty. Public default is how you bootstrap the social graph and get engagement. Watchlist also doesn't contain any sensetive information. 
**Tradeoff acknowledged:** The decision is made for users and deprives users from the ability to control settings of the publicity of their watchlist. The feature that would alow users to toggle and change the access would address this concern.

## Comment 5 — Sort order
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->