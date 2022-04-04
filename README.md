## Feature/App Design Template

### 1. General Requirements (Feature requirements)

### 2. Specific Requirements (Functional requirements, Platforms, Target Browsers, etc.)

### 3. Component Architechture

### 4. Data API and protocol

### 5. Data Entities and Store

### 6. Optimization

### 7. Accessbility

## Widget/Component Design Template

### 1. General Requirements (Feature requirements)

### 2. Functional Requirements (Platforms, etc.)

- debounce?
- cache?
- extensible/configurable
- works on wide range of devices
- accessibles

### 3. Component Architechture

- high-level mockup
- dependency graph

### 4. Props and API design

props - arguments passed to the widget constructor/init function
API - methods the widget expose

- init()
- destroy()
- update()

### 5. State Design

- local state
- how data flows between parts of the component

### 6. Optimization

- Network:
  - GZIP & Brotli & Minify CSS/JS Assets
  - HTTP2 Multiplexing
    - more granular/modular bundles
      - core bundle
      - utils bundle
      - theming bundles
    - ES6 Bundle
  - debounce
  - caching
    - Server cache
    - HTTP Cache
    - JS Map Extension
      - Cache size
      - Cache time
- Rendering:
  - DOM
    - ...React window
  - CSS
    - ...
  - Loading placeholders
- JavaScript:
  - delegate work to server
  - event delegation
  - ...

### 7. Extensibility

- custom render functions
- event callbacks
- css theming

### 8. Accessbility

### 9. NPM Package Distribution

- Abstract component into NPM package
- Integrate the component using a NPM registry
