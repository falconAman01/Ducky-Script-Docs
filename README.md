# Ducky-Script-Docs
usb rubber ducky program language Docs



# Complete DuckyScript Commands Reference Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Basic Commands](#basic-commands)
3. [Modifier Keys](#modifier-keys)
4. [Special Keys](#special-keys)
5. [Function Keys](#function-keys)
6. [Advanced Commands](#advanced-commands)
7. [Practical Examples](#practical-examples)
8. [Version Differences](#version-differences)

---

## Introduction

DuckyScript is a simple scripting language for automating keyboard input. Originally created for the USB Rubber Ducky, it's now used in various keystroke injection tools. Scripts execute line-by-line, simulating human typing.

**Key Concepts:**
- One command per line
- Commands execute sequentially
- Timing is critical for reliability
- Case-insensitive (mostly)
- No variables or loops (in basic version)

---

## Basic Commands

### STRING
**Syntax:** `STRING [text to type]`

**Detailed Explanation:**
Types out literal text exactly as specified. This command converts your text into individual keystrokes. It handles special characters, spaces, and symbols automatically based on the keyboard layout.

**Important Notes:**
- Respects the target system's keyboard layout (US, UK, etc.)
- Cannot type characters not available on the keyboard
- Automatically handles shift for capitals and symbols
- Maximum line length varies by implementation (usually 256 chars)

**Examples:**
```
STRING Hello World!
STRING user@example.com
STRING P@ssw0rd123
STRING C:\Windows\System32
STRING SELECT * FROM users;
STRING <script>alert('XSS')</script>
```

**Common Issues:**
- Wrong keyboard layout produces wrong characters
- Special characters may not work on non-US layouts
- Some symbols require specific keyboard layouts

---

### DELAY
**Syntax:** `DELAY [milliseconds]`

**Detailed Explanation:**
Pauses script execution for a specified duration in milliseconds (1000ms = 1 second). Essential for allowing the system time to respond to previous commands.

**When to Use:**
- After opening applications (wait for window to appear)
- Before typing passwords (wait for password field)
- After GUI commands (wait for menus to open)
- Between network operations
- When target system is slow

**Timing Guidelines:**
```
DELAY 100-300     // Quick operations (key combos)
DELAY 500-1000    // Standard operations (opening menus)
DELAY 2000-3000   // Opening applications
DELAY 5000-10000  // System-heavy operations (booting, loading)
```

**Examples:**
```
GUI r
DELAY 500         // Wait for Run dialog
STRING notepad
ENTER
DELAY 2000        // Wait for Notepad to open
STRING Hello!
```

**Advanced Usage:**
```
REM Adaptive delays for different systems
DELAY 1000        // Fast systems
DELAY 2000        // Medium systems  
DELAY 3000        // Slow systems
```

---

### DEFAULT_DELAY / DEFAULTDELAY
**Syntax:** `DEFAULT_DELAY [milliseconds]` or `DEFAULTDELAY [milliseconds]`

**Detailed Explanation:**
Sets a default pause between EVERY subsequent command in the script. This eliminates the need to write DELAY after each line. The delay applies to all commands that follow until changed or script ends.

**Best Practices:**
- Place at the beginning of scripts
- Use for consistent timing
- Adjust based on target system speed
- Can be changed mid-script

**Examples:**
```
DEFAULT_DELAY 100
GUI r
STRING cmd
ENTER
STRING ipconfig
ENTER

REM Above is equivalent to:
GUI r
DELAY 100
STRING cmd
DELAY 100
ENTER
DELAY 100
STRING ipconfig
DELAY 100
ENTER
```

**Performance Tuning:**
```
DEFAULT_DELAY 50   // Fast systems, minimal delays
DEFAULT_DELAY 100  // Standard timing
DEFAULT_DELAY 200  // Slower systems or complex operations
DEFAULT_DELAY 500  // Very slow or heavily loaded systems
```

---

### REM
**Syntax:** `REM [comment text]`

**Detailed Explanation:**
Adds remarks/comments to your script. Completely ignored during execution. Essential for documenting your code, explaining logic, and maintaining scripts.

**Best Practices:**
- Document script purpose at the top
- Explain complex operations
- Note target OS or requirements
- Include author and date information
- Mark sections of code

**Examples:**
```
REM ================================================
REM Script: Windows Information Gatherer
REM Author: Security Team
REM Date: 2025-10-24
REM Target: Windows 10/11
REM Description: Collects system information
REM ================================================

REM Open Run dialog
GUI r
DELAY 500

REM Launch PowerShell
STRING powershell
ENTER
DELAY 2000

REM Get system info
STRING Get-ComputerInfo
ENTER
```

---

## Modifier Keys

Modifier keys are held down while pressing another key. They can be combined for complex shortcuts.

### GUI / WINDOWS
**Syntax:** 
- `GUI [key]` - Press Windows/Command key with another key
- `GUI` - Press Windows/Command key alone
- `WINDOWS [key]` - Alternative syntax

**Detailed Explanation:**
Simulates the Windows key (⊞) on Windows/Linux or Command key (⌘) on macOS. The most powerful modifier for system-level shortcuts.

**Windows Shortcuts:**
```
GUI               // Open Start Menu
GUI r             // Run dialog
GUI d             // Show Desktop
GUI e             // File Explorer
GUI l             // Lock Screen
GUI x             // Quick Link Menu (Power User Menu)
GUI i             // Windows Settings
GUI s             // Search
GUI v             // Clipboard History
GUI SHIFT s       // Screenshot tool (Snipping)
GUI CTRL d        // New Virtual Desktop
GUI CTRL LEFT     // Switch Virtual Desktop Left
GUI CTRL RIGHT    // Switch Virtual Desktop Right
GUI TAB           // Task View
GUI PAUSE         // System Properties
GUI PLUS          // Magnifier Zoom In
GUI MINUS         // Magnifier Zoom Out
GUI PRINTSCREEN   // Screenshot to Pictures folder
```

**macOS Shortcuts (Command Key):**
```
GUI c             // Copy
GUI v             // Paste
GUI x             // Cut
GUI z             // Undo
GUI SPACE         // Spotlight Search
GUI q             // Quit Application
GUI w             // Close Window
GUI TAB           // Switch Applications
```

**Linux Shortcuts (Super Key):**
```
GUI               // Activities Overview (GNOME)
GUI l             // Lock Screen
GUI d             // Show Desktop (many DEs)
```

---

### CTRL / CONTROL
**Syntax:** 
- `CTRL [key]`
- `CONTROL [key]`
- Can combine with other modifiers

**Detailed Explanation:**
Holds the Control key while pressing another key. Used for application shortcuts and system commands. Works across all operating systems.

**Universal Shortcuts:**
```
CTRL c            // Copy
CTRL v            // Paste  
CTRL x            // Cut
CTRL z            // Undo
CTRL y            // Redo
CTRL a            // Select All
CTRL s            // Save
CTRL f            // Find
CTRL h            // Find and Replace (or History in browsers)
CTRL p            // Print
CTRL n            // New Window/Document
CTRL o            // Open File
CTRL w            // Close Tab/Window
CTRL t            // New Tab (browsers)
CTRL SHIFT t      // Reopen Closed Tab (browsers)
CTRL TAB          // Next Tab
CTRL SHIFT TAB    // Previous Tab
CTRL PLUS         // Zoom In
CTRL MINUS        // Zoom Out
CTRL 0            // Reset Zoom
```

**Developer Shortcuts:**
```
CTRL SHIFT i      // Open Developer Tools (browsers)
CTRL SHIFT j      // Console (browsers)
CTRL SHIFT c      // Inspect Element (browsers)
CTRL k            // Clear Console (many IDEs)
CTRL /            // Toggle Comment (many editors)
CTRL SPACE        // Autocomplete (many IDEs)
```

**System Commands:**
```
CTRL ALT DELETE   // Security Options (Windows)
CTRL ALT t        // Terminal (Linux)
CTRL SHIFT ESC    // Task Manager (Windows)
CTRL ALT l        // Lock Screen (Linux)
```

**Multi-Modifier Examples:**
```
CTRL SHIFT n      // New Incognito Window (Chrome)
CTRL ALT SHIFT    // Can be used together
CTRL GUI LEFT     // Snap window left (Windows)
```

---

### ALT
**Syntax:** `ALT [key]`

**Detailed Explanation:**
Holds the Alt key while pressing another key. Primarily used for menu navigation, window management, and special characters.

**Window Management:**
```
ALT F4            // Close Window
ALT TAB           // Switch Windows (forward)
ALT SHIFT TAB     // Switch Windows (backward)
ALT ESC           // Cycle Through Windows
ALT SPACE         // Window Menu
ALT F5            // Restore Window
ALT F9            // Minimize Window
ALT F10           // Maximize Window
```

**Browser Shortcuts:**
```
ALT LEFT          // Back
ALT RIGHT         // Forward  
ALT HOME          // Home Page
ALT d             // Address Bar (many browsers)
ALT ENTER         // Properties (File Explorer)
```

**Application Menu Access:**
```
ALT f             // File Menu
ALT e             // Edit Menu
ALT v             // View Menu
ALT t             // Tools Menu
ALT h             // Help Menu
```

**Special Uses:**
```
ALT PRINTSCREEN   // Screenshot Active Window Only
ALT ENTER         // Full Screen (media players, games)
ALT PLUS          // Expand Folder (File Explorer)
ALT MINUS         // Collapse Folder (File Explorer)
```

---

### SHIFT
**Syntax:** `SHIFT [key]`

**Detailed Explanation:**
Holds the Shift key while pressing another key. Used for capitalization, extended selection, and alternative functions.

**Text Selection:**
```
SHIFT LEFT        // Extend selection left one character
SHIFT RIGHT       // Extend selection right one character
SHIFT UP          // Extend selection up one line
SHIFT DOWN        // Extend selection down one line
SHIFT HOME        // Select to beginning of line
SHIFT END         // Select to end of line
SHIFT CTRL LEFT   // Select previous word
SHIFT CTRL RIGHT  // Select next word
SHIFT CTRL HOME   // Select to beginning of document
SHIFT CTRL END    // Select to end of document
```

**Navigation:**
```
SHIFT TAB         // Previous field (reverse tab)
SHIFT SPACE       // Page Up (browsers)
SHIFT DELETE      // Cut (alternative to CTRL X)
SHIFT INSERT      // Paste (alternative to CTRL V)
CTRL SHIFT INSERT // Copy (alternative to CTRL C)
```

**Special Functions:**
```
SHIFT F10         // Context Menu (right-click alternative)
SHIFT CTRL ESC    // Task Manager
SHIFT DELETE      // Permanent Delete (skip Recycle Bin)
SHIFT when inserting CD // Prevent autoplay
```

---

## Special Keys

### ENTER / RETURN
**Syntax:** `ENTER`

**Detailed Explanation:**
Simulates pressing the Enter or Return key. Confirms input, submits forms, creates new lines.

**Common Uses:**
```
STRING username
TAB
STRING password
ENTER             // Submit login form

GUI r
DELAY 500
STRING cmd
CTRL SHIFT ENTER  // Run as administrator
```

---

### SPACE
**Syntax:** `SPACE`

**Detailed Explanation:**
Presses the spacebar. Required when you need spaces outside of STRING commands or for special functions.

**Uses:**
```
GUI               // Open Start Menu
DELAY 300
STRING notepad
DELAY 100
SPACE             // Trigger search
ENTER

ALT SPACE         // Window menu
```

---

### TAB
**Syntax:** `TAB`

**Detailed Explanation:**
Moves focus to the next field or element. Essential for navigating forms, dialogs, and interfaces without a mouse.

**Form Navigation:**
```
STRING username
TAB
STRING password
TAB
TAB               // Skip "Remember Me" checkbox
ENTER             // Click Login button
```

**Application Switching:**
```
ALT TAB           // Open switcher
DELAY 200
TAB               // Move to next window
TAB               // Move to next window
ENTER             // Select window
```

---

### ESCAPE / ESC
**Syntax:** `ESCAPE` or `ESC`

**Detailed Explanation:**
Presses the Escape key. Cancels dialogs, closes menus, exits modes, and interrupts operations.

**Common Uses:**
```
ESC               // Close dialog
ESC               // Exit full screen
ESC               // Cancel operation
ESC ESC           // Ensure menu is closed (double tap)
```

---

### BACKSPACE
**Syntax:** `BACKSPACE`

**Detailed Explanation:**
Deletes the character to the left of the cursor. Useful for correcting typos in scripts.

**Examples:**
```
STRING Hello Worldd
BACKSPACE         // Remove extra 'd'

STRING test
BACKSPACE
BACKSPACE
BACKSPACE
BACKSPACE         // Clear the word
STRING production
```

---

### DELETE / DEL
**Syntax:** `DELETE` or `DEL`

**Detailed Explanation:**
Deletes the character to the right of the cursor, or deletes selected items.

**Uses:**
```
CTRL a            // Select all
DELETE            // Delete everything

HOME              // Go to start
DELETE            // Delete first character
```

---

### HOME
**Syntax:** `HOME`

**Detailed Explanation:**
Moves cursor to the beginning of the current line. In some contexts, goes to the top of a document or page.

**Examples:**
```
END               // Go to end of line
HOME              // Return to beginning
STRING //         // Add comment at start
```

---

### END
**Syntax:** `END`

**Detailed Explanation:**
Moves cursor to the end of the current line. In some contexts, goes to the bottom of a document or page.

**Examples:**
```
HOME              // Start of line
SHIFT END         // Select entire line
DELETE            // Delete the line
```

---

### PAGEUP / PAGEDOWN
**Syntax:** `PAGEUP` or `PAGEDOWN`

**Detailed Explanation:**
Scrolls up or down one page/screen. Amount varies by application.

**Uses:**
```
PAGEDOWN
PAGEDOWN
PAGEDOWN          // Scroll down 3 pages

CTRL HOME         // Top of document
PAGEDOWN          // Down one page
```

---

### Arrow Keys
**Syntax:** 
- `UP` or `UPARROW`
- `DOWN` or `DOWNARROW`
- `LEFT` or `LEFTARROW`
- `RIGHT` or `RIGHTARROW`

**Detailed Explanation:**
Navigate in four directions. Move cursor, select menu items, or navigate interfaces.

**Navigation Examples:**
```
DOWN
DOWN
ENTER             // Select third item in menu

LEFT LEFT LEFT    // Move cursor left 3 characters

CTRL RIGHT        // Next word
CTRL LEFT         // Previous word

SHIFT DOWN        // Select line
SHIFT DOWN        // Extend selection
```

**Command History (Terminal/CMD):**
```
UP                // Previous command
UP                // Command before that
DOWN              // Next command
ENTER             // Execute command
```

---

### CAPSLOCK
**Syntax:** `CAPSLOCK`

**Detailed Explanation:**
Toggles Caps Lock on/off. Each press changes the state.

**Important:**
- Affects subsequent STRING commands
- State persists after script ends
- Can cause unexpected behavior

**Safe Usage:**
```
CAPSLOCK          // Turn on
STRING HELLO
CAPSLOCK          // Turn off
STRING world
```

---

### NUMLOCK
**Syntax:** `NUMLOCK`

**Detailed Explanation:**
Toggles Number Lock on/off. Affects numeric keypad behavior.

**Use Cases:**
- Ensure numeric keypad works before using numbers
- Less commonly needed in scripts

---

### SCROLLLOCK
**Syntax:** `SCROLLLOCK`

**Detailed Explanation:**
Toggles Scroll Lock. Rarely used in modern systems but available for legacy support.

---

### PRINTSCREEN
**Syntax:** `PRINTSCREEN`

**Detailed Explanation:**
Takes a screenshot. Behavior varies by operating system.

**Platform Behavior:**
```
PRINTSCREEN       // Windows: Copy full screen to clipboard
ALT PRINTSCREEN   // Windows: Copy active window to clipboard
GUI PRINTSCREEN   // Windows: Save screenshot to Pictures folder
SHIFT GUI s       // Windows: Open Snipping Tool
```

---

### INSERT
**Syntax:** `INSERT`

**Detailed Explanation:**
Toggles insert/overwrite mode in text editors. Also used in some keyboard shortcuts.

**Uses:**
```
SHIFT INSERT      // Paste (alternative to CTRL V)
CTRL INSERT       // Copy (alternative to CTRL C)
```

---

### MENU / APP
**Syntax:** `MENU` or `APP`

**Detailed Explanation:**
Opens the context menu (equivalent to right-clicking). Useful for accessing options without a mouse.

**Examples:**
```
MENU              // Open context menu
DOWN
DOWN
ENTER             // Select third option

STRING C:\file.txt
MENU              // Right-click on file
```

---

### PAUSE / BREAK
**Syntax:** `PAUSE` or `BREAK`

**Detailed Explanation:**
Presses Pause/Break key. Used in some system shortcuts.

**Windows Uses:**
```
GUI PAUSE         // System Properties
CTRL BREAK        // Stop command execution (CMD)
```

---

## Function Keys

### F1 through F12
**Syntax:** `F1`, `F2`, `F3`, `F4`, `F5`, `F6`, `F7`, `F8`, `F9`, `F10`, `F11`, `F12`

**Detailed Explanation:**
Function keys with specific purposes in different applications and contexts.

**Standard Functions:**
```
F1                // Help (almost universal)
F2                // Rename (File Explorer, many apps)
F3                // Search/Find Next
F4                // Address bar (browsers, Explorer)
F5                // Refresh/Reload
F6                // Move focus (browsers)
F7                // Spelling check (Office apps)
F8                // Boot menu (during startup)
F9                // Refresh fields (Office)
F10               // Activate menu bar
F11               // Full screen (browsers)
F12               // Save As (Office), Developer Tools (browsers)
```

**With Modifiers:**
```
ALT F4            // Close window
CTRL F4           // Close tab/document
SHIFT F10         // Context menu
CTRL F5           // Hard refresh (bypass cache)
CTRL SHIFT F12    // Print (Office)
```

**System Boot:**
```
F2                // BIOS/UEFI (during boot)
F8                // Boot options (during boot)
F9                // Boot device menu (during boot)
F10               // Boot menu (some systems)
F12               // Boot device menu (many systems)
```

---

## Advanced Commands

### REPEAT
**Syntax:** `REPEAT [number]`

**Detailed Explanation:**
Repeats the previous command a specified number of times. Efficient for repetitive actions.

**Examples:**
```
STRING A
REPEAT 10         // Types: AAAAAAAAAA

DOWN
REPEAT 5          // Press down arrow 6 times total (1 + 5 repeats)

TAB
REPEAT 3          // Tab 4 times total

BACKSPACE
REPEAT 9          // Delete 10 characters total
```

**Limitations:**
- Only repeats the immediately previous command
- Cannot repeat sequences
- Some implementations have maximum repeat values

**Practical Uses:**
```
REM Clear a field
CTRL a
DELETE

REM Alternative: backspace 20 times
BACKSPACE
REPEAT 19

REM Navigate down a menu
DOWN
REPEAT 4
ENTER
```

---

### HOLD (DuckyScript 3.0)
**Syntax:** `HOLD [key]`

**Detailed Explanation:**
Holds a key down without releasing it. Must be manually released with RELEASE command. Useful for complex key combinations or sustained key presses.

**Examples:**
```
HOLD GUI
HOLD SHIFT
STRING s
RELEASE SHIFT
RELEASE GUI
REM Equivalent to: GUI SHIFT s

HOLD CTRL
STRING a
DELAY 100
STRING c
RELEASE CTRL
REM Holds CTRL while pressing A, waiting, then pressing C
```

**Use Cases:**
- Complex multi-key combinations
- Simulating long key presses
- Games or applications requiring held keys

---

### RELEASE (DuckyScript 3.0)
**Syntax:** `RELEASE [key]`

**Detailed Explanation:**
Releases a key that was previously held with HOLD command.

**Important Notes:**
- Must pair with HOLD
- Release keys in reverse order for nested holds
- Forgetting to release can cause stuck keys

---

### INJECT_MOD (DuckyScript 3.0)
**Syntax:** `INJECT_MOD [keystroke]`

**Detailed Explanation:**
Injects a keystroke while preserving currently held modifier keys. Advanced feature for complex operations.

---

### VAR (DuckyScript 3.0)
**Syntax:** `VAR [name] = [value]`

**Detailed Explanation:**
Creates variables for reusable values in DuckyScript 3.0.

**Examples:**
```
VAR $TARGET_IP = 192.168.1.1
STRING ping $TARGET_IP

VAR $USERNAME = admin
VAR $PASSWORD = P@ssw0rd
STRING $USERNAME
TAB
STRING $PASSWORD
```

---

### IF/ELSE (DuckyScript 3.0)
**Syntax:** 
```
IF condition THEN
    commands
ELSE
    commands
END_IF
```

**Detailed Explanation:**
Conditional logic for advanced scripts. Allows branching based on conditions.

---

### WHILE (DuckyScript 3.0)
**Syntax:**
```
WHILE condition
    commands
END_WHILE
```

**Detailed Explanation:**
Loop structure for repeating operations until a condition is met.

---

### FUNCTION (DuckyScript 3.0)
**Syntax:**
```
FUNCTION name()
    commands
END_FUNCTION
```

**Detailed Explanation:**
Define reusable functions to organize complex scripts.

---

## Practical Examples

### Example 1: Open Notepad and Type Message (Windows)
```
REM Simple Notepad script
DEFAULT_DELAY 100
DELAY 1000

REM Open Run dialog
GUI r
DELAY 500

REM Launch Notepad
STRING notepad
ENTER
DELAY 2000

REM Type message
STRING Hello from DuckyScript!
ENTER
STRING This is a test message.
ENTER
STRING Current date: October 24, 2025
```

---

### Example 2: Open Command Prompt as Administrator (Windows)
```
REM Open elevated CMD
DELAY 1000
GUI r
DELAY 500
STRING cmd
DELAY 200
CTRL SHIFT ENTER
DELAY 1000
ALT y
DELAY 1000
STRING echo Administrator privileges granted
ENTER
```

---

### Example 3: Quick System Information Gathering (Windows)
```
REM Gather system information
DEFAULT_DELAY 200
DELAY 1000

GUI r
DELAY 500
STRING cmd
ENTER
DELAY 1500

STRING systeminfo > C:\temp\sysinfo.txt
ENTER
DELAY 3000

STRING ipconfig /all >> C:\temp\sysinfo.txt
ENTER
DELAY 1000

STRING netstat -an >> C:\temp\sysinfo.txt
ENTER
DELAY 1000

STRING tasklist >> C:\temp\sysinfo.txt
ENTER
DELAY 1000

STRING notepad C:\temp\sysinfo.txt
ENTER
```

---

### Example 4: Create a Text File with Content (Windows)
```
REM Create text file with content
DELAY 1000
GUI r
DELAY 500
STRING cmd
ENTER
DELAY 1500

STRING echo This is line 1 > C:\myfile.txt
ENTER
DELAY 300

STRING echo This is line 2 >> C:\myfile.txt
ENTER
DELAY 300

STRING echo This is line 3 >> C:\myfile.txt
ENTER
DELAY 300

STRING notepad C:\myfile.txt
ENTER
```

---

### Example 5: Open Browser and Navigate (Windows)
```
REM Open browser and navigate
DEFAULT_DELAY 200
DELAY 1000

GUI r
DELAY 500
STRING chrome.exe
ENTER
DELAY 3000

REM Go to address bar
CTRL l
DELAY 300

REM Navigate to website
STRING example.com
ENTER
```

---

### Example 6: Lock Computer (Multi-platform)
```
REM Windows
GUI l

REM macOS
GUI CTRL q

REM Linux (GNOME)
CTRL ALT l
```

---

### Example 7: Take Screenshot and Save (Windows)
```
REM Screenshot and save
DELAY 1000

REM Take screenshot
GUI PRINTSCREEN
DELAY 2000

REM Open File Explorer to Pictures
GUI e
DELAY 1500
CTRL l
DELAY 300
STRING %userprofile%\Pictures\Screenshots
ENTER
```

---

### Example 8: PowerShell One-Liner Execution (Windows)
```
REM Execute PowerShell command
DELAY 1000
GUI r
DELAY 500
STRING powershell -WindowStyle Hidden -Command "Get-Process | Export-Csv C:\processes.csv"
ENTER
```

---

### Example 9: Network Configuration Check (Windows)
```
REM Check network configuration
DEFAULT_DELAY 200
DELAY 1000

GUI r
DELAY 500
STRING cmd
ENTER
DELAY 1500

STRING ipconfig /all
ENTER
DELAY 1000
STRING ping 8.8.8.8
ENTER
DELAY 3000
STRING tracert google.com
ENTER
```

---

### Example 10: Disable/Enable Network Adapter (Windows - Admin Required)
```
REM Disable network adapter
DELAY 1000
GUI r
DELAY 500
STRING cmd
CTRL SHIFT ENTER
DELAY 1000
ALT y
DELAY 1000

STRING netsh interface set interface "Ethernet" disabled
ENTER
DELAY 2000

STRING netsh interface set interface "Ethernet" enabled
ENTER
```

---

## Version Differences

### DuckyScript 1.0
- Basic commands only
- No variables or functions
- No conditional logic
- Simple linear execution
- Supported: STRING, DELAY, ENTER, etc.

### DuckyScript 2.0
- Added REPEAT command
- Improved timing controls
- Better special character handling
- Still no variables or logic

### DuckyScript 3.0
- Variables (VAR)
- Functions (FUNCTION)
- Conditional logic (IF/ELSE)
- Loops (WHILE)
- HOLD and RELEASE commands
- More advanced control flow
- Button and LED support
- Extension support

---

## Keyboard Layouts

DuckyScript respects the target system's keyboard layout. Different layouts can cause issues:

**Common Layouts:**
- US QWERTY (default)
- UK QWERTY
- AZERTY (French)
- QWERTZ (German)
- Dvorak
- Colemak

**Layout Issues:**
```
REM US layout types: @
REM UK layout types: "

REM Solution: Test on target layout
REM Or use alternative key combinations
```

---

## Timing Best Practices

### Initial Delay
Always start with a delay to allow USB device recognition:
```
DELAY 1000
REM or
DELAY 2000
```

### Application Launch Delays
Wait for applications to fully load:
```
GUI r
DELAY 500          // Small app (Run dialog)

STRING chrome
ENTER
DELAY 3000         // Large app (browser)
```

### Network Operation Delays
Account for network latency:
```
STRING ping example.com
ENTER
DELAY 5000         // Wait for ping to complete
```

### Progressive Delay Strategy
```
REM Test with long delays first
DELAY 3000

REM Then optimize
DELAY 1500

REM Final optimized version
DELAY 800
```

---

## Security and Ethical Considerations

**Legal Use Only:**
- Only use on systems you own or have explicit permission to test
- Unauthorized access is illegal in most jurisdictions
- Penetration testing requires written authorization

**Defensive Measures:**
- Disable USB ports when not needed
- Use USB device allow lists
- Monitor for unusual USB activity
- Implement USB device control policies
- User awareness training

**Red Team Usage:**
- Document all activities
- Obtain proper authorization
- Follow rules of engagement
- Report findings properly

---

## Troubleshooting

### Commands Not Executing
- Increase DELAY values
- Check keyboard layout
- Verify USB device is recognized
- Check for typos in commands

### Wrong Characters Typed
- Keyboard layout mismatch
- Use correct layout for target system
- Test special characters

### Missed Keystrokes
- System too slow
- Increase DEFAULT_DELAY
- Add delays before critical commands

### Script Stops Unexpectedly
- Check for syntax errors
- Verify all commands are valid
- Test in smaller sections

---

## Additional Resources

**Testing Tools:**
- Notepad (simple text output)
- Online DuckyScript encoders
- Virtual machines for safe testing

**Development Tips:**
- Build scripts incrementally
- Test each section before adding more
- Comment extensively
- Keep backups of working versions
- Version control for complex scripts

**Community Resources:**
- Hak5 forums
- GitHub repositories
- Reddit communities
- Security conference presentations

---

## Appendix: Complete Command Reference

**Basic:**
STRING, DELAY, DEFAULT_DELAY, REM

**Modifiers:**
GUI, WINDOWS, CTRL, CONTROL, ALT, SHIFT

**Special Keys:**
ENTER, SPACE, TAB, ESCAPE, ESC, BACKSPACE, DELETE, DEL, HOME, END, PAGEUP, PAGEDOWN, INSERT, MENU, APP, PAUSE, BREAK, PRINTSCREEN

**Arrows:**
UP, UPARROW, DOWN, DOWNARROW, LEFT, LEFTARROW, RIGHT, RIGHTARROW

**Locks:**
CAPSLOCK, NUMLOCK, SCROLLLOCK

**Function Keys:**
F1, F2, F3, F4, F5, F6, F7, F8, F9, F10, F11, F12

**Advanced (3.0):**
REPEAT, HOLD, RELEASE, VAR, IF, ELSE, END_IF, WHILE, END_WHILE, FUNCTION, END_FUNCTION

---

## Quick Reference Card

```
REM Comment
STRING text
DELAY ms
DEFAULT_DELAY ms

GUI key = Windows/CMD key
CTRL key = Control key
ALT key = Alt key  
SHIFT key = Shift key

ENTER = Enter/Return
TAB = Tab
SPACE = Space
ESC = Escape
BACKSPACE = Backspace
DELETE = Delete

UP/DOWN/LEFT/RIGHT = Arrows
HOME/END = Line start/end
PAGEUP/PAGEDOWN = Page scroll

F1-F12 = Function keys
REPEAT n = Repeat last command

Common Combos:
GUI r = Run dialog
CTRL c/v/x = Copy/Paste/Cut
ALT F4 = Close window
CTRL SHIFT ESC = Task Manager
```
