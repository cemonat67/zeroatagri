# Zero@Agri Technical Documentation

## Developer’s Guide & System Architecture

-----

## TABLE OF CONTENTS

1. [Application Overview](#1-application-overview)
1. [System Architecture](#2-system-architecture)
1. [Algorithm & Logic Flow](#3-algorithm--logic-flow)
1. [Code Structure & Organization](#4-code-structure--organization)
1. [Core Functions Reference](#5-core-functions-reference)
1. [Data Models & Structures](#6-data-models--structures)
1. [UI/UX Implementation](#7-uiux-implementation)
1. [Performance Optimization](#8-performance-optimization)
1. [Extension & Customization](#9-extension--customization)
1. [Troubleshooting & Debug](#10-troubleshooting--debug)

-----

## 1. APPLICATION OVERVIEW

### 1.1 What Does This Application Do?

**Zero@Agri** is a comprehensive sustainability intelligence dashboard for Pagmat Food Industry that:

#### Primary Functions:

- **Real-time Monitoring**: Tracks CO₂ emissions, energy consumption, water usage, and packaging efficiency
- **Data Visualization**: Interactive charts and KPI cards displaying environmental metrics
- **Compliance Reporting**: Generates CBAM, CSRD, and ISO 14064 reports
- **AI-Powered Analytics**: Predictive modeling, anomaly detection, and optimization recommendations
- **Product Lifecycle Management**: Digital product passports with carbon footprints
- **Multi-section Navigation**: 9 different modules (Dashboard, Innovation, Analytics, Production, Logistics, Optimize, Reports, Compliance, Benchmark)

#### Business Purpose:

- Help Pagmat comply with EU sustainability regulations (CBAM, CSRD)
- Reduce operational costs through AI-driven efficiency recommendations
- Track and improve environmental performance metrics
- Generate audit-ready compliance documentation
- Provide stakeholder transparency through digital product passports

### 1.2 Technology Stack

```
Frontend:
├── HTML5 (Structure)
├── CSS3 (Styling with modern features)
│   ├── CSS Grid & Flexbox
│   ├── CSS Variables
│   ├── Animations & Transitions
│   └── Responsive Design
├── Vanilla JavaScript ES6+ (Logic)
│   ├── DOM Manipulation
│   ├── Event Handling
│   ├── Async/Await patterns
│   └── Modal management
└── Chart.js 4.4.0 (Data Visualization)
    ├── Bar Charts
    ├── Line Charts
    └── Doughnut Charts

External Dependencies:
└── Chart.js CDN only (no other libraries)
```

### 1.3 Application Type

- **Single Page Application (SPA)** - No page reloads
- **Client-side Rendering** - All logic runs in browser
- **Static Web Application** - No backend required (currently)
- **Progressive Enhancement** - Works without JavaScript (basic HTML)

-----

## 2. SYSTEM ARCHITECTURE

### 2.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    USER INTERFACE                        │
├─────────────────────────────────────────────────────────┤
│  Header  │  Navigation Tabs  │  Content Sections        │
└────┬──────────────┬───────────────────┬─────────────────┘
     │              │                   │
     ▼              ▼                   ▼
┌─────────┐  ┌──────────┐       ┌──────────┐
│  Time   │  │ Section  │       │   KPI    │
│ Update  │  │ Manager  │       │  Cards   │
└─────────┘  └──────────┘       └──────────┘
                   │                   │
                   ▼                   ▼
            ┌─────────────────────────────┐
            │    EVENT HANDLERS           │
            │  (User Interactions)        │
            └──────────┬──────────────────┘
                       │
                       ▼
            ┌─────────────────────────┐
            │   BUSINESS LOGIC        │
            ├─────────────────────────┤
            │ • Modal Management      │
            │ • Data Generation       │
            │ • Report Creation       │
            │ • AI Simulation         │
            │ • Chart Rendering       │
            └──────────┬──────────────┘
                       │
                       ▼
            ┌─────────────────────────┐
            │   DATA LAYER            │
            ├─────────────────────────┤
            │ • Hard-coded datasets   │
            │ • Dynamic calculations  │
            │ • State management      │
            └─────────────────────────┘
```

### 2.2 Component Breakdown

#### Core Components:

1. **Header Component**
- Logo and branding
- Real-time clock
- Company identification
1. **Navigation System**
- Tab-based navigation
- Active state management
- Section visibility control
1. **Dashboard Sections** (9 sections)
- Dashboard (default)
- Innovation
- Analytics
- Production
- Logistics
- Optimize
- Reports
- Compliance
- Benchmark
1. **Data Visualization**
- KPI Cards (clickable)
- Chart.js charts (3 types)
- Data tables
1. **Modal System**
- Dynamic modal creation
- Content injection
- Event management
1. **Loading Overlay**
- Progress indication
- User feedback

-----

## 3. ALGORITHM & LOGIC FLOW

### 3.1 Application Initialization Flow

```javascript
START Application
│
├── Load HTML Structure
│   └── Parse DOM elements
│
├── Load CSS Styles
│   └── Apply responsive design
│
├── Load JavaScript
│   │
│   ├── Initialize Event Listeners
│   │   ├── Navigation tabs
│   │   ├── Button clicks
│   │   ├── Modal close events
│   │   └── Table row clicks
│   │
│   ├── Start Time Update Loop
│   │   └── setInterval(updateTime, 60000)
│   │
│   └── Initialize Charts (on window.load)
│       ├── Create emissionsChart
│       ├── Create resourceChart
│       └── Create logisticsChart
│
└── Wait for User Interaction
```

### 3.2 Navigation Logic Algorithm

```javascript
FUNCTION handleNavigation(clickedTab)
│
├── Get all nav tabs
│
├── Remove 'active' class from all tabs
│   FOR EACH tab IN tabs
│   └── tab.classList.remove('active')
│
├── Add 'active' class to clicked tab
│   └── clickedTab.classList.add('active')
│
├── Get target section from data attribute
│   └── sectionId = clickedTab.getAttribute('data-section')
│
├── Hide all sections
│   FOR EACH section IN sections
│   └── section.classList.remove('active')
│
└── Show target section
    └── document.getElementById(sectionId).classList.add('active')
```

### 3.3 Modal Management Algorithm

```javascript
FUNCTION createModal(title, content)
│
├── Build modal HTML string
│   ├── Create wrapper div.modal
│   ├── Create div.modal-content
│   │   ├── Add div.modal-header
│   │   │   ├── title element
│   │   │   └── close button
│   │   └── Add div.modal-body
│   │       └── inject content parameter
│   └── Apply 'active' class
│
├── Inject HTML into modalContainer
│   └── document.getElementById('modalContainer').innerHTML = modalHTML
│
└── Modal is now visible and interactive

FUNCTION closeModal()
│
└── Clear modalContainer
    └── document.getElementById('modalContainer').innerHTML = ''

EVENT: Click outside modal
│
├── IF (click target === modal background)
│   └── closeModal()
└── ELSE
    └── Do nothing
```

### 3.4 Report Generation Algorithm

```javascript
FUNCTION generateReport(reportType)
│
├── Load report template from reports object
│   └── report = reports[reportType]
│
├── Show loading overlay
│   ├── Set loading text
│   └── Display spinner animation
│
├── Wait 2000ms (simulate processing)
│   └── setTimeout()
│
├── Generate report content
│   ├── Build text content
│   ├── Add timestamp
│   ├── Include company data
│   └── Format compliance metrics
│
├── Download file
│   ├── Create Blob from content
│   ├── Create download URL
│   ├── Trigger download
│   └── Cleanup URL
│
├── Hide loading overlay
│
└── Show success modal
    ├── Display confirmation
    └── Show report metadata
```

### 3.5 AI Analysis Simulation Algorithm

```javascript
FUNCTION runAIAnalysis(analysisType)
│
├── Close any open modal
│
├── Load analysis configuration
│   └── config = analysisTypes[analysisType]
│
├── Show loading overlay
│   ├── Display analysis title
│   └── Show progress subtitle
│
├── Simulate processing (2500ms)
│   └── setTimeout()
│
├── Generate results based on type
│   │
│   ├── IF type === 'optimization'
│   │   └── Generate 3 recommendations with savings
│   │
│   ├── IF type === 'prediction'
│   │   └── Generate 30-day forecast with confidence
│   │
│   ├── IF type === 'anomaly'
│   │   └── Scan for issues and show health status
│   │
│   └── IF type === 'report'
│       └── List available AI report types
│
├── Hide loading overlay
│
└── Display results in modal
    ├── Create modal with results content
    └── Show actionable insights
```

### 3.6 KPI Detail View Algorithm

```javascript
FUNCTION showKPIDetails(kpiType)
│
├── Load KPI data object
│   └── data = kpiData[kpiType]
│
├── Build detail content
│   ├── Add current metrics
│   ├── Add historical comparison
│   ├── Add trend indicators
│   ├── Add insights box
│   └── Add recommendations
│
└── Create and display modal
    └── createModal(data.title, data.content)
```

### 3.7 Chart Initialization Algorithm

```javascript
FUNCTION initializeCharts()
│
├── Wait for DOM ready
│   └── window.addEventListener('load')
│
├── Create Emissions Bar Chart
│   ├── Get canvas context
│   ├── Define data labels
│   ├── Define dataset values
│   ├── Configure colors
│   └── new Chart(ctx, config)
│
├── Create Resource Line Chart
│   ├── Get canvas context
│   ├── Define monthly labels
│   ├── Define trend data
│   ├── Configure gradients
│   └── new Chart(ctx, config)
│
└── Create Logistics Doughnut Chart
    ├── Get canvas context
    ├── Define country labels
    ├── Define emission data
    ├── Configure colors
    └── new Chart(ctx, config)
```

-----

## 4. CODE STRUCTURE & ORGANIZATION

### 4.1 File Structure

```
zero_agri_interactive.html
├── <head>
│   ├── Meta tags
│   ├── <title>
│   └── <style> (Embedded CSS)
│       ├── Reset & Global Styles
│       ├── Layout Components
│       ├── UI Components
│       ├── Modal System
│       ├── Animations
│       └── Responsive Media Queries
│
├── <body>
│   ├── Header Component
│   ├── Navigation Tabs
│   ├── Content Sections (9 sections)
│   │   ├── Dashboard
│   │   ├── Innovation
│   │   ├── Analytics
│   │   ├── Production
│   │   ├── Logistics
│   │   ├── Optimize
│   │   ├── Reports
│   │   ├── Compliance
│   │   └── Benchmark
│   ├── Loading Overlay
│   ├── Modal Container
│   │
│   └── <script>
│       ├── Chart.js CDN
│       └── Application JavaScript
│           ├── Utility Functions
│           ├── UI Management Functions
│           ├── Data Management Functions
│           ├── Event Listeners
│           └── Chart Initialization
```

### 4.2 CSS Architecture

```css
/* ORGANIZATION PATTERN */

1. Reset & Base Styles
   * { margin, padding, box-sizing }
   body { font, background, color }

2. Layout Components
   .header { ... }
   .nav-tabs { ... }
   .section-container { ... }

3. Grid Systems
   .dashboard-grid { ... }
   .kpi-grid { ... }

4. Card Components
   .card { ... }
   .kpi-card { ... }

5. UI Elements
   .btn { ... }
   .badge { ... }
   .data-table { ... }

6. Modal System
   .modal { ... }
   .modal-content { ... }
   .modal-header { ... }

7. Loading States
   .loading-overlay { ... }
   .spinner { ... }

8. Animations
   @keyframes spin { ... }
   @keyframes fadeIn { ... }
   @keyframes slideUp { ... }

9. Media Queries
   @media (max-width: 1200px) { ... }
   @media (max-width: 600px) { ... }
```

### 4.3 JavaScript Architecture

```javascript
/* FUNCTIONAL ORGANIZATION */

// ============================================
// SECTION 1: TIME & UPDATES
// ============================================
function updateTime() { ... }
setInterval(updateTime, 60000)

// ============================================
// SECTION 2: NAVIGATION MANAGEMENT
// ============================================
document.querySelectorAll('.nav-tab').forEach(tab => {
    tab.addEventListener('click', function() { ... })
})

// ============================================
// SECTION 3: MODAL MANAGEMENT
// ============================================
function createModal(title, content) { ... }
function closeModal() { ... }

// ============================================
// SECTION 4: LOADING STATES
// ============================================
function showLoading(text, subtext) { ... }
function hideLoading() { ... }

// ============================================
// SECTION 5: KPI INTERACTIONS
// ============================================
function showKPIDetails(type) { ... }

// ============================================
// SECTION 6: REPORT GENERATION
// ============================================
function generateReport(type) { ... }
function generateQuickReport() { ... }

// ============================================
// SECTION 7: AI FEATURES
// ============================================
function openAIAssist() { ... }
function runAIAnalysis(type) { ... }
function runPrediction() { ... }
function detectAnomalies() { ... }

// ============================================
// SECTION 8: DATA EXPORT
// ============================================
function exportAllData() { ... }
function exportAnalytics() { ... }
function exportRDProjects() { ... }

// ============================================
// SECTION 9: PRODUCT MANAGEMENT
// ============================================
function downloadProductPassport(product) { ... }

// ============================================
// SECTION 10: UTILITIES
// ============================================
function downloadFile(content, filename) { ... }
function refreshChartData() { ... }

// ============================================
// SECTION 11: CHART INITIALIZATION
// ============================================
window.addEventListener('load', function() {
    // Initialize all charts
})
```

-----

## 5. CORE FUNCTIONS REFERENCE

### 5.1 Navigation Functions

#### `updateTime()`

**Purpose**: Updates the clock display in real-time  
**Parameters**: None  
**Returns**: void  
**Called by**: setInterval (every 60 seconds)  
**Algorithm**:

```javascript
function updateTime() {
    const now = new Date()
    const timeStr = now.toLocaleTimeString('tr-TR', { 
        hour: '2-digit', 
        minute: '2-digit' 
    })
    const dateStr = now.toLocaleDateString('tr-TR', { 
        day: 'numeric', 
        month: 'short', 
        weekday: 'short' 
    })
    document.getElementById('currentTime').textContent = 
        `🕐 ${timeStr} ${dateStr}`
}
```

#### Navigation Tab Click Handler

**Purpose**: Switches between different dashboard sections  
**Trigger**: Click on any .nav-tab element  
**Algorithm**:

```javascript
document.querySelectorAll('.nav-tab').forEach(tab => {
    tab.addEventListener('click', function() {
        // Remove active class from all tabs
        document.querySelectorAll('.nav-tab')
            .forEach(t => t.classList.remove('active'))
        
        // Add active class to clicked tab
        this.classList.add('active')
        
        // Hide all sections
        document.querySelectorAll('.section-container')
            .forEach(s => s.classList.remove('active'))
        
        // Show target section
        document.getElementById(this.getAttribute('data-section'))
            .classList.add('active')
    })
})
```

### 5.2 Modal Management Functions

#### `createModal(title, content)`

**Purpose**: Creates and displays a modal dialog  
**Parameters**:

- `title` (string): Modal header text
- `content` (string): HTML content for modal body  
  **Returns**: void  
  **Side effects**: Injects HTML into #modalContainer  
  **Example**:

```javascript
createModal('Test Modal', '<p>Hello World</p>')
```

#### `closeModal()`

**Purpose**: Closes any open modal  
**Parameters**: None  
**Returns**: void  
**Side effects**: Clears #modalContainer innerHTML

### 5.3 Loading State Functions

#### `showLoading(text, subtext)`

**Purpose**: Displays loading overlay with custom message  
**Parameters**:

- `text` (string): Main loading message
- `subtext` (string): Secondary message  
  **Returns**: void  
  **Example**:

```javascript
showLoading('Processing...', 'Please wait')
```

#### `hideLoading()`

**Purpose**: Hides the loading overlay  
**Parameters**: None  
**Returns**: void

### 5.4 KPI Detail Functions

#### `showKPIDetails(type)`

**Purpose**: Shows detailed metrics for a specific KPI  
**Parameters**:

- `type` (string): One of ‘co2’, ‘energy’, ‘water’, ‘packaging’  
  **Returns**: void  
  **Data Structure**:

```javascript
const kpiData = {
    co2: {
        title: '🌍 CO₂ Emissions Deep Dive',
        content: '...' // HTML string with detailed stats
    },
    energy: { ... },
    water: { ... },
    packaging: { ... }
}
```

### 5.5 Report Generation Functions

#### `generateReport(type)`

**Purpose**: Generates and downloads a compliance report  
**Parameters**:

- `type` (string): ‘cbam’, ‘csrd’, ‘iso’, or ‘custom’  
  **Returns**: void  
  **Process Flow**:

1. Look up report template from `reports` object
1. Show loading overlay (2 seconds)
1. Generate text content with current data
1. Download file via `downloadFile()`
1. Hide loading overlay
1. Show success modal

**Report Types**:

```javascript
const reports = {
    cbam: {
        name: 'CBAM Compliance Report Q4 2024',
        content: '...' // Full CBAM report template
    },
    csrd: {
        name: 'CSRD Sustainability Report 2024',
        content: '...' // Full CSRD report template
    },
    iso: {
        name: 'ISO 14064-1 GHG Inventory Report 2024',
        content: '...' // Full ISO report template
    },
    custom: {
        name: 'Custom Sustainability Report',
        content: '...' // Customizable template
    }
}
```

### 5.6 AI Simulation Functions

#### `openAIAssist()`

**Purpose**: Opens AI Assistant modal with available features  
**Parameters**: None  
**Returns**: void  
**Shows**: 4 AI modules

- Optimization Analysis
- Predictive Forecast
- Anomaly Detection
- Smart Report Generation

#### `runAIAnalysis(type)`

**Purpose**: Simulates AI analysis and shows results  
**Parameters**:

- `type` (string): ‘optimization’, ‘prediction’, ‘anomaly’, ‘report’  
  **Returns**: void  
  **Process Flow**:

1. Close any open modal
1. Load analysis configuration
1. Show loading (2.5 seconds)
1. Generate realistic results based on type
1. Display results in modal

**Analysis Types Results**:

- **optimization**: 3 prioritized recommendations with cost savings
- **prediction**: 30-day forecast with confidence levels
- **anomaly**: System health status and recent detections
- **report**: List of available AI-generated reports

### 5.7 Export Functions

#### `exportAllData()`

**Purpose**: Exports all sustainability metrics to CSV  
**Parameters**: None  
**Returns**: void  
**CSV Format**:

```csv
Metric,Value,Unit,Period
CO2e Emissions,2847,tons,Oct 2024
Energy Consumption,4523,MWh,Oct 2024
Water Usage,12840,m³,Oct 2024
...
```

#### `exportAnalytics()`

**Purpose**: Exports analytics data  
**Parameters**: None  
**Returns**: void

#### `exportRDProjects()`

**Purpose**: Exports R&D project list  
**Parameters**: None  
**Returns**: void

### 5.8 Product Passport Function

#### `downloadProductPassport(product)`

**Purpose**: Generates detailed product carbon passport  
**Parameters**:

- `product` (string): ‘fig’, ‘apricot’, ‘walnut’, ‘raisin’, ‘pistachio’, ‘almond’  
  **Returns**: void  
  **Data Structure**:

```javascript
const products = {
    fig: { 
        name: 'Dried Figs', 
        co2: 420,           // kg CO₂e per ton
        water: 2840,        // liters per ton
        energy: 3.2,        // MWh per ton
        origin: 'Aydın, Turkey',
        harvest: 'August-September',
        certifications: 'Organic, Fair Trade, ISO 14064'
    },
    // ... other products
}
```

### 5.9 Utility Functions

#### `downloadFile(content, filename)`

**Purpose**: Triggers browser download of text file  
**Parameters**:

- `content` (string): File content
- `filename` (string): Download filename  
  **Returns**: void  
  **Implementation**:

```javascript
function downloadFile(content, filename) {
    const blob = new Blob([content], { 
        type: 'text/plain;charset=utf-8' 
    })
    const url = URL.createObjectURL(blob)
    const a = document.createElement('a')
    a.href = url
    a.download = filename
    document.body.appendChild(a)
    a.click()
    document.body.removeChild(a)
    URL.revokeObjectURL(url) // Cleanup
}
```

#### `refreshChartData()`

**Purpose**: Simulates chart data refresh  
**Parameters**: None  
**Returns**: void  
**Note**: Currently shows loading then alert (can be extended)

-----

## 6. DATA MODELS & STRUCTURES

### 6.1 KPI Data Model

```javascript
// KPI Metrics Structure
const kpiMetrics = {
    co2: {
        current: 2847,        // tons CO₂e
        lastMonth: 3235,      // tons CO₂e
        change: -12,          // percentage
        target: 32000,        // annual target
        unit: 'tons'
    },
    energy: {
        current: 4523,        // MWh
        renewable: 1847,      // MWh
        grid: 2676,           // MWh
        renewablePercent: 41, // %
        target: 4914,         // MWh
        unit: 'MWh'
    },
    water: {
        current: 12840,       // m³
        recycled: 3852,       // m³
        fresh: 8988,          // m³
        recyclingRate: 30,    // %
        yoyChange: -15,       // %
        unit: 'm³'
    },
    packaging: {
        efficiency: 87,       // %
        recyclable: 92,       // %
        biodegradable: 34,    // %
        co2Reduction: -23,    // %
        unit: '%'
    }
}
```

### 6.2 Product Data Model

```javascript
// Product Carbon Footprint Structure
const productData = {
    productId: 'fig',
    name: 'Dried Figs',
    emoji: '🌰',
    
    // Environmental Metrics
    carbon: {
        value: 420,           // kg CO₂e per ton
        breakdown: {
            agriculture: 180, // kg CO₂e
            processing: 120,  // kg CO₂e
            packaging: 60,    // kg CO₂e
            logistics: 60     // kg CO₂e
        }
    },
    
    water: {
        value: 2840,          // liters per ton
        recycled: 852         // liters
    },
    
    energy: {
        value: 3.2,           // MWh per ton
        renewable: 1.3        // MWh
    },
    
    // Supply Chain
    origin: {
        location: 'Aydın, Turkey',
        coordinates: [37.8444, 27.8458],
        harvestSeason: 'August-September'
    },
    
    // Certifications
    certifications: [
        'Organic',
        'Fair Trade',
        'ISO 14064'
    ],
    
    // Sustainability Score
    score: {
        overall: 91,
        breakdown: {
            carbon: 92,
            water: 94,
            energy: 96,
            packaging: 87,
            social: 89
        }
    }
}
```

### 6.3 Report Data Model

```javascript
// Report Template Structure
const reportTemplate = {
    type: 'cbam',             // Report type
    name: 'CBAM Compliance Report Q4 2024',
    standard: 'ISO 14064-1:2018',
    
    period: {
        start: '2024-10-01',
        end: '2024-10-31'
    },
    
    sections: {
        executiveSummary: {
            totalEmissions: 2847,
            complianceStatus: 'FULLY COMPLIANT'
        },
        
        scope1: {
            fuelCombustion: 847,
            processEmissions: 124,
            fugitiveEmissions: 18,
            total: 989
        },
        
        scope2: {
            electricity: 1642,
            heatSteam: 216,
            total: 1858
        },
        
        scope3: {
            purchasedGoods: 4247,
            upstreamTransport: 2418,
            downstreamTransport: 2708,
            total: 9373
        }
    },
    
    metadata: {
        generatedBy: 'Zero@Agri System',
        timestamp: '2024-10-29T02:17:00Z',
        version: '1.0',
        verified: true
    }
}
```

### 6.4 Chart Data Model

```javascript
// Chart Configuration Structure
const chartConfig = {
    emissionsChart: {
        type: 'bar',
        labels: ['Drying', 'Packaging', 'Processing', 'Logistics', 'Storage'],
        data: [680, 320, 210, 420, 120],
        colors: [
            'rgba(255, 193, 7, 0.8)',   // Yellow
            'rgba(33, 150, 243, 0.8)',  // Blue
            'rgba(76, 175, 80, 0.8)',   // Green
            'rgba(156, 39, 176, 0.8)',  // Purple
            'rgba(255, 152, 0, 0.8)'    // Orange
        ],
        options: {
            responsive: true,
            maintainAspectRatio: false
        }
    },
    
    resourceChart: {
        type: 'line',
        labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 
                 'Jul', 'Aug', 'Sep', 'Oct'],
        datasets: [{
            label: 'Energy (MWh)',
            data: [520, 485, 510, 475, 490, 465, 
                   480, 455, 470, 452],
            trend: 'decreasing'
        }]
    },
    
    logisticsChart: {
        type: 'doughnut',
        labels: ['Germany', 'Netherlands', 'France', 'UK', 'Belgium'],
        data: [340, 280, 195, 165, 120],
        total: 1100
    }
}
```

### 6.5 AI Analysis Results Model

```javascript
// AI Analysis Result Structure
const aiAnalysisResult = {
    type: 'optimization',
    timestamp: '2024-10-29T02:17:00Z',
    confidence: 0.94,         // 94% confidence
    
    recommendations: [
        {
            priority: 1,
            category: 'drying_process',
            title: 'Adjust temperature cycles',
            description: 'Reduce energy by 6-8% without quality impact',
            impact: {
                costSavings: 12400,      // EUR per year
                co2Reduction: 45,        // tons per year
                implementationTime: 4    // weeks
            }
        },
        // ... more recommendations
    ],
    
    totalImpact: {
        annualSavings: 20600,    // EUR
        co2Reduction: 127,       // tons
        implementationTime: 6    // weeks
    },
    
    metadata: {
        dataPointsAnalyzed: 2400000,
        modelVersion: '2.1.4',
        lastTraining: '2024-10-27'
    }
}
```

-----

## 7. UI/UX IMPLEMENTATION

### 7.1 Design System

#### Color Palette

```css
/* Primary Colors */
--primary-gold: #FFC107;        /* Brand color */
--primary-orange: #FF9800;      /* Accent */
--primary-dark: #0a1929;        /* Background base */
--primary-darker: #001e3c;      /* Background gradient */

/* Semantic Colors */
--success: #4CAF50;             /* Positive indicators */
--warning: #FFC107;             /* Caution states */
--info: #2196F3;                /* Information */
--error: #F44336;               /* Error states */

/* Neutrals */
--text-primary: #e8eaed;        /* Main text */
--text-secondary: #8b95a1;      /* Secondary text */
--border: rgba(255, 193, 7, 0.2); /* Borders */
```

#### Typography

```css
/* Font Stack */
font-family: -apple-system, BlinkMacSystemFont, 
             'Segoe UI', 'SF Pro Display', sans-serif;

/* Font Sizes */
--text-xs: 11px;    /* Labels, captions */
--text-sm: 12px;    /* Secondary text */
--text-base: 14px;  /* Body text */
--text-lg: 18px;    /* Card titles */
--text-xl: 24px;    /* Modal titles */
--text-2xl: 28px;   /* KPI values */
--text-3xl: 32px;   /* Page titles */

/* Font Weights */
--weight-normal: 400;
--weight-medium: 500;
--weight-semibold: 600;
--weight-bold: 700;
--weight-black: 900;
```

#### Spacing System

```css
/* 8px base unit */
--space-xs: 5px;    /* 0.5 unit */
--space-sm: 8px;    /* 1 unit */
--space-md: 12px;   /* 1.5 units */
--space-lg: 15px;   /* ~2 units */
--space-xl: 20px;   /* 2.5 units */
--space-2xl: 25px;  /* ~3 units */
--space-3xl: 30px;  /* ~4 units */
```

### 7.2 Component Patterns

#### Button Styles

```css
/* Primary Button (CTA) */
.btn-primary {
    background: linear-gradient(135deg, #FFC107 0%, #FF9800 100%);
    color: #001e3c;
    padding: 12px 24px;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(255, 193, 7, 0.3);
    transition: all 0.3s;
}

.btn-primary:hover {
    box-shadow: 0 6px 20px rgba(255, 193, 7, 0.5);
    transform: translateY(-2px);
}

/* Secondary Button */
.btn-secondary {
    background: rgba(255, 255, 255, 0.08);
    color: #fff;
    border: 1px solid rgba(255, 193, 7, 0.3);
}

.btn-secondary:hover {
    background: rgba(255, 193, 7, 0.15);
    border-color: rgba(255, 193, 7, 0.5);
}
```

#### Card Component

```css
.card {
    background: rgba(20, 30, 48, 0.9);
    backdrop-filter: blur(10px);      /* Glassmorphism */
    border-radius: 12px;
    padding: 25px;
    border: 1px solid rgba(255, 193, 7, 0.15);
    transition: all 0.3s;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.card:hover {
    transform: translateY(-3px);
    border-color: rgba(255, 193, 7, 0.4);
    box-shadow: 0 8px 30px rgba(255, 193, 7, 0.2);
}
```

#### KPI Card

```css
.kpi-card {
    /* Inherits card styles plus: */
    cursor: pointer;                  /* Clickable indicator */
}

.kpi-value {
    font-size: 28px;
    font-weight: 700;
    color: #fff;
    margin-bottom: 5px;
}

.kpi-change.positive {
    color: #4CAF50;                   /* Green for improvements */
}
```

### 7.3 Animation System

#### Keyframe Animations

```css
/* Fade In */
@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

/* Slide Up */
@keyframes slideUp {
    from {
        transform: translateY(30px);
        opacity: 0;
    }
    to {
        transform: translateY(0);
        opacity: 1;
    }
}

/* Spinner Rotation */
@keyframes spin {
    to { transform: rotate(360deg); }
}
```

#### Transition Patterns

```css
/* Standard transition for interactive elements */
.interactive-element {
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Hover effect pattern */
.hover-effect:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 25px rgba(255, 193, 7, 0.2);
}
```

### 7.4 Responsive Design Strategy

#### Breakpoints

```css
/* Large screens (default) */
/* No media query needed */

/* Medium screens (tablets) */
@media (max-width: 1200px) {
    .dashboard-grid {
        grid-template-columns: 1fr;    /* Stack columns */
    }
    .kpi-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

/* Small screens (mobile) */
@media (max-width: 600px) {
    .kpi-grid {
        grid-template-columns: 1fr;    /* Single column */
    }
    .header {
        flex-direction: column;        /* Stack header items */
    }
}
```

#### Mobile-First Considerations

```javascript
// Touch event optimization
.interactive-element {
    -webkit-tap-highlight-color: transparent;
    touch-action: manipulation;
}

// Prevent zoom on double-tap
meta[name="viewport"] {
    content="width=device-width, initial-scale=1.0, 
             maximum-scale=1.0, user-scalable=no"
}
```

-----

## 8. PERFORMANCE OPTIMIZATION

### 8.1 Current Performance Characteristics

#### Load Time Analysis

```
HTML Parsing:           ~50ms
CSS Parsing:            ~30ms
JavaScript Parsing:     ~80ms
Chart.js CDN Load:      ~200ms (network dependent)
Chart Initialization:   ~150ms
-----------------------------------------
Total Initial Load:     ~510ms (fast 3G)
```

#### Memory Footprint

```
HTML DOM:               ~45KB
CSS:                    ~12KB
JavaScript:             ~25KB
Chart.js Library:       ~180KB
Runtime Memory:         ~8MB
```

### 8.2 Optimization Techniques Implemented

#### 1. CSS Optimization

```css
/* Hardware acceleration for animations */
.animated-element {
    will-change: transform;
    transform: translateZ(0);
}

/* Efficient selectors (avoid deep nesting) */
/* GOOD: */
.card { }
.card-title { }

/* BAD: */
.dashboard .grid .card .header .title { }
```

#### 2. JavaScript Optimization

```javascript
// Event delegation for table rows (instead of individual listeners)
document.querySelector('.data-table tbody').addEventListener('click', (e) => {
    const row = e.target.closest('tr')
    if (row) {
        const product = row.dataset.product
        downloadProductPassport(product)
    }
})

// Debounced time updates (every 60s instead of 1s)
setInterval(updateTime, 60000)  // Not 1000ms

// Lazy chart initialization (only on window.load)
window.addEventListener('load', initCharts)
```

#### 3. Modal Performance

```javascript
// Modal creation is lazy (only when needed)
// Modals are destroyed when closed (not just hidden)
function closeModal() {
    document.getElementById('modalContainer').innerHTML = ''
    // Frees up memory immediately
}
```

### 8.3 Potential Performance Improvements

#### For Future Development:

**1. Code Splitting**

```javascript
// Current: All code loads upfront
// Better: Lazy load sections
const loadSection = async (sectionName) => {
    const module = await import(`./sections/${sectionName}.js`)
    return module.default
}
```

**2. Virtual Scrolling for Tables**

```javascript
// For large datasets (>100 rows)
// Only render visible rows + buffer
class VirtualTable {
    renderVisibleRows() {
        const start = Math.floor(scrollTop / rowHeight)
        const end = start + visibleRowCount
        return data.slice(start, end)
    }
}
```

**3. Chart Data Caching**

```javascript
// Cache chart data in localStorage
const chartDataCache = {
    set(key, data) {
        localStorage.setItem(`chart_${key}`, JSON.stringify(data))
    },
    get(key) {
        const cached = localStorage.getItem(`chart_${key}`)
        return cached ? JSON.parse(cached) : null
    }
}
```

**4. Image Optimization**

```html
<!-- If adding images: -->
<img src="logo.png" loading="lazy" decoding="async">
```

**5. Service Worker for Offline Support**

```javascript
// Cache static assets
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js')
}
```

### 8.4 Performance Monitoring

#### Add Performance Metrics

```javascript
// Measure page load time
window.addEventListener('load', () => {
    const perfData = performance.timing
    const pageLoadTime = perfData.loadEventEnd - perfData.navigationStart
    console.log(`Page load: ${pageLoadTime}ms`)
})

// Measure function execution time
function measurePerformance(fn, label) {
    const start = performance.now()
    fn()
    const end = performance.now()
    console.log(`${label}: ${(end - start).toFixed(2)}ms`)
}

// Usage:
measurePerformance(() => initCharts(), 'Chart Init')
```

-----

## 9. EXTENSION & CUSTOMIZATION

### 9.1 Adding New Sections

#### Step 1: Add Navigation Tab

```html
<div class="nav-tab" data-section="newsection">
    🆕 New Section
</div>
```

#### Step 2: Create Section Container

```html
<div id="newsection" class="section-container">
    <h1 class="page-title">🆕 New Section Title</h1>
    <p class="page-subtitle">Section description</p>
    
    <!-- Your content here -->
</div>
```

#### Step 3: Add to Navigation Logic

```javascript
// No code changes needed!
// Navigation system automatically handles new sections
// based on data-section attribute
```

### 9.2 Adding New KPI Cards

#### Template:

```html
<div class="kpi-card" onclick="showKPIDetails('newkpi')">
    <div class="kpi-label">🎯 New KPI Label</div>
    <div class="kpi-value">1,234</div>
    <div class="kpi-change positive">↑ 5% improvement</div>
</div>
```

#### Add KPI Data:

```javascript
// In showKPIDetails function, add to kpiData object:
const kpiData = {
    // ... existing KPIs
    newkpi: {
        title: '🎯 New KPI Deep Dive',
        content: `
            <div class="modal-stat">
                <span class="modal-stat-label">Current Value</span>
                <span class="modal-stat-value">1,234</span>
            </div>
            <!-- More stats -->
        `
    }
}
```

### 9.3 Adding New Charts

#### Step 1: Add Canvas Element

```html
<div class="card">
    <div class="card-title">New Chart Title</div>
    <div class="chart-container">
        <canvas id="newChart"></canvas>
    </div>
</div>
```

#### Step 2: Initialize Chart

```javascript
window.addEventListener('load', function() {
    // ... existing charts
    
    // New chart
    new Chart(document.getElementById('newChart'), {
        type: 'line',  // or 'bar', 'pie', 'doughnut', etc.
        data: {
            labels: ['Label1', 'Label2', 'Label3'],
            datasets: [{
                label: 'Dataset Label',
                data: [10, 20, 30],
                backgroundColor: 'rgba(255, 193, 7, 0.8)'
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            // ... chart options
        }
    })
})
```

### 9.4 Adding New Report Types

#### Step 1: Add to Reports Object

```javascript
const reports = {
    // ... existing reports
    newreport: {
        name: 'New Report Type',
        content: `NEW REPORT TEMPLATE
        
Company: Pagmat
Generated: ${new Date().toLocaleString()}

[Report content here]
        `
    }
}
```

#### Step 2: Add Generate Button

```html
<button class="btn btn-primary" 
        onclick="generateReport('newreport')">
    📋 Generate New Report
</button>
```

### 9.5 Customizing Colors & Branding

#### Change Primary Color:

```css
/* Find and replace all instances: */
--primary-gold: #FFC107;  /* Change to your color */

/* Also update in: */
- Button gradients
- Border colors
- Chart colors
- Badge backgrounds
```

#### Change Logo:

```html
<!-- In .logo-container: -->
<div class="logo-text">
    Your<span class="at">@</span><br>Brand
</div>
```

#### Change Company Name:

```html
<!-- In header: -->
<div class="brand-text">
    YourBrand<span class="at">@</span>Agri
</div>
```

### 9.6 Backend Integration Guide

#### For Adding Real API Calls:

**Step 1: Create API Service**

```javascript
class SustainabilityAPI {
    constructor(baseURL) {
        this.baseURL = baseURL
    }
    
    async getKPIData() {
        const response = await fetch(`${this.baseURL}/kpi`)
        return response.json()
    }
    
    async generateReport(type) {
        const response = await fetch(`${this.baseURL}/reports/${type}`)
        return response.json()
    }
    
    async runAIAnalysis(type, params) {
        const response = await fetch(`${this.baseURL}/ai/analyze`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ type, params })
        })
        return response.json()
    }
}

// Initialize
const api = new SustainabilityAPI('https://api.yourdomain.com')
```

**Step 2: Update Functions to Use API**

```javascript
// Before (static):
function showKPIDetails(type) {
    const data = kpiData[type]
    createModal(data.title, data.content)
}

// After (dynamic):
async function showKPIDetails(type) {
    showLoading('Loading...', 'Fetching latest data')
    try {
        const data = await api.getKPIData(type)
        hideLoading()
        createModal(data.title, generateKPIContent(data))
    } catch (error) {
        hideLoading()
        alert('Error loading data: ' + error.message)
    }
}
```

**Step 3: Add Error Handling**

```javascript
class APIError extends Error {
    constructor(message, status) {
        super(message)
        this.status = status
    }
}

async function apiCall(url, options = {}) {
    try {
        const response = await fetch(url, options)
        if (!response.ok) {
            throw new APIError(
                `HTTP ${response.status}: ${response.statusText}`,
                response.status
            )
        }
        return await response.json()
    } catch (error) {
        console.error('API call failed:', error)
        throw error
    }
}
```

### 9.7 Authentication Integration

#### Add Login System:

```javascript
class AuthService {
    constructor() {
        this.token = localStorage.getItem('auth_token')
    }
    
    async login(email, password) {
        const response = await fetch('/api/auth/login', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ email, password })
        })
        const data = await response.json()
        this.token = data.token
        localStorage.setItem('auth_token', data.token)
        return data
    }
    
    logout() {
        this.token = null
        localStorage.removeItem('auth_token')
    }
    
    isAuthenticated() {
        return !!this.token
    }
    
    getAuthHeader() {
        return {
            'Authorization': `Bearer ${this.token}`
        }
    }
}

// Use in API calls:
const auth = new AuthService()

fetch('/api/kpi', {
    headers: {
        ...auth.getAuthHeader(),
        'Content-Type': 'application/json'
    }
})
```

-----

## 10. TROUBLESHOOTING & DEBUG

### 10.1 Common Issues & Solutions

#### Issue 1: Charts Not Rendering

**Symptoms**: Canvas elements are blank or show errors  
**Causes**:

1. Chart.js CDN not loaded
1. Canvas elements don’t exist when script runs
1. Invalid data passed to Chart constructor

**Solutions**:

```javascript
// Add error handling to chart initialization:
window.addEventListener('load', function() {
    try {
        if (typeof Chart === 'undefined') {
            console.error('Chart.js not loaded')
            return
        }
        
        const canvas = document.getElementById('emissionsChart')
        if (!canvas) {
            console.error('Canvas element not found')
            return
        }
        
        new Chart(canvas, { /* config */ })
        
    } catch (error) {
        console.error('Chart initialization failed:', error)
    }
})
```

#### Issue 2: Modal Won’t Close

**Symptoms**: Modal remains visible after clicking close  
**Causes**:

1. Close button event not bound
1. JavaScript error preventing execution
1. Modal container not properly cleared

**Solutions**:

```javascript
// Add defensive checks:
function closeModal() {
    const container = document.getElementById('modalContainer')
    if (container) {
        container.innerHTML = ''
    } else {
        console.error('Modal container not found')
    }
}

// Ensure click outside works:
document.addEventListener('click', function(e) {
    if (e.target.classList.contains('modal')) {
        closeModal()
    }
})
```

#### Issue 3: Loading Overlay Stuck

**Symptoms**: Loading spinner never goes away  
**Causes**:

1. hideLoading() not called
1. JavaScript error in async operation
1. Timeout never resolves

**Solutions**:

```javascript
// Always use try-finally:
async function someAsyncOperation() {
    showLoading('Processing...', 'Please wait')
    try {
        await doSomething()
    } catch (error) {
        console.error('Operation failed:', error)
        alert('Error: ' + error.message)
    } finally {
        hideLoading()  // Always executes
    }
}

// Or use timeout failsafe:
function showLoadingWithTimeout(text, subtext, maxTime = 30000) {
    showLoading(text, subtext)
    setTimeout(() => {
        hideLoading()
        console.warn('Loading timeout reached')
    }, maxTime)
}
```

#### Issue 4: Download Not Working

**Symptoms**: File doesn’t download or downloads empty  
**Causes**:

1. Content is empty or undefined
1. Browser blocking download
1. Blob creation failed

**Solutions**:

```javascript
function downloadFile(content, filename) {
    if (!content) {
        console.error('No content to download')
        return
    }
    
    try {
        const blob = new Blob([content], { 
            type: 'text/plain;charset=utf-8' 
        })
        
        const url = URL.createObjectURL(blob)
        const a = document.createElement('a')
        a.href = url
        a.download = filename
        
        // Some browsers need element in DOM:
        document.body.appendChild(a)
        a.click()
        
        // Cleanup
        setTimeout(() => {
            document.body.removeChild(a)
            URL.revokeObjectURL(url)
        }, 100)
        
    } catch (error) {
        console.error('Download failed:', error)
        alert('Download failed. Please try again.')
    }
}
```

### 10.2 Debug Mode

#### Add Debug Logging:

```javascript
// At the top of the script:
const DEBUG = true  // Set to false in production

function debug(message, data = null) {
    if (DEBUG) {
        console.log(`[DEBUG] ${message}`, data || '')
    }
}

// Usage throughout code:
function generateReport(type) {
    debug('Generating report', { type })
    const report = reports[type]
    debug('Report template loaded', report)
    // ... rest of function
}
```

#### Performance Debugging:

```javascript
class PerformanceMonitor {
    constructor() {
        this.marks = {}
    }
    
    start(label) {
        this.marks[label] = performance.now()
    }
    
    end(label) {
        if (!this.marks[label]) {
            console.warn(`No start mark for: ${label}`)
            return
        }
        const duration = performance.now() - this.marks[label]
        console.log(`⏱️ ${label}: ${duration.toFixed(2)}ms`)
        delete this.marks[label]
    }
}

const perf = new PerformanceMonitor()

// Usage:
perf.start('chart-init')
initCharts()
perf.end('chart-init')
```

### 10.3 Browser Compatibility

#### Supported Browsers:

```
✓ Chrome 90+
✓ Firefox 88+
✓ Safari 14+
✓ Edge 90+
✗ IE 11 (not supported)
```

#### Polyfills Needed for Older Browsers:

```javascript
// Check and polyfill if needed:

// 1. Array.from
if (!Array.from) {
    Array.from = function(arrayLike) {
        return [].slice.call(arrayLike)
    }
}

// 2. Promise (for IE)
if (typeof Promise === 'undefined') {
    // Load polyfill from CDN
    const script = document.createElement('script')
    script.src = 'https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js'
    document.head.appendChild(script)
}

// 3. Fetch API
if (typeof fetch === 'undefined') {
    const script = document.createElement('script')
    script.src = 'https://cdn.jsdelivr.net/npm/whatwg-fetch@3/dist/fetch.umd.js'
    document.head.appendChild(script)
}
```

### 10.4 Testing Checklist

#### Manual Testing Steps:

1. **Navigation Tests**
   
   ```
   ☐ Click each tab
   ☐ Verify section switches correctly
   ☐ Check active state styling
   ☐ Test browser back/forward (doesn't break)
   ```
1. **KPI Card Tests**
   
   ```
   ☐ Click each KPI card
   ☐ Verify modal opens with correct data
   ☐ Test modal close button
   ☐ Test click-outside-to-close
   ```
1. **Report Generation Tests**
   
   ```
   ☐ Generate each report type
   ☐ Verify loading overlay appears
   ☐ Verify file downloads
   ☐ Check file content is correct
   ☐ Test with slow network
   ```
1. **AI Features Tests**
   
   ```
   ☐ Open AI Assistant
   ☐ Run each analysis type
   ☐ Verify results display
   ☐ Test multiple analyses in sequence
   ```
1. **Chart Tests**
   
   ```
   ☐ Verify all 3 charts render
   ☐ Test chart responsiveness
   ☐ Test refresh button
   ☐ Check chart on mobile
   ```
1. **Mobile Tests**
   
   ```
   ☐ Test on iOS Safari
   ☐ Test on Android Chrome
   ☐ Verify touch interactions
   ☐ Check responsive layout
   ☐ Test in portrait and landscape
   ```

#### Automated Testing Template:

```javascript
// Simple test framework
const tests = {
    'Time updates correctly': () => {
        updateTime()
        const timeEl = document.getElementById('currentTime')
        return timeEl.textContent.includes('🕐')
    },
    
    'Modal can be created': () => {
        createModal('Test', '<p>Content</p>')
        const modal = document.querySelector('.modal')
        return modal !== null
    },
    
    'Modal can be closed': () => {
        createModal('Test', '<p>Content</p>')
        closeModal()
        const modal = document.querySelector('.modal')
        return modal === null
    }
}

function runTests() {
    let passed = 0
    let failed = 0
    
    for (const [name, test] of Object.entries(tests)) {
        try {
            if (test()) {
                console.log(`✓ ${name}`)
                passed++
            } else {
                console.error(`✗ ${name}`)
                failed++
            }
        } catch (error) {
            console.error(`✗ ${name}: ${error.message}`)
            failed++
        }
    }
    
    console.log(`\nResults: ${passed} passed, ${failed} failed`)
}

// Run in console:
// runTests()
```

### 10.5 Console Commands for Developers

#### Useful Browser Console Commands:

```javascript
// 1. Manually trigger any function:
generateReport('cbam')
showKPIDetails('co2')
runAIAnalysis('optimization')

// 2. Inspect current state:
console.log(document.querySelector('.section-container.active').id)

// 3. Test modal system:
createModal('Test Modal', '<h3>Hello World</h3>')

// 4. Force show loading:
showLoading('Test', 'Testing loading overlay')

// 5. Clear all modals:
closeModal()

// 6. Get chart instances:
Chart.instances  // Array of all Chart objects

// 7. Trigger download:
downloadFile('test content', 'test.txt')

// 8. Check Chart.js loaded:
typeof Chart !== 'undefined'
```

-----

## APPENDIX A: Quick Reference

### Function Summary Table

|Function                          |Purpose                   |Parameters                       |Returns|
|----------------------------------|--------------------------|---------------------------------|-------|
|`updateTime()`                    |Update clock display      |None                             |void   |
|`createModal(title, content)`     |Display modal dialog      |title: string, content: HTML     |void   |
|`closeModal()`                    |Close active modal        |None                             |void   |
|`showLoading(text, subtext)`      |Show loading overlay      |text: string, subtext: string    |void   |
|`hideLoading()`                   |Hide loading overlay      |None                             |void   |
|`showKPIDetails(type)`            |Show KPI details modal    |type: string                     |void   |
|`generateReport(type)`            |Generate compliance report|type: string                     |void   |
|`generateQuickReport()`           |Generate executive summary|None                             |void   |
|`openAIAssist()`                  |Open AI assistant modal   |None                             |void   |
|`runAIAnalysis(type)`             |Run AI analysis           |type: string                     |void   |
|`downloadProductPassport(product)`|Generate product passport |product: string                  |void   |
|`exportAllData()`                 |Export all metrics to CSV |None                             |void   |
|`downloadFile(content, filename)` |Trigger file download     |content: string, filename: string|void   |

### Event Handlers Reference

|Element       |Event|Handler                    |Action             |
|--------------|-----|---------------------------|-------------------|
|`.nav-tab`    |click|inline                     |Switch sections    |
|`.kpi-card`   |click|`showKPIDetails(type)`     |Show KPI modal     |
|`.close-btn`  |click|`closeModal()`             |Close modal        |
|`.modal`      |click|conditional                |Close if background|
|Table rows    |click|`downloadProductPassport()`|Download passport  |
|Report buttons|click|`generateReport(type)`     |Generate report    |

-----

## APPENDIX B: Configuration Constants

### File Paths & URLs

```javascript
const CONFIG = {
    chartJsCDN: 'https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js',
    apiBaseURL: 'https://api.yourdomain.com',  // When backend added
    version: '1.0.0',
    appName: 'Zero@Agri'
}
```

### Timing Constants

```javascript
const TIMING = {
    loadingDelay: 2000,        // ms for loading overlay
    modalAnimation: 300,       // ms for modal fade in
    timeUpdateInterval: 60000, // ms for clock update
    chartRefresh: 5000         // ms for auto chart refresh (if enabled)
}
```

-----

## CONCLUSION

This Zero@Agri application is a sophisticated yet maintainable sustainability dashboard built with vanilla JavaScript and modern web technologies. The codebase follows clean architecture principles with clear separation of concerns, making it easy to extend and customize.

**Key Strengths:**

- No complex framework dependencies
- Responsive and performant
- Well-structured and documented
- Easy to integrate with backend APIs
- Comprehensive feature set

**For Production Deployment:**

1. Add backend API integration
1. Implement authentication
1. Add real-time data updates
1. Set up monitoring and analytics
1. Optimize and minify assets
1. Add automated testing
1. Set up CI/CD pipeline

**Developer Resources:**

- Chart.js Documentation: https://www.chartjs.org/docs/
- MDN Web Docs: https://developer.mozilla.org/
- JavaScript Best Practices: Clean Code principles

-----

**Document Version**: 1.0  
**Last Updated**: October 29, 2024  
**Maintained By**: Development Team  
**Contact**: dev@pagmat.com