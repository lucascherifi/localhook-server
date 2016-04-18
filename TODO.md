Todo
====

Symfony Server (2.8 LTS)
------------------------
```
Vendors
  https://github.com/Wisembly/elephant.io
Controller
  AdminController
    ListAction # List all webhooks # ADMIN_ROLE
    NewAction # USER_ROLE
    AddAction # USER_ROLE / Emit "CreateChannel" message
    EditAction # USER_ROLE
    DeleteAction # USER_ROLE
  NotificationController
    NotificationsAction # ANONYMOUS_ROLE
Entity
  Webhook
    id
    hash
    endpoint
    userRole
Command
  StartServer # start the socket.io server + Emit "CreateChannel" message for every Webhook entities
  StopServer # stop the socket.io server
```

Socket.io Server
----------------

```
  Events
    CreateChannel # Add a channel
    GetConfigurationFromHash(hash) # Return configuration (hash, endpoint)
    SubscribeChannel # Link a client to a channel
    UnsubscribeChannel # Unlink a client to a channel
    EmitNotification # Emit a notification to every subscribed channels
```

Client
------
```
  Vendors
    https://github.com/Wisembly/elephant.io
    guzzle
  Commands
    configure-channel
      - Ask hash
      - Emit "GetConfigurationFromHash" message
      - Handle error (not found)
      - Store configuration in a local yaml file
    run
      - emit "SubscribeChannel" message (hash, id)
      - handle "Notification" message (then execute Guzzle requests)
    stop # emit "UnsubscribeChannel" message
  Install
    composer global require
```