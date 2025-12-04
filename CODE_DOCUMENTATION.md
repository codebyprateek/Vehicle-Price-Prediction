# Code Structure & Documentation

This document outlines the technical implementation of the Vehicle Price Prediction System.

## 1. Project Architecture
The project is built as a **Single Page Application (SPA)** using React and Vite. It follows a component-based architecture.

### Directory Map
- `client/src/pages/`: Contains the main views.
    - `Home.tsx`: Landing page with project overview.
    - `Dashboard.tsx`: Analytics dashboard using Recharts.
    - `Predictor.tsx`: The core prediction form and logic.
- `client/src/lib/`: Utility functions.
    - `mockData.ts`: Stores the simulated dataset and statistical constants.
- `client/src/components/`: Reusable UI elements (Buttons, Cards, Inputs).

## 2. Key Components

### Data Visualization (`Dashboard.tsx`)
We use `recharts` to render responsive charts.
```typescript
<BarChart data={priceDistributionData}>
  <XAxis dataKey="range" />
  <YAxis />
  <Bar dataKey="count" fill="hsl(var(--primary))" />
</BarChart>
```

### Prediction Logic (`Predictor.tsx`)
The prediction is handled client-side for the prototype. It calculates a price based on weighted factors.

**Algorithm Snippet:**
```typescript
// Base calculation
let basePrice = 30000;

// 1. Make Factor (Brand Value)
if (["GMC", "RAM", "Ford"].includes(make)) basePrice += 15000;
if (["BMW", "Mercedes"].includes(make)) basePrice += 25000;

// 2. Depreciation (Age)
const age = currentYear - vehicleYear;
basePrice -= (age * 2500); // $2.5k loss per year

// 3. Mileage Depreciation
basePrice -= (mileage * 0.15); // 15 cents per mile

// 4. Condition Adjustment
basePrice *= (condition / 10);
```

### Mock Data Structure (`mockData.ts`)
To simulate a real database without a backend, we export typed arrays.
```typescript
export const stats = {
  totalVehicles: 3379,
  avgPrice: 54200,
  brands: ["Jeep", "Ford", "GMC", "RAM", ...],
};
```

## 3. Styling System
The project uses **Tailwind CSS** with a custom theme defined in `index.css`. We use CSS variables for theming to support easy dark mode switching.

```css
@theme inline {
  --color-primary: hsl(210 100% 50%);
  --color-background: hsl(222 47% 11%);
  --font-display: 'Space Grotesk', sans-serif;
}
```

## 4. Future Work (Backend Integration)
To move this to production:
1.  Create a Python API (`app.py`) using FastAPI.
2.  Load the CSV dataset using Pandas.
3.  Train a model: `model = RandomForestRegressor().fit(X, y)`.
4.  Replace the frontend heuristic logic with an API call:
    ```typescript
    const response = await fetch('/api/predict', { method: 'POST', body: JSON.stringify(data) });
    ```
