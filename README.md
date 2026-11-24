# Fleet Tracking and Dispatch Alert Prototype

A browser demo of a live fleet map for small HazMat and tanker carriers. It shows approved routes, flags a truck that leaves its route, and drafts customer updates when a truck is delayed or off course. Every truck position and alert in it is simulated. It is a prototype, not a working product.

I dispatched 10 to 20 drivers at a hazmat carrier, and the part I wanted to fix was how much of a dispatcher's day goes to watching for problems instead of handling them. This demo is my answer to what a dispatcher's screen could look like if the system watched the fleet and only surfaced what needs attention.

## What it demonstrates

- A dark-theme map of eastern Ohio with three trucks moving along their routes
- Each truck's approved route as a dashed line and the path it actually drove as a trail behind it
- One truck that leaves its approved route partway through, which flips its status to "Off route"
- An "AI Situational Awareness" panel with a feed of alerts ranked low, medium and high
- Per-customer alert settings with two response modes:
  - **Approve first:** the system drafts the customer update and the dispatcher sends it with one click
  - **Auto-send:** pre-approved customers get the update the moment a threshold is crossed

## What is real and what is simulated

| Piece | Status |
| --- | --- |
| Map, truck cards, status colors, alert feed and approve button | Working in the demo |
| Truck positions and routes | Simulated |
| Staging and delay events | Scripted, timed to the animation |
| Customer message drafts | Written text, not generated live |
| Geotab or any live GPS data | Not connected |
| Backend, database, user accounts | Not built |

The alerts are driven by a short script, so this proves the screen design and the alert logic, not the detection itself.

## Tech stack

- HTML, CSS and vanilla JavaScript in a single file
- Leaflet 1.9.4 for the map
- CARTO dark map tiles and OpenStreetMap data

## How to run it

1. Download `index.html`.
2. Open it in a browser. It needs an internet connection to load Leaflet, the map tiles and the fonts.
3. Wait about 40 seconds. The alerts fire on a timer, in this order:
   1. a low severity note that T-108 is on schedule
   2. a medium alert as T-104 nears its customer's 20 minute staging limit
   3. a high alert with a drafted message the dispatcher can approve
   4. a high alert about 35 seconds in, when T-111 leaves its route, with a message that is sent automatically because that customer is set to auto-send

## Design decisions and what I learned

**Alerts should be tiered, not all-or-nothing.** I started with the question of whether customer alerts should send automatically or wait for approval. The answer was "it depends on the customer and the severity". Low severity stays on the dashboard only, medium goes to the dispatcher, and high drafts a customer message. Each customer gets two settings: a delay threshold and a notify mode.

**Most customers do not want every delay.** Some want to hear about a 15 minute wait and most want nothing, so the thresholds are set per account instead of fleet-wide.

**Live tracking needs a backend.** A tracking provider's login cannot live in a web page, because anyone could read it in the page source. A real version needs a server that holds the credentials, pulls the data and passes only the results to the dispatch screen. I did not build that part.

## Future improvements

- Connect a real GPS data source through a hosted backend
- Add a HazMat-aware routing source so approved routes respect road, bridge and tunnel restrictions
- Replace the scripted alerts with detection driven by live data
- Make the Edit button on drafted messages work
- Store jobs, drivers and customer alert settings in a database
- Add user roles so a dispatcher and a customer see different things

## Author

Francisco Juarez
[LinkedIn](https://www.linkedin.com/in/franciscojuarez02) | [GitHub](https://github.com/pacoj02)
