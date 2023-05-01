# Event Triggers

## Step 1

Perform the different actions to run the tests and activate the redstone lamps

```template
player.onTellCommand("tell", function () {
    eventFired("on tell command")
})
mobs.onMobKilled(CHICKEN, function () {
    eventFired("on animal killed")
})
player.onDied(function () {
    loops.pause(1000)
    eventFired("on player died")
})
player.onTravelled(SWIM_LAVA, function () {
    eventFired("on player swim lava")
})
player.onItemInteracted(SNOWBALL, function () {
    eventFired("on item used")
})
blocks.onBlockPlaced(GLASS, function () {
    eventFired("on block placed")
})
player.onTravelled(SWIM_WATER, function () {
    eventFired("on player swim water")
})
player.onTravelled(CLIMB, function () {
    eventFired("on player climb")
})
function initializeEventNames () {
    eventNames = [
    "on start",
    "run in background",
    "forever",
    "on chat command",
    "on chat command arg",
    "on tell command",
    "on block placed",
    "on block broken",
    "on item used",
    "on arrow shot",
    "on animal killed",
    "on player walk",
    "on player sprint",
    "on player sneak",
    "on player fall",
    "on player fly",
    "on player bounce",
    "on player riding",
    "on player climb",
    "on player swim water",
    "on player swim lava",
    "on player teleported",
    "on player died"
    ]
}
player.onTravelled(BOUNCE, function () {
    eventFired("on player bounce")
})
player.onChat("chat", function () {
    eventFired("on chat command")
})
function eventFired (eventName: string) {
    if (!(results[eventNames.indexOf(eventName)])) {
        player.say(eventName)
        results[eventNames.indexOf(eventName)] = true
        blocks.place(REDSTONE_BLOCK, positions.add(
        ANCHOR,
        pos(0, 0, eventNames.indexOf(eventName))
        ))
        count += 1
    }
}
player.onChat("test", function () {
    ii = 0
    for (let index = 0; index < EVENT_TOTAL; index++) {
        player.say("" + results[ii] + ": " + eventNames[ii])
        ii += 1
    }
})
player.onArrowShot(function () {
    eventFired("on arrow shot")
})
player.onTravelled(WALK, function () {
    eventFired("on player walk")
})
loops.runInBackground(function () {
    loops.pause(3000)
    eventFired("run in background")
})
player.onTravelled(FALL, function () {
    eventFired("on player fall")
})
player.onChat("count", function (num1) {
    ii = 0
    for (let index = 0; index < num1; index++) {
        ii += 1
        player.say(ii)
    }
    if (ii == num1) {
        eventFired("on chat command arg")
    }
})
loops.forever(function () {
    loops.pause(1000)
    foreverCount += 1
    if (foreverCount == 5) {
        eventFired("forever")
    }
})
player.onTravelled(RIDING, function () {
    eventFired("on player riding")
})
blocks.onBlockBroken(GLASS, function () {
    eventFired("on block broken")
})
player.onTravelled(FLY, function () {
    eventFired("on player fly")
})
player.onTravelled(SNEAK, function () {
    eventFired("on player sneak")
})
player.onTravelled(SPRINT, function () {
    eventFired("on player sprint")
})
player.onTeleported(function () {
    eventFired("on player teleported")
})
let ii = 0
let results: boolean[] = []
let EVENT_TOTAL = 0
let eventNames: string[] = []
let ANCHOR: Position = null
let foreverCount = 0
player.say("==============")
player.say("resetting...")
let count = 0
foreverCount = 0
ANCHOR = world(7, 3, 9)
eventNames = []
initializeEventNames()
EVENT_TOTAL = eventNames.length
results = []
for (let index = 0; index < EVENT_TOTAL; index++) {
    results.push(false)
}
blocks.fill(
GRASS,
ANCHOR,
positions.add(
ANCHOR,
pos(0, 0, EVENT_TOTAL)
),
FillOperation.Replace
)
player.say("begin tests")
loops.pause(1000)
eventFired("on start")


```
    
