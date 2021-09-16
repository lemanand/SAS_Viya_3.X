## [How to Check and Set System Locales in Linux](https://www.tecmint.com/set-system-locales-in-linux/)

**1. View System Locale:**

```(bash)
locale
localectl status
```

**2. Display Available Locales:**

```(bash)
locale -a
localectl list-locales
localectl list-locales | grep en_US
```

**3. Set System Locale:**
```(bash)
localectl set-locale LANG=en_US.utf8
```
