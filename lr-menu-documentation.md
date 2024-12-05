# LR-Menu Documentation

Welcome to the **LR-Menu** documentation. This guide provides instructions on how to create, manipulate, and utilize menus using the `lr-menu` resource within FiveM.

## Table of Contents

1. [Introduction](#introduction)
2. [Initiating the Menu Manager](#initiating-the-menu-manager)
3. [Creating and Registering a Menu](#creating-and-registering-a-menu)
4. [Adding Items Dynamically](#adding-items-dynamically)
   - [Switch Item](#switch-item)
   - [Small Button](#small-button)
   - [Normal Button](#normal-button)
   - [HTML Content](#html-content)
   - [Checkbox](#checkbox)
   - [Input Box](#input-box)
   - [Range Box](#range-box)
   - [Slider (Multiple Options)](#slider-multiple-options)
5. [Removing Items](#removing-items)
6. [Navigational Arrows](#navigational-arrows)
7. [Updating Items](#updating-items)
8. [Closing and Destroying Menus](#closing-and-destroying-menus)
9. [Multi-Page Menu Example](#multi-page-menu-example)
10. [Opening the Menu via a Command](#opening-the-menu-via-a-command)

---

## Introduction

`lr-menu` is a UI menu system designed for FiveM (GTA V mod) servers. It allows you to create draggable menus with buttons, checkboxes, sliders, and even multiple pages of content. All menu elements are customizable, and events can be triggered when users interact with them.

---

## Initiating the Menu Manager

Before creating menus, you need to initiate the `MenuManager`:

```lua
MenuManager = exports['lr-menu']:initiate()
