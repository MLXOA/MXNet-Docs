# Protocol
<p>
    This topic will define how the MXNet protocol works and is used.
</p>

<deflist>
    <def title="UUID">a universally unique identifier, also known as a GUID on Microsoft systems.</def>
</deflist>

The MXNet protocol is mostly based on requests, 

```mermaid
stateDiagram-v2
    DurableObject: Durable Object
    state DurableObject {
        DurableObjectRequested: Received Request From Worker
        GUID: Generate UUID
        IsUUIDUsed?: Is this UUID already used?
        UUIDIsUsed: Yes, Generate a new UUID
        IsNewUUIDUsed?: Is this UUID already used?
        NewUUIDUsedResponse: Yes, Respond with HTTP 409 (Conflict)
    
        [*] --> DurableObjectRequested
        DurableObjectRequested --> GUID
        GUID --> IsUUIDUsed?
        IsUUIDUsed? --> UUIDIsUsed
        UUIDIsUsed --> IsNewUUIDUsed?
        IsNewUUIDUsed? --> NewUUIDUsedResponse
        IsNewUUIDUsed? --> AddUserToLists

        IsUUIDUsed? --> AddUserToLists
    
        AddUserToLists: No, Add Player To Users and UUIDs lists
        AcceptWebsocket: Accept the WebSocket connection and Respond

        AddUserToLists --> AcceptWebsocket
        AcceptWebsocket --> [*]
    }
```