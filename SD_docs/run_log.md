#
+ -> worked
- -> not worked
* -> changes that i did and it worked
#
+ node --version  => v24.20.0
- npm install
* npm.cmd install
+ npm.cmd run dev => Event Photo API listening on http://localhost:3000
- test the server => Cannot GET /
- curl http://localhost:3000/health => PowerShell interpreted curl as Invoke-WebRequest and displayed a security warning.
* curl.exe http://localhost:3000/health 
=> {"status":"ok"}
+ curl.exe -H "x-photographer-id: studio-amber" http://localhost:3000/api/events 
=> [{"id":"event-100","title":"Cohen Wedding","galleryPublished":true}]
- curl -H "x-photographer-id: studio-amber" http://localhost:3000/api/events/event-100/gallery 
=> PowerShell interpreted curl as Invoke-WebRequest and displayed a security warning.
* curl.exe -H "x-photographer-id: studio-amber" http://localhost:3000/api/events/event-100/gallery 
=> {"id":"event-100","title":"Cohen Wedding","photos":["photo-1","photo-2","photo-3"]}
- +  curl.exe -X POST http://localhost:3000/api/events/event-100/selections -H "content-type: application/json" -H "x-client-id: client-100" -d '{"photoIds":["photo-1"],"requestedServices":["retouching","print-10x15"]}'
=> the server received the POST request, but The request failed with a JSON parsing error
- curl.exe -X POST "http://localhost:3000/api/events/event-100/selections" -H "content-type: application/json" -H "x-client-id: client-100" --data-raw "{`"photoIds`":[`"photo-1`"],`"requestedServices`":[`"retouching`",`"print-10x15`"]}"
=> the same reason, i try to fix, but it is not worked
-  $body = '{"photoIds":["photo-1"],"requestedServices":["retouching","print-10x15"]}'
=> another try...
* Set-Content -Path request.json -Value '{"photoIds":["photo-1"],"requestedServices":["retouching","print-10x15"]}' -Encoding utf8
* * curl.exe -X POST "http://localhost:3000/api/events/event-100/selections" -H "content-type: application/json" -H "x-client-id: client-100" --data-binary "@request.json"
==>> {"id":"selection-1","eventId":"event-100","clientId":"client-100","photoIds":["photo-1"],"requestedServices":["retouching","print-10x15"],"status":"submitted","createdAt":"2026-10-21T18:39:02.562Z"}
+ npm.cmd test
=> Result: Passed. Test files: 1 passed. Tests: 5 passed, 0 failed.
Verified: photographer identity is required; events are filtered by photographer; clients can submit selections for their own gallery but cannot access another client's gallery.
+ npm.cmd run typecheck