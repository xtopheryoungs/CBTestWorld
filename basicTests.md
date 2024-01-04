# Basic Agent Tests

## Step 1

Type "run" in chat to perform tests

```template
player.onChat("Blocked", function () {
    if (blockedTests.length > 0) {
        for (let value of blockedTests) {
            player.say(value)
        }
    } else {
        player.say("No tests were blocked")
    }
    player.say("-----------------")
})
function samePosition (p: Position, q: Position) {
    if (p.getValue(Axis.X) == q.getValue(Axis.X)) {
        if (p.getValue(Axis.Y) == q.getValue(Axis.Y)) {
            if (p.getValue(Axis.Z) == q.getValue(Axis.Z)) {
                return true
            }
        }
    }
    return false
}
function _00_teleportTo_position () {
    agent.teleport(HOME, NORTH)
    if (samePosition(agent.getPosition(), HOME)) {
        updateResults("agentTeleport / agentPosition", true)
    } else {
        updateResults("agentTeleport / agentPosition", false)
    }
    reset()
}
function inspectHelper (dir: string, block: boolean) {
    delay(1)
    if (block) {
        if (dir == "north") {
            return agent.inspect(AgentInspection.Block, FORWARD)
        } else if (dir == "south") {
            return agent.inspect(AgentInspection.Block, BACK)
        } else if (dir == "west") {
            return agent.inspect(AgentInspection.Block, LEFT)
        } else if (dir == "east") {
            return agent.inspect(AgentInspection.Block, RIGHT)
        } else if (dir == "up") {
            return agent.inspect(AgentInspection.Block, UP)
        } else {
            return agent.inspect(AgentInspection.Block, DOWN)
        }
    } else {
        if (dir == "north") {
            return agent.inspect(AgentInspection.Data, FORWARD)
        } else if (dir == "south") {
            return agent.inspect(AgentInspection.Data, BACK)
        } else if (dir == "west") {
            return agent.inspect(AgentInspection.Data, LEFT)
        } else if (dir == "east") {
            return agent.inspect(AgentInspection.Data, RIGHT)
        } else if (dir == "up") {
            return agent.inspect(AgentInspection.Data, UP)
        } else {
            return agent.inspect(AgentInspection.Data, DOWN)
        }
    }
}
function _06_inspect () {
    agent.teleport(HOME, NORTH)
    if (agent.getOrientation() == FACING_NORTH && samePosition(agent.getPosition(), HOME)) {
        for (let value of ABS_DIRECTION) {
            testBlock = GEM_BLOCKS[ABS_DIRECTION.indexOf(value)]
            blocks.place(testBlock, positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
            if (inspectHelper(value, true) == testBlock) {
                updateResults("inspect block: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
            } else {
                updateResults("inspect block: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
            }
            blocks.place(AIR, positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
        }
        for (let value of ABS_DIRECTION) {
            auxData = ABS_DIRECTION.indexOf(value)
            blocks.place(blocks.blockWithData(blocks.blockByName("observer"), auxData), positions.add(
            HOME,
            UNIT_VECTOR[auxData]
            ))
            if (inspectHelper(value, false) == auxData) {
                updateResults("inspect data: " + REL_DIRECTION[auxData], true)
            } else {
                updateResults("inspect data: " + REL_DIRECTION[auxData], false)
            }
            blocks.place(AIR, positions.add(
            HOME,
            UNIT_VECTOR[auxData]
            ))
        }
        blocks.place(GRASS, positions.add(
        HOME,
        pos(0, -1, 0)
        ))
    } else {
        blockedTests.push("agent inspect")
    }
    reset()
}
function placeHelper (dir: string) {
    if (dir == "north") {
        agent.place(FORWARD)
    } else if (dir == "south") {
        agent.place(BACK)
    } else if (dir == "west") {
        agent.place(LEFT)
    } else if (dir == "east") {
        agent.place(RIGHT)
    } else if (dir == "up") {
        agent.place(UP)
    } else {
        agent.place(DOWN)
    }
    delay(1)
}
function clearInventory () {
    agent.dropAll(FORWARD)
    player.execute(
    "kill @e[type=!agent]"
    )
}
function _04_turn () {
    agent.teleport(HOME, NORTH)
    if (agent.getOrientation() == FACING_NORTH) {
        agent.turn(RIGHT_TURN)
        if (agent.getOrientation() == FACING_EAST) {
            updateResults("turn: RIGHT (North -> East)", true)
            agent.turn(RIGHT_TURN)
            if (agent.getOrientation() == FACING_SOUTH) {
                updateResults("turn: RIGHT (East -> South)", true)
                agent.turn(RIGHT_TURN)
                if (agent.getOrientation() == FACING_WEST) {
                    updateResults("turn: RIGHT (South -> West)", true)
                    agent.turn(RIGHT_TURN)
                    if (agent.getOrientation() == FACING_NORTH) {
                        updateResults("turn: RIGHT (West -> North)", true)
                    } else {
                        updateResults("turn: RIGHT (West -> North)", false)
                    }
                } else {
                    updateResults("turn: RIGHT (South -> West)", false)
                }
            } else {
                updateResults("turn: RIGHT (East > South)", false)
            }
        } else {
            updateResults("turn: RIGHT (North -> East)", false)
        }
        agent.teleport(HOME, NORTH)
        agent.turn(LEFT_TURN)
        if (agent.getOrientation() == FACING_WEST) {
            updateResults("turn: LEFT (North -> West)", true)
            agent.turn(LEFT_TURN)
            if (agent.getOrientation() == FACING_SOUTH) {
                updateResults("turn: LEFT (West -> South)", true)
                agent.turn(LEFT_TURN)
                if (agent.getOrientation() == FACING_EAST) {
                    updateResults("turn: LEFT (South -> East)", true)
                    agent.turn(LEFT_TURN)
                    if (agent.getOrientation() == FACING_NORTH) {
                        updateResults("turn: LEFT (East -> North)", true)
                    } else {
                        updateResults("turn: LEFT (East -> North)", false)
                    }
                } else {
                    updateResults("turn: LEFT (South -> East)", false)
                }
            } else {
                updateResults("turn: LEFT (West -> South)", false)
            }
        } else {
            updateResults("turn: LEFT (North -> West)", false)
        }
    } else {
        blockedTests.push("agent turn")
    }
    reset()
}
player.onChat("Passed", function () {
    if (passedTests.length > 0) {
        for (let value of passedTests) {
            player.say(value)
        }
    } else {
        player.say("No tests passed")
    }
    player.say("-----------------")
})
function _08_place () {
    agent.teleport(HOME, NORTH)
    clearInventory()
    testBlock = BRICKS
    blocks.place(AIR, positions.add(
    HOME,
    pos(0, -1, 0)
    ))
    for (let value of ABS_DIRECTION) {
        agent.setItem(testBlock, 1, 1)
        placeHelper(value)
        if (blocks.testForBlock(testBlock, positions.add(
        HOME,
        UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
        ))) {
            updateResults("place: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
        } else {
            updateResults("place: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
        }
    }
    reset()
}
function _01_orientation () {
    agent.teleport(HOME, EAST)
    if (agent.getOrientation() == FACING_EAST) {
        updateResults("agentOrientation: EAST", true)
    } else {
        updateResults("agentOrientation: EAST", false)
    }
    agent.teleport(HOME, SOUTH)
    if (agent.getOrientation() == FACING_SOUTH) {
        updateResults("agentOrientation: SOUTH", true)
    } else {
        updateResults("agentOrientation: SOUTH", false)
    }
    agent.teleport(HOME, WEST)
    if (agent.getOrientation() == FACING_WEST) {
        updateResults("agentOrientation: WEST", true)
    } else {
        updateResults("agentOrientation: WEST", false)
    }
    agent.teleport(HOME, NORTH)
    if (agent.getOrientation() == FACING_NORTH) {
        updateResults("agentOrientation: NORTH", true)
    } else {
        updateResults("agentOrientation: NORTH", false)
    }
    reset()
}
function _07_inventory () {
    clearInventory()
    testBlock = STONE
    agent.setItem(testBlock, INV_FULL_STACK, INV_SLOT_SOURCE)
    inv_getItemCount = agent.getItemCount(INV_SLOT_SOURCE) == INV_FULL_STACK
    inv_getItemID = agent.getItemDetail(INV_SLOT_SOURCE) == testBlock
    if (inv_getItemCount) {
        updateResults("get item count", true)
    } else {
        updateResults("get item count", false)
    }
    if (inv_getItemID) {
        updateResults("get item id", true)
    } else {
        updateResults("get item id", false)
    }
    if (inv_getItemCount && inv_getItemID) {
        updateResults("set block or item", true)
        agent.transfer(INV_TRANSFER_COUNT, INV_SLOT_SOURCE, INV_SLOT_DESTINATION)
        inv_getRemainingSpace = agent.getItemSpace(INV_SLOT_SOURCE) == INV_TRANSFER_COUNT
        inv_getItemCount = agent.getItemCount(INV_SLOT_DESTINATION) == INV_TRANSFER_COUNT
        if (inv_getRemainingSpace) {
            updateResults("get remaining space", true)
        } else {
            updateResults("get remaining space", false)
        }
        if (inv_getItemCount && inv_getRemainingSpace) {
            updateResults("transfer amount", true)
            inv_keepSlot = agent.getItemCount(INV_SLOT_DESTINATION)
            agent.drop(FORWARD, INV_SLOT_SOURCE, INV_DROP_COUNT)
            inv_getRemainingSpace = agent.getItemSpace(INV_SLOT_SOURCE) == INV_TRANSFER_COUNT + INV_DROP_COUNT
            inv_getItemCount = inv_keepSlot == agent.getItemCount(INV_SLOT_DESTINATION)
            if (inv_getRemainingSpace && inv_getItemCount) {
                updateResults("drop from slot", true)
                for (let index = 0; index <= 26; index++) {
                    agent.setItem(testBlock + index % 3, 1, index + 1)
                }
                agent.dropAll(DOWN)
                inv_sum = 0
                for (let index = 0; index <= 26; index++) {
                    inv_sum += agent.getItemCount(index + 1)
                }
                if (inv_sum == 0) {
                    updateResults("drop all", true)
                    agent.collect(testBlock + 1)
                    inv_getItemCount = agent.getItemCount(1) > 0
                    inv_getItemID = agent.getItemDetail(1) == testBlock + 1
                    inv_getRemainingSpace = agent.getItemSpace(2) == INV_FULL_STACK
                    if (inv_getItemCount && inv_getItemID && inv_getRemainingSpace) {
                        updateResults("collect item", true)
                        agent.collectAll()
                        if (agent.getItemCount(2) > 0) {
                            updateResults("collect all", true)
                        } else {
                            updateResults("collect all", false)
                        }
                    } else {
                        updateResults("collect item", false)
                        blockedTests.push("collect all")
                    }
                } else {
                    updateResults("drop all", false)
                    blockedTests.push("collect item")
                    blockedTests.push("collect all")
                }
            } else {
                updateResults("drop from slot", false)
                blockedTests.push("drop all")
                blockedTests.push("collect item")
                blockedTests.push("collect all")
            }
        } else {
            updateResults("transfer amount", false)
            blockedTests.push("drop from slot")
            blockedTests.push("drop all")
            blockedTests.push("collect item")
            blockedTests.push("collect all")
        }
    } else {
        updateResults("set block or item", false)
        blockedTests.push("transfer amount")
        blockedTests.push("drop from slot")
        blockedTests.push("drop all")
        blockedTests.push("collect item")
        blockedTests.push("collect all")
    }
    reset()
}
function _05_detect () {
    agent.teleport(HOME, NORTH)
    if (agent.getOrientation() == FACING_NORTH && samePosition(agent.getPosition(), HOME)) {
        for (let value of ABS_DIRECTION) {
            blocks.place(GLASS, positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
            if (detectHelper(value, true)) {
                updateResults("detect block (true): " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
            } else {
                updateResults("detect block (true): " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
            }
            blocks.place(AIR, positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
        }
        for (let value of ABS_DIRECTION) {
            blocks.place(blocks.blockWithData(LEVER, 13), positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
            if (detectHelper(value, false)) {
                updateResults("detect redstone (true): " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
            } else {
                updateResults("detect redstone (true): " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
            }
            blocks.place(AIR, positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
        }
        for (let value of ABS_DIRECTION) {
            blocks.place(COBWEB, positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
            if (!(detectHelper(value, false))) {
                updateResults("detect redstone (false): " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
            } else {
                updateResults("detect redstone (false): " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
            }
            blocks.place(AIR, positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
        }
        for (let value of ABS_DIRECTION) {
            blocks.place(AIR, positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
            if (!(detectHelper(value, true))) {
                updateResults("detect block (false): " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
            } else {
                updateResults("detect block (false): " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
            }
        }
        blocks.place(GRASS, positions.add(
        HOME,
        pos(0, -1, 0)
        ))
    } else {
        blockedTests.push("agent detect")
    }
    reset()
}
function updateResults (testName: string, passed: boolean) {
    runTotal += 1
    if (passed) {
        passTotal += 1
        passedTests.push(testName)
    } else {
        failedTests.push(testName)
    }
}
function destroyHelper (dir: string) {
    if (dir == "north") {
        agent.destroy(FORWARD)
    } else if (dir == "south") {
        agent.destroy(BACK)
    } else if (dir == "west") {
        agent.destroy(LEFT)
    } else if (dir == "east") {
        agent.destroy(RIGHT)
    } else if (dir == "up") {
        agent.destroy(UP)
    } else {
        agent.destroy(DOWN)
    }
    delay(1)
}
function detectHelper (dir: string, block: boolean) {
    delay(1)
    if (block) {
        if (dir == "north") {
            return agent.detect(AgentDetection.Block, FORWARD)
        } else if (dir == "south") {
            return agent.detect(AgentDetection.Block, BACK)
        } else if (dir == "west") {
            return agent.detect(AgentDetection.Block, LEFT)
        } else if (dir == "east") {
            return agent.detect(AgentDetection.Block, RIGHT)
        } else if (dir == "up") {
            return agent.detect(AgentDetection.Block, UP)
        } else {
            return agent.detect(AgentDetection.Block, DOWN)
        }
    } else {
        if (dir == "north") {
            return agent.detect(AgentDetection.Redstone, FORWARD)
        } else if (dir == "south") {
            return agent.detect(AgentDetection.Redstone, BACK)
        } else if (dir == "west") {
            return agent.detect(AgentDetection.Redstone, LEFT)
        } else if (dir == "east") {
            return agent.detect(AgentDetection.Redstone, RIGHT)
        } else if (dir == "up") {
            return agent.detect(AgentDetection.Redstone, UP)
        } else {
            return agent.detect(AgentDetection.Redstone, DOWN)
        }
    }
}
function _09_destroy () {
    agent.teleport(HOME, NORTH)
    for (let value of ABS_DIRECTION) {
        blocks.place(PURPUR_BLOCK, positions.add(
        HOME,
        UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
        ))
        destroyHelper(value)
        if (blocks.testForBlock(AIR, positions.add(
        HOME,
        UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
        ))) {
            updateResults("destroy: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
        } else {
            updateResults("destroy: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
            blocks.place(AIR, positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))
        }
    }
    reset()
}
player.onChat("run", function () {
    player.teleport(world(6, 4, 1))
    passTotal = 0
    runTotal = 0
    failedTests = []
    passedTests = []
    blockedTests = []
    clearInventory()
    _00_teleportTo_position()
    _01_orientation()
    _02_tpToPlayer()
    _03_move()
    _04_turn()
    _05_detect()
    _06_inspect()
    _07_inventory()
    _08_place()
    _09_destroy()
    _10_till()
    _11_interact()
    _12_other()
    player.say("-----RESULTS-----")
    player.say("Passed: " + passedTests.length)
    player.say("Failed: " + failedTests.length)
    player.say("Blocked: " + blockedTests.length)
    player.say("-----------------")
    player.say("Type \"passed\" / \"failed\" / \"blocked\" to see which tests")
    player.teleport(world(8, 4, 7))
})
function moveHelper (dir: string) {
    if (dir == "north") {
        agent.move(FORWARD, 1)
    } else if (dir == "south") {
        agent.move(BACK, 1)
    } else if (dir == "west") {
        agent.move(LEFT, 1)
    } else if (dir == "east") {
        agent.move(RIGHT, 1)
    } else if (dir == "up") {
        agent.move(UP, 1)
    } else {
        agent.move(DOWN, 1)
    }
}
player.onChat("Failed", function () {
    if (failedTests.length > 0) {
        for (let value of failedTests) {
            player.say(value)
        }
    } else {
        player.say("No tests failed")
    }
    player.say("-----------------")
})
function _10_till () {
    agent.teleport(HOME, NORTH)
    testBlock = GRASS
    tillXYZ = world(0, 0, 0)
    blocks.fill(
    testBlock,
    positions.add(
    HOME,
    pos(-1, -1, -1)
    ),
    positions.add(
    HOME,
    pos(1, -1, 1)
    ),
    FillOperation.Replace
    )
    blocks.place(AIR, positions.add(
    HOME,
    pos(0, -1, 0)
    ))
    for (let value of ABS_DIRECTION) {
        if (value == "up") {
            blocks.place(GRASS, positions.add(
            HOME,
            pos(0, 1, 0)
            ))
            tillXYZ = positions.add(
            HOME,
            pos(0, 1, 0)
            )
        } else {
            tillXYZ = positions.add(
            HOME,
            positions.add(
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)],
            pos(0, -1, 0)
            )
            )
        }
        tillHelper(value)
        if (blocks.testForBlock(blocks.blockByName("farmland"), tillXYZ)) {
            updateResults("till: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
        } else {
            updateResults("till: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
        }
    }
    reset()
}
function tillHelper (dir: string) {
    if (dir == "north") {
        agent.till(FORWARD)
    } else if (dir == "south") {
        agent.till(BACK)
    } else if (dir == "west") {
        agent.till(LEFT)
    } else if (dir == "east") {
        agent.till(RIGHT)
    } else if (dir == "up") {
        agent.till(UP)
    } else {
        agent.till(DOWN)
    }
    delay(1)
}
function _12_other () {
    clearInventory()
    agent.teleport(HOME, NORTH)
    if (samePosition(HOME, agent.getPosition())) {
        testBlock = STONE
        wrongBlock = BOOKSHELF
        for (let index = 0; index <= 26; index++) {
            agent.setItem(wrongBlock, INV_FULL_STACK, index + 1)
        }
        agent.setItem(testBlock, INV_FULL_STACK, INV_SLOT_SOURCE)
        agent.setSlot(INV_SLOT_SOURCE)
        agent.setAssist(PLACE_ON_MOVE, false)
        agent.move(UP, 1)
        if (blocks.testForBlock(AIR, HOME)) {
            updateResults("place on move (false)", true)
        } else {
            updateResults("place on move (false)", false)
        }
        agent.setAssist(PLACE_ON_MOVE, true)
        agent.move(DOWN, 1)
        if (blocks.testForBlock(testBlock, positions.add(
        HOME,
        pos(0, 1, 0)
        ))) {
            updateResults("place on move (true)", true)
            updateResults("set active slot", true)
        } else {
            updateResults("place on move (true)", false)
            updateResults("set active slot", false)
        }
        agent.setSlot(1)
        blocks.place(PLANKS_OAK, positions.add(
        HOME,
        pos(0, 1, 0)
        ))
        agent.setAssist(PLACE_ON_MOVE, false)
        agent.teleport(HOME, NORTH)
        agent.setAssist(DESTROY_OBSTACLES, false)
        agent.move(UP, 1)
        if (samePosition(HOME, agent.getPosition())) {
            updateResults("destroy obstacles (false)", true)
            agent.setAssist(DESTROY_OBSTACLES, true)
            agent.move(UP, 1)
            if (samePosition(positions.add(
            HOME,
            pos(0, 1, 0)
            ), agent.getPosition())) {
                updateResults("destroy obstacles (true)", true)
            } else {
                updateResults("destroy obstacles (true)", false)
            }
        } else {
            updateResults("destroy obstacles (false)", false)
            blockedTests.push("destroy obstacles (true)")
        }
    } else {
        blockedTests.push("place on move")
        blockedTests.push("set active slot")
        blockedTests.push("destroy obstacles")
    }
    reset()
}
function _03_move () {
    agent.teleport(HOME, NORTH)
    blocks.place(AIR, positions.add(
    HOME,
    pos(0, -1, 0)
    ))
    if (agent.getOrientation() == FACING_NORTH) {
        for (let value of ABS_DIRECTION) {
            agent.teleport(HOME, NORTH)
            moveHelper(value)
            if (samePosition(agent.getPosition(), positions.add(
            HOME,
            UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
            ))) {
                updateResults("move: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
            } else {
                updateResults("move: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
            }
        }
    } else {
        blockedTests.push("agent move")
    }
    reset()
}
function delay (ticks: number) {
    for (let index = 0; index < ticks; index++) {
        if (blocks.testForBlock(GRASS, pos(0, 0, 0))) {
        	
        }
    }
}
function _02_tpToPlayer () {
    agent.teleportToPlayer()
    if (samePosition(agent.getPosition(), player.position())) {
        updateResults("tpToPlayer / player position", true)
    } else {
        updateResults("tpToPlayer / player position", false)
    }
    reset()
}
function reset () {
    delay(4)
    blocks.fill(
    AIR,
    positions.add(
    HOME,
    pos(-1, 0, -1)
    ),
    positions.add(
    HOME,
    pos(1, 1, 1)
    ),
    FillOperation.Replace
    )
    blocks.fill(
    GRASS,
    positions.add(
    HOME,
    pos(-1, -1, -1)
    ),
    positions.add(
    HOME,
    pos(1, -1, 1)
    ),
    FillOperation.Replace
    )
    agent.teleport(HOME, NORTH)
    clearInventory()
    delay(4)
}
function interactHelper (dir: string) {
    delay(2)
    if (dir == "north") {
        agent.interact(FORWARD)
    } else if (dir == "south") {
        agent.interact(BACK)
    } else if (dir == "west") {
        agent.interact(LEFT)
    } else if (dir == "east") {
        agent.interact(RIGHT)
    } else if (dir == "up") {
        agent.interact(UP)
    } else {
        agent.interact(DOWN)
    }
    delay(2)
}
function _11_interact () {
    agent.teleport(HOME, NORTH)
    for (let value of ABS_DIRECTION) {
        blocks.place(REDSTONE_LAMP, HOME)
        blocks.place(blocks.blockWithData(LEVER, 5), positions.add(
        HOME,
        UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
        ))
        interactHelper(value)
        if (blocks.testForBlock(blocks.blockByName("lit_redstone_lamp"), HOME)) {
            updateResults("interact: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], true)
        } else {
            updateResults("interact: " + REL_DIRECTION[ABS_DIRECTION.indexOf(value)], false)
        }
        blocks.fill(
        AIR,
        HOME,
        positions.add(
        HOME,
        UNIT_VECTOR[ABS_DIRECTION.indexOf(value)]
        ),
        FillOperation.Replace
        )
    }
    reset()
}
let wrongBlock = 0
let tillXYZ: Position = null
let failedTests: string[] = []
let passTotal = 0
let runTotal = 0
let inv_sum = 0
let inv_keepSlot = 0
let inv_getRemainingSpace = false
let inv_getItemID = false
let inv_getItemCount = false
let passedTests: string[] = []
let auxData = 0
let testBlock = 0
let blockedTests: string[] = []
let INV_DROP_COUNT = 0
let INV_SLOT_SOURCE = 0
let INV_SLOT_DESTINATION = 0
let INV_TRANSFER_COUNT = 0
let INV_FULL_STACK = 0
let GEM_BLOCKS: number[] = []
let REL_DIRECTION: string[] = []
let ABS_DIRECTION: string[] = []
let UNIT_VECTOR: Position[] = []
let FACING_WEST = 0
let FACING_SOUTH = 0
let FACING_EAST = 0
let FACING_NORTH = 0
let HOME: Position = null
player.say("=====BEGIN=====")
player.say("Type \"run\" to perform tests")
HOME = world(0, 4, 0)
agent.teleport(HOME, NORTH)
FACING_NORTH = -180
FACING_EAST = -90
FACING_SOUTH = 0
FACING_WEST = 90
UNIT_VECTOR = [
pos(0, 0, -1),
pos(0, 0, 1),
pos(-1, 0, 0),
pos(1, 0, 0),
pos(0, 1, 0),
pos(0, -1, 0)
]
ABS_DIRECTION = [
"north",
"south",
"west",
"east",
"up",
"down"
]
REL_DIRECTION = [
"FORWARD",
"BACK",
"LEFT",
"RIGHT",
"UP",
"DOWN"
]
GEM_BLOCKS = [
GOLD_BLOCK,
IRON_BLOCK,
DIAMOND_BLOCK,
EMERALD_BLOCK,
LAPIS_LAZULI_BLOCK,
COAL_BLOCK
]
INV_FULL_STACK = 64
INV_TRANSFER_COUNT = 4
INV_SLOT_DESTINATION = 3
INV_SLOT_SOURCE = 5
INV_DROP_COUNT = 2

```
    
