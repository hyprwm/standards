---
weight: 2
title: hyprlang
---

Hyprlang is a neat, small, easy to understand config language used across the hyprland ecosystem.

## Overview

Example small config:
```ini
bakery {
    counter_color = rgba(ee22eeff)          # color by rgba()
    door_color = rgba(122, 176, 91, 0.1)    # color by rgba()
    dimensions = 10 20                      # vec2
    employees = 3                           # int
    average_time_spent = 8.13               # float
    hackers_password = 0xDEADBEEF           # int, as hex
    enable_anime = true                     # bool

    # nested categories
    secrets {
        password = hyprland                 # string
    }
}

# variable
$NUM_ORDERS = 3

cakes {
    number = $NUM_ORDERS                    # use a variable
    colors = red, green, blue               # string
}

# keywords, invoke your own handler with the parameters
add_baker = Jeremy, 26, Warsaw
add_baker = Andrew, 21, Berlin
add_baker = Koichi, 18, Morioh
```

## Basic

hyprlang is a primarily K-V config language. Every statement is essentially a `key = value` statement.

There are two primary types of statements, a value and a keyword.

### Values

Values are what they seem as, variables that can be changed. See `bakery` in the example for all supported types.

### Keywords

Keywords define an action, or a "function", see `add_baker` in the example config.

## Other functionality

hyprlang provides additional functionality to aid your config writing.

### Variables

Variables can be defined with `$NAME = VALUE`. They can later be used with `$NAME`. No separation is required, so `$NAMEbcd` is allowed.

### Escaping errors

You can escape errors like this:
```ini
# hyprlang noerror true
some_missing_value = true
# hyprlang noerror false
```

### Escaping the # character

You can escape the `#` character with `##`.
