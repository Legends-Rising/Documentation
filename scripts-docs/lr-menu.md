```markdown
---
icon: ferris-wheel
---

# lr-menu

## Introduction

**lr-menu** is a flexible and user-friendly menu system designed for RedM scripts. It simplifies the process of creating dynamic, draggable, multi-page menus with interactive elements like buttons, switches, inputs, and sliders.

### Key Features

- **Intuitive Menus:** Easily create menus that players can drag, navigate, and interact with.
- **Multiple Pages:** Organize content into multiple pages, helping players find what they need quickly.
- **Dynamic Updates:** Add, remove, or update menu items on-the-fly without rebuilding the entire menu.
- **Interactive Elements:** Incorporate checkboxes, sliders, text inputs, and more to enhance user engagement.

### Prerequisites

- A running FiveM/RedM server environment
- Basic Lua knowledge
- Familiarity with resource script exports and command registration

---

## Getting Started

### Initialization

Start by initiating the `MenuManager`:

```lua
MenuManager = exports['lr-menu']:initiate()
```

This `MenuManager` instance lets you create and manage menus.

---

## Creating a Menu

Use `MenuManager:registerMenu()` to create a new menu:

```lua
local testMenu = MenuManager:registerMenu({
    header = 'Test Menu',
    description = 'Created via exports.',
    draggable = true,
    canExit = true,
    items = {
        {
            label = 'Test Button',
            smallButton = true,
            effect = function()
                print('Button clicked!')
            end
        }
    }
})
```

### Menu Properties

- **header:** Main title of the menu.
- **description:** A subtitle or context message.
- **draggable:** Allow users to move the menu around.
- **canExit:** Enable closing the menu by the user.
- **items:** A list of elements (buttons, switches, inputs, etc.).

---

## Adding Items Dynamically

You can add new items after the menu is created:

### Switch Item

```lua
testMenu:addItem({
    id = 'switch1',
    label = 'Enable Feature',
    switch = true,
}, function(data)
    print('Switch state:', data.checked)
end)
```

### Buttons

```lua
testMenu:addItem({
    id = 'button1',
    label = 'Click Me',
    smallButton = true,
}, function()
    print('Button was clicked!')
end)
```

For a normal-sized button:

```lua
testMenu:addItem({
    id = 'button2',
    label = 'Regular Button',
}, function()
    print('Regular button clicked!')
end)
```

### HTML Content

Embed custom HTML:

```lua
testMenu:addItem({
    id = 'html1',
    html = '<p>Custom HTML content</p>'
})
```

### Checkbox

```lua
testMenu:addItem({
    label = 'Enable Option',
    checked = false,
}, function(data)
    print('Checkbox state:', data.checked)
end)
```

### Input Box

```lua
testMenu:addItem({
    label = 'Enter Text',
    placeholder = 'Type here...',
}, function(data)
    print('User input:', data.inputValue)
end)
```

### Range Slider

```lua
testMenu:addItem({
    label = 'Volume',
    min = 0,
    max = 100,
    step = 1,
    value = 50,
}, function(data)
    print('Volume set to:', data.value)
end)
```

### Multi-Option Slider

```lua
testMenu:addItem({
    label = 'Choose an Option',
    items = {
        { label = 'Option A', data = 'A' },
        { label = 'Option B', data = 'B' },
        { label = 'Option C', data = 'C' },
    },
}, function(data)
    print('Selected:', data.value)
end)
```

---

## Managing Items

**Remove an item:**
```lua
testMenu:removeItem('button1')
```

**Update an item:**
```lua
testMenu:updateItem('button1', {
    label = 'Updated Button Text',
})
```

---

## Navigation Between Pages

Add navigation arrows for multi-page menus:

```lua
testMenu:addArrows({
    current = 2,
    total = 3,
}, function(data)
    print('Current page:', data.currentPage)
end)
```

---

## Multi-Page Menus

You can link multiple menus as pages of a larger menu:

```lua
local totalPages = 5

local mainMenu = MenuManager:registerMenu({
    id = 'mainMenu',
    header = 'Main Menu',
    description = 'Your main access point',
    draggable = true,
    canExit = true,
    items = {
        {
            label = 'Go to Page 2',
            effect = function()
                page2:openMenu()
            end
        },
    }
})

local page2 = MenuManager:registerMenu({
    header = 'Page 2',
    pageFor = 'mainMenu',
    description = 'Second Page',
    draggable = true,
    canExit = true,
    arrows = { current = 2, total = totalPages },
    items = {
        {
            id = 'switch1',
            label = 'Toggle Something',
            switch = true,
            effect = function(data)
                print('Switch toggled:', data.checked)
            end
        }
    }
})

mainMenu:addArrows({ current = 1, total = totalPages })
```

**Tip:**  
- Use `pageFor = 'mainMenu'` to link pages.  
- `addArrows()` handles pagination.

---

## Opening & Closing Menus

**Open a menu:**
```lua
testMenu:openMenu()
```

**Close a menu:**
```lua
testMenu:close()
```

**Destroy a menu:**
```lua
testMenu:destroy()
```

**Open via Command:**
```lua
RegisterCommand('openmenulr', function()
    testMenu:openMenu()
end, false)
```

---

## Conclusion

With **lr-menu**, you can quickly create polished, interactive menus. As you build more complex UIs, refer back to this guide for examples and best practices. Enjoy making user experiences more engaging and intuitive!
```
