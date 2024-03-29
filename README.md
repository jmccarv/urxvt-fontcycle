# OVERVIEW

This rxvt-unicode (urxvt) extension provides the ability to have multiple
fonts configured and easily switch between them at runtime.

A list of fonts for each of the following font styles may be configured:

- plain - normal font
- bold
- italic
- boldItalic

To save typing, one list may be cloned from another with the `clone` directive.

When urxvt starts, font-cycle will look at the current font setting and
search for that font (or list of fonts) in its font list.  If found, font-cycle
will start with that position in the list, otherwise it will start with the
first element in its list. 

# USAGE

Keys may be bound by the normal urxvt mechanism 
(urxvt.keysym.&lt;key>: perl:font-cycle:&lt;cmd>) to the
following font-cycle commands:

- increase\_font

    Switch to the next index in the font list

- decrease\_font

    Switch to the previous index in the font list

- default\_font

    Switch to the configured or detected default font

- font:&lt;num>

    Switch to the font at index &lt;num>

You may configure the `urxvt.font-cycle.accelerator` 
and `urxvt.font-cycle.mouseAccelerator` (see CONFIGURATION)

By configuring the accelerator key you gain the following key bindings automatically:

- &lt;accel>+KP\_Minus

    Switch to previous index in font list (same as font-cycle:decrease\_font)

- &lt;accel>+KP\_Plus

    Switch to next index in font list (same as font-cycle:increase\_font)

- &lt;accel>+KP\_Enter

    Switch to default font (same as font-cycle:default\_font)

- &lt;accel>+KP0-KP9

    Switch to index 0-9 in font list (same as font-cycle:font:<0-9>)

By configuring the mouseAccelerator key you gain the following mouse bindings:

- &lt;accel>+Button4 (scroll wheel up)

    Switch to next index in font list

- &lt;accel>+Button5 (scroll wheel down)

    Switch to previous index in font list

- &lt;accel>+Button2 (middle button)

    Switch to default font

# CONFIGURATION

Configuration is done much the same as other urxvt settings, as X resources.

- urxvt.font-cycle.font.&lt;num>: &lt;list>

    Each &lt;num> (starting with 0 and increasing by 1 for each entry) is a
    comma separated list of fonts to switch to.

- urxvt.font-cycle.boldFont.&lt;num>
- urxvt.font-cycle.itialicFont.&lt;num>
- urxvt.font-cycle.boldItialicFont.&lt;num>

    These are all the same as .font.&lt;num> for bold, italic, and boldItalic
    fonts respectively.

- urxvt.font-cycle.clone: &lt;from>

    Clone the &lt;from> list to the normal font list.  &lt;from> may be one of:

    - bold
    - italic
    - boldItalic

- urxvt.font-cycle.boldClone: &lt;from>
- urxvt.font-cycle.italicClone: &lt;from>
- urxvt.font-cycle.boldItalicColne: &lt;from>

    Clone the &lt;from> list to bold, italic, or boldItalic respectively.  To
    clone the normal font list (font-cycle.font.\*) use 'plain' for the &lt;from> value.

- urxvt.font-cycle.defaultIdx: &lt;num>

    This configures a default font setting for font-cycle.  &lt;num> is an index into
    the font lists.  When the command `default_font` is executed, the
    font(s) will be switched to those at the &lt;num> index in the font lists.

    If not configured, this will default to the detected starting font or 0 if
    no starting font is detected.

- urxvt.font-cycle.accelerator: &lt;key>

    Configure the accelerator key to one of the following keys
    (single character shortcuts in parenthesis)

    - Shift (S)
    - Control (C)
    - ISOLevel3 (I)
    - Meta (M)
    - Mod1 (1)
    - Mod2 (2)
    - Mod3 (3)
    - Mod4 (4)
    - Mod5 (5)

- urxvt.font-cycle.debug:

    If true, debugging output will be sent to STDERR

# ENVIRONMENT

The following environment variables may be used to control font-cycle:

- FONT\_CYCLE\_DEBUG

    If set to something perl interprets as true, debugging will be output 
    to STDERR, otherwise debugging will be disabled.

    The environment variable takes precedence over the X resource.

# EXAMPLE CONFIGURATION

Set the regular font and boldFont settings for urxvt
  urxvt\*font: -misc-fixed-medium-r-semicondensed--13-100-100-100-c-60-iso8859-1
  urxvt\*boldFont: -misc-fixed-medium-r-semicondensed--13-100-100-100-c-60-iso8859-1

With this commented out, font-cycle will use the detected starting font as the
default index (in this case, 2)
  !urxvt\*font-cycle.defaultIdx: 4

Tell font-cycle to wrap around when increase\_font/decrease\_font limits are exceeded.
  urxvt\*font-cycle.wrap: false

Tell font-cycle to create the boldFont list from the regular font list
  urxvt\*font-cycle.boldClone: plain

Set up the regular font list with all fonts to switch between
  urxvt\*font-cycle.font.0: -misc-fixed-medium-r-normal--7-50-100-100-c-50-iso8859-1
  urxvt\*font-cycle.font.1: -misc-fixed-medium-r-normal--10-100-75-75-c-60-iso8859-1
  urxvt\*font-cycle.font.2: -misc-fixed-medium-r-semicondensed--13-100-100-100-c-60-iso8859-1
  urxvt\*font-cycle.font.3: -xos4-terminus-bold-r-normal--14-140-72-72-c-80-iso8859-1
  urxvt\*font-cycle.font.4: -xos4-terminus-bold-r-normal--16-160-72-72-c-80-iso8859-1
  urxvt\*font-cycle.font.5: -xos4-terminus-bold-r-normal--20-200-72-72-c-100-iso8859-1
  urxvt\*font-cycle.font.6: -xos4-terminus-bold-r-normal--24-240-72-72-c-120-iso8859-1
  urxvt\*font-cycle.font.7: -xos4-terminus-bold-r-normal--28-280-72-72-c-140-iso8859-1
  urxvt\*font-cycle.font.8: -xos4-terminus-bold-r-normal--32-320-72-72-c-160-iso8859-1

Configure key bindings
  urxvt\*keysym.Shift-KP\_Subtract: perl:font-cycle:decrease\_font
  urxvt\*keysym.Shift-KP\_Add: perl:font-cycle:increase\_font
  urxvt\*keysym.Shift-KP\_Enter: perl:font-cycle:default\_font
  urxvt\*keysym.Shift-KP\_Multiply: perl:font-cycle:font:4

Configure accelerator
  !urxvt\*font-cycle.accelerator: Control
  urxvt\*font-cycle.accelerator: C

Notes:

- the .font.2 matches the urxvt\*font setting.  font-cycle will see
this and know to start with the .2 element in the font list and to configure
the defaultIdx to be 2 (since it was commented out).
- all clone operations are performed after all font lists have been
read, so in the example above, the bold list that is cloned from
the plain list will have all 9 entries that the plain list is
configured with.

# AUTHOR

- Jason McCarver [slam@parasite.cc](https://metacpan.org/pod/slam%40parasite.cc)
- Campbell Barton [https://bitbucket.org/ideasman42/](https://bitbucket.org/ideasman42/)
