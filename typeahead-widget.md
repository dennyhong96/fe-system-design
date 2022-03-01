### 1. General Requirements (Feature requirements)

- Widget provides search items results based on the given query user typed in
- The search results should be customizable (Render props, slots)
- Supports static data or asynchronous data

### 2. Functional Requirements (Platforms, etc.)

- Network efficiency
- Result caching
- Widget should be configurable
  - cacheSize
  - filterFunction
  - item representation(template)
  - minQuery
- Wdiget should work on a wide range of devices
- Widget should be accessible(keyboard) and friendly to people with disabilities
- Performance optimized

### 3. Component Architechture

```TSX
<Search>
  <SearchInput/>
    {suggestions.length > 0 && (
      <SuggestionsList>
        <Suggestion/> // Can accept templates (Render props pattern in react, slot template in Vue)
        <Suggestion/>
        <Suggestion/>
    </SuggestionsList>
    )}
</Search>
```

### 4. API Design

```typescript
// Generic because we want to work with any type of data
interface IProps<T> {
  getResults: (query: string) => Promise<T[]>;
  cacheSize?: number; // default = 5
  minQuery?: number; // default = 3, when user type in at lest 3 chars we fetch results
  maxResults?: number; // default = 10, show 10 results max
  renderItem: (item: T) => HTMLElement;
  className?: string;
}
```

### 6. Store Normalization (Local component state)

```typescript
interface IState {
  query: string;
  data: T[]; // results for current query
  cache: Map<string, T[]>; // a map of query to its results
  cacheSize: number;
  cacheTime?: number;
  minQuery: number;
  pageSize: number; // maxResults from prop
  pageNumber: number;
  onQuery: (query: string) => Promise<T[]>;
  template: (data: T) => HTMLElement;
}
```

```TSX
<Search>
  <SearchInput/> // -> query:string
    {suggestions.length > 0 && (
      <SuggestionsList> -> data: T[]
        <Suggestion/> -> renderItem(item:T)
        <Suggestion/>
        <Suggestion/>
    </SuggestionsList>
    )}
</Search>
```

### 7. Optimization

- Network performance
  - match results from server with what user has already typed
    - even with debounce a new http request could be sent before getting the response from previous one
      - use AbortController(with fetch) to cancel previous requests
      - use cancel tokens(with Axios) to cancel previous requests
      - include a query field in the server response so we can match it with current client query state
  - Dependent on the application side
  - Debounce the search calls
    - Reduce load on API
    - Reduce waste of data
  - Caching
    - Server cache
    - Browser cache (HTTP)
    - Widget cache (internal state)
- Rendering performance
  - DOM
    - List Virtualization for search results
      - Maintain constant number of nodes, only update data
      - Use Intersection observer
  - CSS
    - Use CSS animation over JS when possible
      - More optimized
      - Works on paint -> composition level
      - Does not trigger browser reflows(avoid changing width & height..)
  - Use Skeletons, Loaders, and placeholders when loading
    - Improve user's perceived performance
    - Clearly communicate to user that some operation is in progress
- JavaScript performance
  - prevent heavy operations from blocking UI interactions(single threaded)
    - Use promise/async to run operations that takes time
    - Cache results of heavy computation
    - Delegate heavy computation to Web Workers in background
    - Use high performence library written in WASM
  - Minimize the code
  - Use Web workers when filtering through large set of static data
  - Filter search result on the server side when possible

### 8. Accessbility

- Keyboard navigation
  - Tabable (Tab to move forwards, Shift + Tab to move backwards)
  - Search
  - Arrow keys to move between search suggestions
  - Close suggestions
  - Open suggestions
  - Next page of suggestions
  - previous page of suggestions
- Use rem units instead of px, adapts to browser zoom and custome font sizes
- Provide proper aria-roles and attributes for custom components
  - aria-haspopup to announce the element can load another layer
  - All inputs elements should have aria-live attribute. So assistive technology can inform user of content change

### 9. NPM Package Distribution

- Extract the widget as an NPM package
- Install and import to use in our app from a NPM registry
- Completely decoupled with the App, highly reusable
