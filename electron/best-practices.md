# Electron / Best Practices

#### Don't use `ipcRenderer.sendSync`

As explained in the [docs](https://electronjs.org/docs/api/ipc-renderer#ipcrenderersendsyncchannel-arg1-arg2-):
> Sending a synchronous message will block the whole renderer process, unless you know what you are doing you should never use it.

For example, the following code completely freezes the app:

```js
// renderer
ipcRenderer.sendSync("event-key");
// main
ipcMain.on("event-key", event => {});
```

#### Subscribe main's process listeners as soon as it's ready

This way the channels are up and initial requests from renderer won't be absorbed.

```js
app.on("ready", () => {
    subscribeListeners();

    // ...
});
```
