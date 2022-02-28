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
- Rendering performance
- JavaScript performance
- PWA (offline access)

### 8. Accessbility
