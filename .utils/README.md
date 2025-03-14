# Florr.io translation files

`{0}` will get replaced by whatever argument number that is. Some (like mob types, petal types) accept a separate argument to know what to fetch in that argument, or how to format it. For example, a petal argument 0 can print its rarity (common/unusual/etc) using `{0:rarity}`, or its internal name with `{0:base}` (cactus, rose, magic_stinger etc.)

`{#key}` will fetch another key in the translation files of the same language. It can be nested to accomplish more complex substitutions. You can to forward the arguments you want to that new key, for example: `{#key:0:2}`. will forward the 0th and 2nd arguments in that order (so 0th and 1st when processing that key). If you don't specify which arguments to forward, it'll forward all of them in order. You can also specify multiply keys. For example, in `{#key1#key2}` it'll first look for `key1`, and if it can't be found, it'll fetch `key2` instead. If no keys can be found, it'll be replaced by an empty string (it's not an error by itself). Using `@` will use English as the key lookup language. For example: `{#key@key}` will first lookup `key` in the current language, then in the English translation.

`{!func:arg0:arg1:...}` will call specific built in functions. See below.


An example of how rarities are done in English:
```
Rarities/rare/Name=Rare
[...]

UI/Petal/Rarity={#Rarities/{0:rarity}/Name}
```

In Portuguese, you'd need to take into account the gender:

```
Rarities/rare/Name/Female=Rara
Rarities/rare/Name/Male=Raro
[...]
Petals/rose/Gender=Female
[...]

UI/Petal/Rarity={#Rarities/{0:rarity}/Name/{#Petals/{0:base}/Gender}}
```

## Testing changes (web version)

Run `florrio.utils.uploadCustomLang();` in the JS console. That will prompt you to upload .txt files. Select ALL txt files for the language you wanna test (you do not need to upload the `base.txt` file). The page will refresh with the new language loaded. If you leave the page again, it'll go back to your previously selected language.

## Argument types

### Numbers
- `{0}` Will print raw
- `{0:tooltip}` Will be print in tooltip form (like 2.2k)
- `{0:time}` Will be print as a time. Assumes number is in seconds. It'll transform into something like `X seconds` or `3 hours` for larger numbers.
- `{0:time:type}` Will send `type` as a string in `{1}` for the time formatting keys.
- `{0:tooltipTime}` Will be print as a time. Assumes number is in seconds. It'll transform into something like `X seconds` or `3 hours` for larger numbers. It will also display decimals, such as 0.7 seconds, 5.4 seconds, etc.
- `{0:1orN}` Will print either 1 or N, depending on the value of the number.
- `{0:lastDigit}` Will print the last digit (0-9). If the number is a float, it is first cast to integer.

### String
- `{0}` Will print in escaped form (< will turn into &amp;lt;, > will turn into &amp;gt;, etc.). This is mostly used when printing player names or other user text, to prevent them from using colors
- `{0:raw}` Will print raw

### Mob Type
- `{0:base}` Will print the internal id (rock/cactus/wasp_hel)
- `{0:rarity}` Will print the rarity (common/unusual/rare/etc)

### Mob Base Type
- `{0}` Will print the internal id (rock/cactus/wasp_hel)

### Mob Rarity
- `{0}` Will print the rarity (common/unusual/rare/etc)

### Petal Type
- `{0:base}` Will print the internal id (rock/cactus/magic_stinger)
- `{0:rarity}` Will print the rarity (common/unusual/rare/etc)

### Petal Base Type
- `{0}` Will print the internal id (rock/cactus/magic_stinger)

### Petal Rarity
- `{0}` Will print the rarity (common/unusual/rare/etc)

## Functions

### rand
- `{!rand:A:B}` will generate a random number between A and B (inclusive). Note that the number might be different each time the translation is done, so it's only really meant to be used in places like mob chat lines, so you can pick one of several random options.
