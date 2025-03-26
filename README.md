
# TOURNAMENTS

A description of how to use the tournament service


## Tournament Namespace

Firstly, connect to the tournament Namespace, This would be on waiting for fixture page.

```
const socket = io("<URL>/tournament", {
    query: {
        userId: <USERID>,
        tournamentId: <TOURNAMENTID>,
    }
})
```

The listen to these events

- `matched` - when a player has be matched with another player. returns a lobby code {lobbyCode: 2129818}

- `error` - on any error. returns a message {message: string}

When the `matched` event is called, store the lobby code and send them to the page with the "play" button.

Events to emit;
- `tournament-fixture-completed` - (userId): emit this event after being redirected from a tournament fixture after completion. when this event is emitted, it should mean a fixture has finished and a player can be matched again.

#### Note - This event should only be emitted when a redirect has occured that brought them back to the waiting for fixture page. so the event shouldn't be emitted when they have firstly open the page.


### Before Play Events

This is the page with the play button. The events to emit on this page are;

- `join-tournament-waiting-room` - when a user navigates to this page, emit this event with the params (userId, lobbyCode)

- `leave-tournament-waiting-room` - when a user leaves or disconnects, emit this event with the params (userId, lobbyCode)


the events to listen to on this page are;

- `opponent-not-active` - when an opponent is not online or on the same page with the play button, this event is emitted and a timer starts on the server.

- `opponent-not-available` - when a timer has elapsed and the opponent is not ready, this event is emitted signalling the opponents forfeiture.

- `start-tournament-fixture` - when an both opponents are on online and are on the same page, this event is emitted prompting the start of the tournament fixture by you then activating the play button.

- `error` - on any error. returns a message {message: string}
