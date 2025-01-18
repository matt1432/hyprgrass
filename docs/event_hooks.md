# Hyprgrass event hooks

Hyprgrass exposes event hooks that allow other plugins to integrate with
hyprgrass. These work more or less the same way as Hyprland hook events. You
register them like this:

```cpp
void onEdgeBegin(void*, SCallbackInfo& cbinfo, std::any args) {
    auto ev = std::any_case<std::pair<std::string, Vector2D>>(args);

    // ignore all other events
    if ev.first != "edge:l:u" {
        return;
    }

    // set this to true when you handle a event
    // edgeUpdate and edgeEnd events won't trigger otherwise
    cbinfo.cancelled = true;

    // do something...
}

APICALL EXPORT PLUGIN_DESCRIPTION_INFO PLUGIN_INIT(HANDLE handle) {
    // note that the hook is dropped when P1 is dropped
    static auto P1 = HyprlandAPI::registerCallbackDynamic(
        PHANDLE,
        "hyprgrass:edgeBegin",
        onEdgeBegin
    );
}
```

Examples in the `examples/` directory may be helpful.

## Event list

Here are the supported events:

> [!NOTE] `EventName` is a `std::string` that is used in hyprgrass-bind, as
> described in the README.
>
> Examples:
>
> - `edge:l:u` - Swipe upwards on the left edge
> - `swipe:4:d` - Swipe down with 4 fingers

| name                 | argument(s)                      | Note                                                                  |
| -------------------- | -------------------------------- | --------------------------------------------------------------------- |
| hyprgrass:edgeBegin  | `std::pair<EventName, Vector2D>` | set `SCallbackInfo.cancelled = true` after you handle this hook event |
| hyprgrass:edgeUpdate | `Vector2D`                       |                                                                       |
| hyprgrass:edgeEnd    | -                                |                                                                       |