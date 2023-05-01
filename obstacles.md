# Obstacles

## Step 1

Type "run" in chat to perform tests

```template
let start: Position = null
let slot = 0
let amount = 0
player.onChat("run", function () {
    start = world(5, 4, -7)
    agent.teleport(start, WEST)
    agent.teleportToPlayer()
    agent.dropAll(FORWARD)
    slot = 4
    amount = 2
    agent.teleport(positions.add(
    start,
    pos(0, 2, 0)
    ), EAST)
    agent.setItem(GRASS, amount, 1)
    for (let index = 0; index < 4; index++) {
        agent.turn(LEFT_TURN)
    }
    agent.dropAll(UP)
    for (let index = 0; index < 4; index++) {
        agent.turn(RIGHT_TURN)
    }
    agent.collectAll()
    agent.move(FORWARD, 2)
    agent.collectAll()
    for (let index = 0; index < 2; index++) {
        agent.move(FORWARD, 1)
        agent.move(DOWN, 1)
    }
    for (let index = 0; index < 6; index++) {
        if (agent.detect(AgentDetection.Block, FORWARD)) {
            agent.destroy(FORWARD)
        }
        agent.move(FORWARD, 1)
    }
    while (!(agent.detect(AgentDetection.Block, UP))) {
        agent.move(UP, 1)
    }
    agent.move(FORWARD, 1)
    while (agent.inspect(AgentInspection.Block, DOWN) == blocks.blockByName("allow")) {
        agent.interact(FORWARD)
        agent.move(FORWARD, 1)
    }
    agent.turn(RIGHT_TURN)
    agent.transfer(amount, 1, slot)
    agent.setSlot(slot)
    agent.place(LEFT)
    agent.move(UP, agent.getItemCount(slot))
    agent.turn(RIGHT_TURN)
    for (let index = 0; index < 4; index++) {
        agent.till(FORWARD)
        agent.move(FORWARD, 1)
    }
    while (!(agent.detect(AgentDetection.Block, FORWARD))) {
        agent.move(FORWARD, 1)
    }
    agent.setItem(AIR, 1, slot)
    agent.interact(UP)
})


```
    
