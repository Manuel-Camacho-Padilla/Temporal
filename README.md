```from libqtile import hook
import os
import subprocess 

from libqtile import bar, layout, widget
from libqtile.config import Click, Drag, Group, Key, Match, Screen
from libqtile.lazy import lazy
from libqtile.utils import guess_terminal

# AUTOSTART
@hook.subscribe.startup_once
def autostart():
    home = os.path.expanduser('~/.config/qtile/autostart.sh')
    subprocess.call([home])

mod = "mod4"
terminal = "kitty"
browser = "brave" 

keys = [
    # ------------ Window Configs ------------
    # Switch between windows
    Key([mod], "h", lazy.layout.left(), desc="Move focus to left"),
    Key([mod], "l", lazy.layout.right(), desc="Move focus to right"),
    Key([mod], "j", lazy.layout.down(), desc="Move focus down"),
    Key([mod], "k", lazy.layout.up(), desc="Move focus up"),
    Key([mod], "space", lazy.layout.next(), desc="Move window focus to other window"),

    # will be to screen edge - window would shrink.
    Key([mod, "control"], "h", lazy.layout.grow_left(), desc="Grow window to the left"),
    Key([mod, "control"], "l", lazy.layout.grow_right(), desc="Grow window to the right"),
    Key([mod, "control"], "j", lazy.layout.grow_down(), desc="Grow window down"),
    Key([mod, "control"], "k", lazy.layout.grow_up(), desc="Grow window up"),
    Key([mod], "n", lazy.layout.normalize(), desc="Reset all window sizes"),

    # Moving out of range in Columns layout will create new column.
    #Key([mod, "shift"], "h", lazy.layout.shuffle_left(), desc="Move window to the left"),
    #Key([mod, "shift"], "l", lazy.layout.shuffle_right(), desc="Move window to the right"),
    Key([mod, "shift"], "j", lazy.layout.shuffle_down(), desc="Move window down"),
    Key([mod, "shift"], "k", lazy.layout.shuffle_up(), desc="Move window up"),

    # Change window sizes (MonadTall)
    Key([mod, "shift"], "l", lazy.layout.grow()),
    Key([mod, "shift"], "h", lazy.layout.shrink()),
    # Toggle floating
    Key([mod, "shift"], "f", lazy.window.toggle_floating()),

    # Toggle between different layouts as defined below
    Key([mod], "Tab", lazy.next_layout()),
    Key([mod, "shift"], "Tab", lazy.prev_layout()),

    # Kill window
    Key([mod], "w", lazy.window.kill()),

    # Switch focus of monitors
    Key([mod], "period", lazy.next_screen()),
    Key([mod], "comma", lazy.prev_screen()),

    # Restart Qtile
    Key([mod, "control"], "r", lazy.reload_config(), desc="Reload the config"),

    # Spawn a command using a prompt widget
    Key([mod], "r", lazy.spawncmd(), desc="Spawn a command using a prompt widget"),

    # Shutdown Qtile
    Key([mod, "control"], "q", lazy.shutdown()),

    # ------------ App Configs ------------
    # Menu
    Key([mod], "m", lazy.spawn("rofi -show drun")),

    # Window Nav
    Key([mod, "shift"], "m", lazy.spawn("rofi -show")),

    # Browser
    Key([mod], "b", lazy.spawn(browser)),

    # File Explorer
    Key([mod], "e", lazy.spawn("thunar")),

    # Terminal
    Key([mod], "Return", lazy.spawn(terminal)),

    # Redshift
    Key([mod], "r", lazy.spawn("redshift -O 2400")),
    Key([mod, "shift"], "r", lazy.spawn("redshift -x")),

    # Screenshot
    Key([mod], "s", lazy.spawn("scrot")),
    Key([mod, "shift"], "s", lazy.spawn("scrot -s")),

    # ------------ Hardware Configs ------------
    # Media player
    Key([], "XF86AudioPlay", lazy.spawn("playerctl play-pause"), desc="Play media"),
    Key([], "XF86AudioStop", lazy.spawn("playerctl play-pause"), desc="Stop media"),
    Key([], "XF86AudioPrev", lazy.spawn("playerctl previous"), desc="Play previous media"),
    Key([], "XF86AudioNext", lazy.spawn("playerctl next"), desc="Play next media"),

    # Volume
    Key([], "XF86AudioLowerVolume", lazy.spawn("pactl set-sink-volume @DEFAULT_SINK@ -5%" )),
    Key([], "XF86AudioRaiseVolume", lazy.spawn("pactl set-sink-volume @DEFAULT_SINK@ +5%")),
    Key([], "XF86AudioMute", lazy.spawn("pactl set-sink-mute @DEFAULT_SINK@ toggle")),

    # Brightness
    Key([], "XF86MonBrightnessUp", lazy.spawn("brightnessctl set +10%")),
    Key([], "XF86MonBrightnessDown", lazy.spawn("brightnessctl set 10%-")),
]


# WORKSPACES

#groups = [ Group(f"{i+1}", label="♥") for i in range(5)]
groups = [ Group(f"{i+1}", label="") for i in range(5)]
""" groups = [
    Group("1", label=""),
    Group("2", label=""),
    Group("3", label=""),
    Group(
        "3",
        label="",
        matches=[
        Match(wm_class=["zoom"]),
      ],
    ),
    Group("4", label=""),
    Group("5", label="")
]
"""
for i in groups:
    keys.extend([
        # mod1 + letter of group = switch to group
        Key([mod], i.name, lazy.group[i.name].toscreen(),
            desc="Switch to group {}".format(i.name)),

        # mod1 + shift + letter of group = switch to & move focused window to group
        Key([mod, "shift"], i.name, lazy.window.togroup(i.name, switch_group=True),
            desc="Switch to & move focused window to group {}".format(i.name)),
        # Or, use below if you prefer not to switch to that group.
        # # mod1 + shift + letter of group = move focused window to group
        # Key([mod, "shift"], i.name, lazy.window.togroup(i.name),
        #     desc="move focused window to group {}".format(i.name)),
    ])



# LAYOUTS
catppuccin = {
    "flamingo": "#F2CDCD",
    "mauve": "#DDB6F2",
    "pink": "#F5C2E7",
    "maroon": "#E8A2AF",
    "red": "#F28FAD",
    "peach": "#F8BD96",
    "yellow": "#FAE3B0",
    "green": "#ABE9B3",
    "teal": "#B5E8E0",
    "blue": "92CDFB",
    "sky": "#89DCEB",
    "white": "#D9E0EE",
    "gray0": "#6E6C7E",
    "black1": "#1A1826",
}

rose_pine = {
    "bg": "#1f1d2e",
    "fg": "#e0def4",
    "fg_gutter": "#555169",
    "black": "#191724",
    "red": "#eb6f92",
    "green": "#31748f",
    "yellow": "#f6c177",
    "blue": "#9ccfd8",
    "magenta": "#c4a7e7",
    "cyan": "#555169",
    "white": "#e0def4",
    "orange": "#6e6a86",
    "pink": "#2a2837"
}
colors = rose_pine

default_layout_config = {
    "border_focus": colors["green"],
    "border_normal": colors["red"],
    "border_width": 2,
    "margin": 3,
}

layouts = [
    layout.Columns(**default_layout_config),
    layout.Max(),
    # Try more layouts by unleashing below layouts.
    # layout.Stack(num_stacks=2),
    # layout.Bsp(),
    # layout.Matrix(),
    layout.MonadTall(**default_layout_config),
    # layout.MonadWide(),
    layout.RatioTile(**default_layout_config),
    # layout.Tile(),
    # layout.TreeTab(),
    # layout.VerticalTile(),
    # layout.Zoomy(),
]

floating_layout = layout.Floating(
    float_rules=[
        # Run the utility of `xprop` to see the wm class and name of an X client.
        *layout.Floating.default_float_rules,
        Match(wm_class="confirmreset"),  # gitk
        Match(wm_class="makebranch"),  # gitk
        Match(wm_class="maketag"),  # gitk
        Match(wm_class="ssh-askpass"),  # ssh-askpass
        Match(title="branchdialog"),  # gitk
        Match(title="pinentry"),  # GPG key password entry
    ]
)


# Drag floating layouts.
mouse = [
    Drag([mod], "Button1", lazy.window.set_position_floating(), start=lazy.window.get_position()),
    Drag([mod], "Button3", lazy.window.set_size_floating(), start=lazy.window.get_size()),
    Click([mod], "Button2", lazy.window.bring_to_front()),
]


# PANEL

widget_defaults = dict(
    font = "JetBrainsMono Nerd Font",
    fontsize = 16,
    padding = 3,
    foreground = "#000000",
)
extension_defaults = widget_defaults.copy()


def get_widgets(primary=False):
    widgets = [
         widget.TextBox(
            text = "",
            padding = 0,
            fontsize = 30,
            foreground = colors["magenta"],
            background = "#00000000",
            mouse_callbacks = {"Button1": lazy.spawn("rofi -show drun")},
        ),
        widget.TextBox(
            text = "   ",
            fontsize = 20,
            padding = 0,
            mouse_callbacks = {"Button1": lazy.spawn("rofi -show drun")},
            background = colors["magenta"],
            foreground = colors["black"]
        ),
        widget.TextBox(
            text = "",
            padding = 0,
            fontsize = 30,
            foreground = colors["black"],
            background = colors["magenta"],
        ),
        widget.CurrentLayout(
            foreground = colors["white"],
            background = colors["black"],
        ),

        widget.GroupBox(
            #font = font["clear"]["family"],
            #padding = font["clear"]["padding"],
            #fontsize = font["clear"]["fontsize"],
            foreground = colors["cyan"],
            highlight_method = "text",
            block_highlight_text_color = colors["white"],
            active = colors["fg"],
            inactive = colors["cyan"],
            rounded = False,
            highlight_color = [colors["fg"], colors["yellow"]],
            urgent_alert_method = "line",
            urgent_text = colors["red"],
            urgent_border = colors["red"],
            disable_drag = True,
            use_mouse_wheel = False,
            hide_unused = False,
            spacing = 5,
            this_current_screen_border = colors["yellow"],
            background = colors["black"]
        ),
        widget.TextBox(
            text = "",
            padding = 0,
            fontsize = 30,
            foreground = colors["fg_gutter"],
            background = colors["black"],
        ),
        widget.WindowName(
            #font = windowname,
            fontsize = 16,
            padding = 3,
            format = '{state}{name}',
            foreground = colors["white"],
            background = colors["fg_gutter"],
            center_aligned = True
        ),
        widget.Wallpaper(
            directory = '/home/manu320/.config/wallpapers/',
            random_selection = True,
            fmt='  ',
            foreground = colors["white"],
            background = colors["fg_gutter"],
        ),
        widget.PulseVolume(
            fmt = "墳 {} ",
            mute_command = "amixer -D pulse set Master toggle",
            foreground = colors["white"],
            background = colors["fg_gutter"],
        ),
        widget.TextBox(
            text = "",
            padding = 0,
            fontsize = 30,
            foreground = colors["yellow"],
            background = colors["fg_gutter"],
        ),
        widget.CPU(
            format=" {load_percent:04}% ",
            mouse_callbacks={"Button1": lazy.spawn("kitty -e htop")},
            background = colors["yellow"],
            foreground = colors["bg"]
        ),
        widget.Memory(
            format = '{MemUsed: .0f}{mm}/{MemTotal:.0f}{mm} ',
            padding = 0,
            fontsize = 16,
            measure_mem = 'G',
            measure_swap = 'G',
            foreground = colors["black"],
            background = colors["yellow"],
        ),
        widget.TextBox(
            text = "",
            padding = 0,
            fontsize = 30,
            foreground = colors["magenta"],
            background = colors["yellow"],
        ),
        widget.Battery(
            format = "{char} {percent:2.0%} ",
            charge_char = "",
            discharge_char = "",
            full_char = "",
            unknown_char = "",
            empty_char = "",
            show_short_text = False,
            background = colors["magenta"],
        ),
        widget.ThermalZone(
            format=' {temp}°C',
            format_crit = '{temp}°C',
            crit = 60,
            high = 40,
            fgcolor_crit = 'ff0000',
            fgcolor_high = 'ffaa00',
            fgcolor_normal= '000000',
            background = colors["magenta"]
        ),
        widget.NvidiaSensors(
            format = '| {temp}°C ',
            threshold=  68,
            foreground_alert = 'ff6000',
            foreground = "000000",
            background = colors["magenta"]
        ),
        widget.TextBox(
            text = "",
            padding = 0,
            fontsize = 30,
            foreground = colors["pink"],
            background = colors["magenta"],
        ),
         widget.Net(
            fmt="{}",
            format="\u2193 {down} \u2191 {up} ",
            prefix='k',
            update_interval = 3,
            foreground = colors["white"],
            background = colors["pink"]
        ),
        widget.TextBox(
            text = "",
            padding = 0,
            fontsize = 30,
            foreground = colors["fg_gutter"],
            background = colors["pink"],
        ),
        widget.Clock(
            format="%d/%m/%y - %I:%M %p", 
            foreground = colors["white"],
            background = colors["fg_gutter"]
        ),
        widget.TextBox(
            text="",
            padding=0,
            fontsize=30,
            foreground=colors["fg_gutter"],
            background="00000000"
        ),
        widget.Spacer(length=3, background="#00000000"),
    ]
    if primary:
        widgets.insert(7, widget.Systray())
    return widgets


screens = [
    Screen(
        top = bar.Bar(
            get_widgets(primary = True),
            30,
            background = colors["fg_gutter"],
            margin=[0, 0, 0, 0],
        ),
    ),
    Screen(
        top = bar.Bar(
            # Use everything except the systray, which would crash
            get_widgets(primary = False),
            30,
            background="#00000000",
            margin=[0, 0, 0, 0],
        )
    )
]

dgroups_key_binder = None
dgroups_app_rules = []  # type: list
follow_mouse_focus = True
bring_front_click = False
cursor_warp = False
auto_fullscreen = True
focus_on_window_activation = "smart"
reconfigure_screens = True
auto_minimize = True
wl_input_rules = None
wmname = "qtile"
#wmname = "LG3D"

```
