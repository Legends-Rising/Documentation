---
icon: ferris-wheel
---

# lr-menu

## Introduction

Welcome to the **lr-menu** documentation. This guide will help you understand how to create, manipulate, and destroy menus using the `lr-menu` resource. With `lr-menu`, you can build dynamic, draggable, multi-page menus that include various interactive elements such as buttons, switches, inputs, sliders, and more.

### What is `lr-menu`?

`lr-menu` is a configurable menu system that makes it easy to create intuitive and user-friendly UI menus in your Redm scripts. It provides functions to:

* Initiate and register menus
* Add and remove items dynamically
* Handle user inputs (checkboxes, text inputs, sliders, etc.)
* Navigate between multiple pages
* Update, close, and destroy menus

### Prerequisites

* FiveM server environment
* Basic knowledge of Lua
* Familiarity with registering commands and exporting functions in resource scripts

***

## Getting Started

### Initiating the Menu Manager

Before creating any menus, you need to initiate the `MenuManager` provided by `lr-menu`.

```etlua
MenuManager = exports['lr-menu']:initiate()
```

This exports call returns a `MenuManager` object that you will use to register and manipulate menus.

***

## Creating and Registering a Menu

To create a new menu, use the `MenuManager:registerMenu()` function. This will return a `Menu` object that you can store and modify later.

```lua
local testMenu = MenuManager:registerMenu({
    header = 'Test Menu',            -- The main header/title of the menu
    description = 'This menu is created using exports.',
    draggable = true,                -- Allows the user to drag the menu around the screen
    canExit = true,                  -- Whether the user can close the menu
    items = {
        {
            label = 'Test Button',
            smallButton = true,
            effect = function()
                print('Test Button Pressed!')
            end
        }
    }
})
```

#### Properties

* **header**: The main title displayed at the top of the menu.
* **description**: A subheader or subtitle giving context or instructions.
* **draggable**: A boolean value indicating if the menu can be clicked and dragged.
* **canExit**: A boolean indicating if the menu can be closed by the user.
* **items**: A table of items to display in the menu. Each item can be a button, switch, checkbox, input, etc.

***

## Adding Items Dynamically

You are not limited to defining all items when registering the menu. You can add items at any time using `Menu:addItem()`.

### Switch Item

A switch provides a toggleable on/off state.

```lua
testMenu:addItem({
    id = 'switch1',
    label = 'Test Switch',
    switch = true,
}, function(data)
    print(data.checked)  -- 'true' if on, 'false' if off
end)
```

### Small Button

A small button is a clickable item that can trigger a function when pressed.

```lua
testMenu:addItem({
    id = 'button1',
    label = 'Test Button',
    smallButton = true,
}, function()
    print('clicked')
end)
```

### Normal Button

Similar to a small button, but often styled or sized differently.

```lua
testMenu:addItem({
    id = 'button2',
    label = 'Test Button',
}, function()
    print('clicked')
end)
```

### HTML Content

You can insert custom HTML content within the menu.

```lua
testMenu:addItem({
    id = 'html1',
    html = '<p>This is a paragraph</p>'
})
```

### Checkbox

Check or uncheck a box and capture the state change.

```lua
testMenu:addItem({
    label = 'Test Checkbox',
    checked = false,
}, function(data)
    print('Checkbox state:', data.checked)
end)
```

### Input Box

Collect user input via a text field.

```lua
testMenu:addItem({
    label = 'Gang Colors Hex Code',
    placeholder = 'Type something...',
}, function(data)
    print('Input value:', data.inputValue)
end)
```

### Range Box

A numeric slider allowing selection of a value within a specified range.

```lua
testMenu:addItem({
    label = 'Test Range',
    min = 0,
    max = 100,
    step = 1,
    value = 50,
}, function(data)
    print('Range value:', data.value)
end)
```

### Slider with Multiple Options

A slider to select from predefined options, not just numeric values.

```lua
testMenu:addItem({
    label = 'Test Slider',
    items = {
        { label = 'Option 1', data = 'option1' },
        { label = 'Option 2', data = 'option2' },
        { label = 'Option 3', data = 'option3' }
    },
}, function(data)
    print('Selected item:', data.value)
end)
```

***

## Removing Items

Items can be removed by referencing their unique `id`.

```lua
testMenu:removeItem('button1')
testMenu:removeItem('switch1')
```

***

## Navigational Arrows

Arrows can be added to indicate pagination or navigation between multiple screens.

```lua
testMenu:addArrows({
    current = 2,
    total = 2,
}, function(data, self)
    print(data.currentPage)
end)
```

***

## Updating Items

You can update an existing item’s properties without recreating it.

```lua
testMenu:updateItem('button1', {
    label = 'Updated label',
})
```

***

## Closing and Destroying Menus

Close a menu while retaining its data:

```lua
testMenu:close()
```

Destroy a menu entirely, freeing its data from memory:

```lua
testMenu:destroy()
```

***

## Multi-Page Menu Example

`lr-menu` supports multiple pages within a single menu. Each page can have its own header, items, and navigation arrows. The example below creates a main menu and multiple sub-pages linked to it.

```lua
local totalPages = 5
local mainMenu = MenuManager:registerMenu({
    id = 'mainMenu',
    header = 'Menu 1',
    description = 'This menu is created using exports.',
    draggable = true,
    canExit = true,
    items = {
        {
            label = 'Test Button',
            effect = function()
                page2:openMenu()
            end
        },
        {
            label = 'Test Checkbox',
            checked = false,
            effect = function(data)
                print('Checkbox state:', data.checked)
            end
        },
        {
            label = 'Test Input',
            placeholder = 'Type something...',
            effect = function(data)
                print('Input value:', data.inputValue)
            end
        },
        {
            label = 'Test Range',
            min = 0,
            max = 100,
            step = 1,
            value = 50,
            effect = function(data)
                print('Selected item:', data.value)
            end
        },
    }
})

local page2 = MenuManager:registerMenu({
    header = 'Menu 2',
    pageFor = 'mainMenu',    -- Links this page to mainMenu
    description = 'Menu 2.',
    draggable = true,
    canExit = true,
    arrows = { current = 2, total = totalPages },
    items = {
        {
            id = 'switch1',
            label = 'Test Switch',
            switch = true,
            effect = function(data)
                print(data.checked)
            end
        },
    }
})

-- Additional pages...
-- page3, page4, page5 defined similarly with 'pageFor = "mainMenu"'

mainMenu:addArrows({ current = 1, total = totalPages })
```

In this multi-page setup:

* `pageFor = 'mainMenu'` links the sub-page to the main menu.
* Arrow navigation (`addArrows()`) updates the displayed page.
* Each page can have unique items and behaviors.

***

## Opening the Menu via a Command

You can register a server/client command to open a specific menu:

```lua
RegisterCommand('openmenulr', function()
    testMenu:openMenu()
end, false)
```

Executing `/openmenulr` in-game will open `testMenu`.

***

## Conclusion

You now have all the basics for using `lr-menu`:

* Initiate and register menus
* Add and remove items dynamically
* Implement various UI elements (buttons, checkboxes, sliders, inputs)
* Manage multiple pages and navigate between them
* Update, close, and destroy menus when they’re no longer needed

Use this guide as a reference while building more complex and interactive in-game UIs with `lr-menu`.
