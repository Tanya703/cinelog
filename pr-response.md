# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** Renamed save_to_watchlist() to add_to_watchlist() in services/watchlist_service.py. Updated all references to use the new function name, including the call in routes/watchlist/watchlist.py.
**How I verified:** Confirmed that a project-wide search returned no remaining occurrences of save_to_watchlist(). Ran pytest on tests/test_collection.py

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


## Comment 3 — Missing test
**What I did:**
**How I verified:**

## Comment 4 — Default visibility
**My position:**
**Reasoning:**
**Tradeoff acknowledged:**

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