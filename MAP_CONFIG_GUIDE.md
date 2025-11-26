# Map Functionality Configuration Guide

## Problem
Map functionality is not working because required API keys and configuration are missing.

## Root Causes

### 1. **Frontend Map Not Rendering**
- Missing `VITE_MAPBOX_TOKEN` in `frontend/.env`
- `ShopMap.jsx` requires this token to initialize Mapbox GL
- Currently, maps gracefully hide when token is missing

### 2. **Geocoding/Location Services Disabled**
- Missing `MAPPLE_API_KEY` in `backend/.env`
- Missing `MAPPLE_GEOCODE_URL` in `backend/.env`
- Backend logs show: "Geocoding service disabled: No API key provided"

### 3. **API Key Priority**
The system uses this fallback strategy:
1. **Mappls (MapmyIndia)** - Primary (best for India)
2. **Mapbox** - Fallback if Mappls disabled
3. **Google Maps** - Fallback if neither configured
4. **Nominatim** - Free fallback (limited)

---

## Solution

### Step 1: Get Mapbox Token (for frontend maps)
1. Visit: https://mapbox.com/
2. Create account → Get free token
3. Add to `frontend/.env`:
```env
VITE_MAPBOX_TOKEN=pk.eyJ1IjoiYXlvdXJlbWFpbCIsImEiOiJjazEyMzQ1Njc4In0.xxx
```

### Step 2: Configure Mappls (Recommended for India)
1. Visit: https://www.mapmyindia.com/api/
2. Sign up for free account
3. Get API key
4. Add to `backend/.env`:
```env
MAPPLE_API_KEY=your_api_key_here
MAPPLE_GEOCODE_URL=https://atlas.mappls.com/api/places/geocode
```

### Step 3 (Alternative): Use Google Maps API
If you prefer Google Maps instead:
1. Get API key from: https://console.cloud.google.com/
2. Add to `backend/.env`:
```env
GOOGLE_MAPS_API_KEY=AIzaSyABC123DEF456GHI789JKL012MNO345PQR
GOOGLE_PLACES_API_KEY=AIzaSyABC123DEF456GHI789JKL012MNO345PQR
GEOCODER_PROVIDER=google
```

### Step 4: Restart Services
After updating `.env` files:
```bash
# Restart backend (if running in background terminal)
# Ctrl+C then: npm run dev

# Frontend will auto-reload with hot reload
```

---

## Testing Map Functionality
1. Open http://localhost:3000
2. Search for any shop → Should display on map
3. Check browser console for errors
4. Check backend logs for geocoding errors

## Files Affected
- `backend/.env` - Missing map API keys
- `frontend/.env` - Missing VITE_MAPBOX_TOKEN
- `backend/src/services/geocodingService.js` - Requires API keys to function
- `frontend/src/components/map/ShopMap.jsx` - Requires token to render
