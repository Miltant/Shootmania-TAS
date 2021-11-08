Shootmania-TAS uses .inputs files to store the inputs to load on every physics tick.

# Example
    0 press restart                 # we ensure that the run starts at 0.00
    -120 press alt                  # starts an alt glitch
    -43 press w aim yaw -1.24       # ends the alt glitch and starts aiming at the right spot to exit spawn
    0 press jump                    # jumps on the spawn, doesn't release w
    5 press w                       # waits 5 frames to avoid dash jumping
    10 press nomouse                # releases jump (10 frame = full jump), w is still pressed
    20 press nokbd                  # releases w, setting up for a kz jump
    etc.

# Syntax
An input file consists of (crlf-separated, case sensitive) lines that will be executed sequentially, in order. They are of the form:

    124 press s respawn fire aim 0.856 1.114 fl # respawn bug and setup for the namsei jump

Where all of these elements are **optional** (except the **frame number**):
- `124` is the ***frame** (physics tick) **number***, the first frame of the run being 0, and every frame lasting 0.01 second. This **frame number** asks the TAS engine to wait the `124`th frame of the run to perform the rest of the instructions on this line. Note that the frame number resets if the `restart` key is pressed, and that negative numbers can be used (expected use: alt glitches).
Commas and dots are *ignored*, so you can write eg. `24,256.43` or `24.256,43` if it helps you to read "24 thousand 256 seconds and 43 hundredths".
- `press` is a keyword starting a keyboard block, in which:
    * `s` is a directional key to be pressed. 
    Available directional keys are:
        - nothing, which will keep the previous frame's keys pressed.
        - `nokbd` (asking to *realease* wasd);
        - `alt` (pressing anything else at the same time is not allowed as it has no effect on the physics engine);
        - `w` (up), `a` (left), `s` (down) and `d` (left);
        - `wa`  (up left), `wd` (up right), `sa` (down left) and `sd` (down right), or the aliases `wa`, `wd`, `sa` and `sd` (pressing contradictory keys at the same time would result in only one of each contradicting pair being picked by the game, and thus is not allowed by the presently described language);
    * `respawn` is a *miscealnous key* to be pressed only on this specific frame.
    Available **misc keys** would be:
        - `restart` to restart the race ("delete");
        - `respawn` to respawn to the latest checkpoint ("backspace");
        - `recpspawn` to respawn in the nearest checkpoint-spawn (legacy spec-unspec move).
    * `fire` means the player has to shoot, keep shooting, or seldom take a grapple ("left mouse button").
    Other available **mouse buttons** are:
        - `nomouse` to release `fire` and `action`;
        - `action` to jump, hold stamina, teleport, take grapples, etc. ("space"/"right mouse button");
        - `smash` to *hold down* or *keep holding* both `action` and `fire` at the same time.
- `aim` is a keyword starting expectation for two elements:
    - The **pitch**: a floating-point number (eg. `0.856`) which specifies the angle in radians that the crosshair should lack to be horizontal. So `0` is horizontal, and `1.57079632` and `-1.57079632` (=+-π/2) are vertical.
    - The **yaw**: a floating-point number (eg. `1.114`) which specifies the angle in radians that the crosshair lack to be pointing to the North. So `0` is pointing straight to the North, `1.570796` and `-1.570796` (=+-π/2) are pointing to the East and to the West, and `3.141592` (=π) is pointing to the South.

    You can either write:
    - Both these numbers next to each other eg. `aim 0.856 1.114`;
    - Only one of these, leaving the other as it was for the previous frame, eg. `aim pitch 0.856` or `aim yaw 1.114`.

- `fl` starts holding freelook, and `nofl` releases it.
- `#` introduces a comment. A line that doesn't start with a frame number is automatically considered a comment, regardless of its content.
