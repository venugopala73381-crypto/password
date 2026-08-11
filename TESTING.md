# Testing Checklist

Run through this list after starting both the backend and frontend servers.

## Backend (http://localhost:5000)
- [ ] `npm run dev` inside `backend/` starts without errors
- [ ] Terminal shows "MongoDB connected: ..." and "Server running on port 5000"
- [ ] Visiting http://localhost:5000/ in a browser shows a JSON message
- [ ] POST http://localhost:5000/api/analysis with a valid body returns 201
- [ ] GET http://localhost:5000/api/analysis/recent returns an array

## Frontend (http://localhost:5173)
- [ ] `npm run dev` inside `frontend/` starts without errors
- [ ] Home page loads with hero section and 4 feature cards
- [ ] Analyzer page: typing a password shows live score, strength badge, reasons, and suggestions
- [ ] Analyzer page: clicking "Save Analysis (Anonymous)" shows a success message
- [ ] Analyzer page: "Recent Analyses" list updates after saving
- [ ] Generator page: slider changes length, checkboxes toggle character types
- [ ] Generator page: "Generate Password" produces a password matching the selected options
- [ ] Generator page: "Copy" button copies the password and shows "Copied!"
- [ ] Security Tips page shows 6 tip cards
- [ ] About page shows project information
- [ ] Navbar links work on both desktop and mobile widths (resize browser to test)

## Security checks
- [ ] Open browser DevTools → Network tab → confirm no request ever contains the raw password
- [ ] Open browser DevTools → Application → Local Storage → confirm it's empty (no password saved)
- [ ] Check backend terminal logs — confirm no password ever appears in console output
- [ ] Check MongoDB collection (`analyses`) — confirm documents only contain score/strength/problems/suggestions/createdAt

## Common errors and fixes

**"Failed to fetch" / Network error on Analyzer or Generator page**
→ Make sure the backend server is running on port 5000 and `frontend/.env` has `VITE_API_URL=http://localhost:5000/api`.

**"MongoDB connection error" in backend terminal**
→ Check `backend/.env` — make sure `MONGO_URI` is your real, correct MongoDB connection string.

**CORS error in browser console**
→ Make sure `backend/server.js` has `app.use(cors())` before the routes (it already does in this project).
