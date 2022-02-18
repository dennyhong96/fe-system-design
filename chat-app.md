## 1. General Requirements (Feature requirements)

- User can send and receive messages
- User can attach medias to messages
  - video
  - audio
  - pictures
  - location
- User can see contact list
- User can share messages

## 2. Specific Requirements (Functional requirements, Platforms, etc.)

- Low latency for messages (real-time, 40-60ms)
- App should work in short network band (mobile devices)
- App should not waste resource (battery life)
- Focus on traffic and resources
- App should work on a wide range of devices

## 3. Component Architechture

```TSX
<ChatApp>
  <ContactList>
    <SearchBox/>
    <Contact/>
    <Contact/>
  </ContactList>
  <ChatView>
    <MessageList>
      <Message>
        <MessageActions /> // reply, copy, share, delete
      </Message>
      <Message>
        <AttachmentList>
          <Attachment/>
          <Attachment/>
        </AttachmentList>
        <MessageActions />
      </Message>
    </MessageList>
    <Controls>
      <Input/>
      <InputActions/> // media, emoji, quick photo, record audio
    </Controls>
  </ChatView>
</ChatApp>
```

## 4. Data API and protocol

- ### Protocol
  - Long polling
    - Pros.
      - HTTP benefits
      - Easy implementation
    - Cons.
      - When moving, network cell changes often, increases latency
      - Connections can timeout caused by certain proxies
      - Large traffic overhead, always send full request, incl. header, token, etc
  - WebSockets
    - Pros.
      - Duplex, bi-directional client-server communication
      - Super low latency
    - Cons.
      - Constant TCP connection is expensive from resource perspective
      - Not HTTP2 compatible
        - We have to manually do optimization that comes for free in HTTP2 protocol
          - Asset zipping
          - Multiplexing
      - Hard to load balance
        - Usually server has proxies and firefalls, they can sometimes filter TCP traffic, and re-connecting WebSockets is not a trivial to do in production.
        - Creating additional infrastructure that needs to be maintained
        - Costing additional resources
  - Server Sent Events
    - Pros.
      - HTTP2 compatible
        - More scalable
        - Asset zipping
        - Caching
        - Multiplexing
      - Don't transfer unneccessary data, no headers, tokens, cookies, etc.
      - SSE doesn't waste device resources, doesn't drain battery life
      - Easier to load balance and re-connect from infrastructure perspective (in-case one server node has gone offline)
    - Cons.
      - Weird API
      - Uni-directional, server to client only
      - Only text data, needs additional parsing on the frontend
    - How to communicate client to server if using SSE?
      - Server to client -> SSE
      - Client to server -> HTTP POST request (traffic overhead is one trade-off)

## 5. Data Entities and Store

## 6. Optimization

- Network performance
- Rendering performance
- JavaScript performance
- PWA (offline access)

## 7. Accessbility
