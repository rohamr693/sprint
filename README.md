

controller.up.onEvent(ControllerButtonEvent.Pressed, function () {
    direction = 0
})
controller.B.onEvent(ControllerButtonEvent.Pressed, function () {
    if (statusbar.value > 0) {
        sprint = 1
        controller.moveSprite(mySprite, dddd * 1.6, dddd * 1.6 - 5)
        statusbar.value += -10
        mySprite.startEffect(effects.trail, 1000)
    } else {
        controller.moveSprite(mySprite, dddd, dddd - 5)
    }
})
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    if (direction == 0) {
        projectile = sprites.createProjectileFromSprite(img`
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . 2 . . . . . . . . 
            . . . . . . . 2 . . . . . . . . 
            . . . . . . . 2 . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            `, mySprite, 0, -100)
    } else if (direction == 1) {
        projectile = sprites.createProjectileFromSprite(img`
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . 2 . . . . . . . . 
            . . . . . . . 2 . . . . . . . . 
            . . . . . . . 2 . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            `, mySprite, 0, 100)
    } else if (direction == 2) {
        projectile = sprites.createProjectileFromSprite(img`
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . 2 2 2 . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            `, mySprite, -100, 0)
    } else if (direction == 3) {
        projectile = sprites.createProjectileFromSprite(img`
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . 2 2 2 . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            . . . . . . . . . . . . . . . . 
            `, mySprite, 100, 0)
    }
})
controller.left.onEvent(ControllerButtonEvent.Pressed, function () {
    direction = 2
})
controller.B.onEvent(ControllerButtonEvent.Repeated, function () {
    if (statusbar.value > 0) {
        sprint = 1
        controller.moveSprite(mySprite, dddd * 1.6, dddd * 1.6 - 5)
        statusbar.value += -1
        mySprite.startEffect(effects.trail, 1000)
    } else {
        controller.moveSprite(mySprite, dddd, dddd - 5)
    }
})
controller.right.onEvent(ControllerButtonEvent.Pressed, function () {
    direction = 3
})
controller.down.onEvent(ControllerButtonEvent.Pressed, function () {
    direction = 1
})
controller.B.onEvent(ControllerButtonEvent.Released, function () {
    controller.moveSprite(mySprite, dddd, dddd - 5)
    timer.after(500, function () {
        sprint = 0
    })
})
sprites.onOverlap(SpriteKind.Projectile, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.changeDataNumberBy(otherSprite, "enemy lives", -1)
    sprites.destroy(sprite)
    if (sprites.readDataNumber(otherSprite, "enemy lives") <= 0) {
        sprites.destroy(otherSprite)
        info.changeScoreBy(1)
    }
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    game.gameOver(false)
})
let mySprite2: Sprite = null
let projectile: Sprite = null
let direction = 0
let sprint = 0
let statusbar: StatusBarSprite = null
let mySprite: Sprite = null
let dddd = 0
dddd = 65
tiles.setCurrentTilemap(tilemap`level1`)
mySprite = sprites.create(img`
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . 6 6 6 . . . . . . . . . 
    . . . . 6 6 6 . . . . . . . . . 
    . . . . 6 6 6 . . . . . . . . . 
    . . . 9 9 9 9 9 . . . . . . . . 
    . . . 9 9 9 9 9 . . . . . . . . 
    . . . . 9 9 9 . . . . . . . . . 
    . . . . 6 . 6 . . . . . . . . . 
    . . . . 6 . 6 . . . . . . . . . 
    `, SpriteKind.Player)
controller.moveSprite(mySprite, 65, 65)
scene.cameraFollowSprite(mySprite)
tiles.placeOnRandomTile(mySprite, assets.tile`myTile`)
statusbar = statusbars.create(20, 4, StatusBarKind.Energy)
statusbar.attachToSprite(mySprite, 0, 1)
statusbar.setStatusBarFlag(StatusBarFlag.SmoothTransition, true)
sprint = 0
statusbar.setColor(5, 7)
game.onUpdateInterval(1000, function () {
    mySprite2 = sprites.create(img`
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . . . . . . . . . . . . . 
        . . . . 4 4 4 . . . . . . . . . 
        . . . . 4 4 4 . . . . . . . . . 
        . . . . 4 4 4 . . . . . . . . . 
        . . . 2 2 2 2 2 . . . . . . . . 
        . . . 2 2 2 2 2 . . . . . . . . 
        . . . . 2 2 2 . . . . . . . . . 
        . . . . 4 . 4 . . . . . . . . . 
        . . . . 4 . 4 . . . . . . . . . 
        `, SpriteKind.Enemy)
    sprites.setDataNumber(mySprite2, "enemy lives", 2)
    mySprite2.follow(mySprite, 20)
    tiles.placeOnRandomTile(mySprite2, assets.tile`myTile`)
})
game.onUpdateInterval(100, function () {
    if (statusbar.value < statusbar.max && sprint != 1) {
        statusbar.value += 5
    }
})
