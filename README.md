AviRadar Sky

A mobile-first aircraft radar interface for the M.A.R.V.I.N. Worker API.

The website visualizes nearby aircraft in a circular radar view and displays live aircraft details, detected Munich departures and system health information.

Live API

The website is connected to:


Used endpoints:

GET /aircraft
GET /identify
GET /departures?hours=3
GET /health

Features

Circular live radar view

Aircraft position and heading visualization

Radar ranges of 25, 50 and 100 km

Automatic aircraft refresh every 30 seconds

Browser geolocation support

AVILUS location as fallback center

Aircraft details including:

operator

callsign

registration

aircraft type

altitude

speed

heading

vertical rate

ICAO address

Aircraft identification through the M.A.R.V.I.N. /identify endpoint

Munich Airport departure overview

Worker health and connection status

Responsive mobile and desktop layout

AVILUS-inspired color palette

Project Structure

The project can consist of a single file:

/
└── index.html

No build process or external JavaScript framework is required.

Installation

Create a new GitHub repository or use an existing repository.

Upload the supplied file as:

index.html

Commit the file to the main branch.

Open the repository settings.

Go to:

Settings → Pages

Under Build and deployment, select:

Source: Deploy from a branch
Branch: main
Folder: /root

Save the settings.

GitHub Pages will publish the website after a short deployment period.

Recommended GitHub Pages URL

The current Worker configuration already allows:

https://avitracker.github.io

The radar can therefore be hosted below this origin, for example:

https://avitracker.github.io/sky/

CORS Configuration

The Worker currently allows these origins:

allowedOrigins: new Set([
  "https://avitracker.github.io",
  "http://localhost:8000",
  "http://127.0.0.1:8000",
])

When the website is hosted on another domain, add that exact origin to the Worker configuration.

Example:

allowedOrigins: new Set([
  "https://avitracker.github.io",
  "https://example.com",
  "http://localhost:8000",
  "http://127.0.0.1:8000",
])

After changing the Worker code, deploy the Worker again.

Local Testing

Do not open index.html only by double-clicking it. Some browser features and API requests may not work correctly with a file:// URL.

Start a local web server instead.

Using Python:

python -m http.server 8000

Then open:

http://localhost:8000

The Worker already allows this local origin.

Geolocation

The website asks the browser for the user's location.

When permission is granted, distances are calculated from the device location.

When permission is declined or unavailable, the website uses the configured fallback center:

const FALLBACK_CENTER = {
  lat: 48.226,
  lon: 11.675
};

Geolocation normally requires HTTPS. GitHub Pages provides HTTPS automatically.

Configuration

The main settings are located near the beginning of the JavaScript section inside index.html.

API URL

const API_BASE =
  "https://aviradar-api.joschko-hammermann.workers.dev";

Fallback Center

const FALLBACK_CENTER = {
  lat: 48.226,
  lon: 11.675
};

Refresh Interval

const REFRESH_MS = 30_000;

Maximum Aircraft in the Main List

const MAX_LIST_ITEMS = 12;

Navigation

The interface contains four sections:

Sky

Shows the circular radar and aircraft currently inside the selected range.

Nearby

Shows aircraft sorted by distance from the current radar center.

Departures

Shows departures detected by the Worker near Munich Airport during the previous three hours.

System

Shows the current health and configuration of the M.A.R.V.I.N. Worker.

Data Notes

Aircraft data is supplied by the Worker and may contain incomplete fields.

Depending on the available ADS-B data, some aircraft may not include:

operator

aircraft type

registration

callsign

altitude

vertical rate

The interface automatically uses the best available fallback label.

Aircraft positions should be treated as informational and may be delayed or inaccurate.

Troubleshooting

The page shows “Unavailable”

Check:

https://aviradar-api.joschko-hammermann.workers.dev/health

The response should include:

{
  "ok": true
}

Aircraft do not appear

Check:

https://aviradar-api.joschko-hammermann.workers.dev/aircraft

The response should contain an ac array.

Example:

{
  "ac": []
}

An empty array means that no aircraft data was returned for the configured area.

CORS error in the browser console

Confirm that the website's exact domain is included in the Worker's allowedOrigins list.

The origin must not include a path.

Correct:

https://example.com

Incorrect:

https://example.com/sky/

Location permission does not appear

Confirm that the page is opened through HTTPS or through:

http://localhost:8000

Departures remain empty

The departure list depends on the Worker's departure detection logic and D1 database ingestion. An empty list does not necessarily indicate a website error.

Browser Support

Recommended browsers:

Google Chrome

Microsoft Edge

Safari

Firefox

Recent browser versions are recommended.

Branding Colors

The interface uses the following AVILUS-inspired colors:

Midnight Ops: #183032
Deep Snow:    #f7f7f7
Radar Beam:   #ddff2c
Mission Green:#465046

Security

The website does not require API keys.

Do not place private credentials, Cloudflare tokens or database secrets inside index.html, because all browser-side code is publicly visible.

License

Internal AVILUS project. Add a separate license file before public reuse or external distribution.
