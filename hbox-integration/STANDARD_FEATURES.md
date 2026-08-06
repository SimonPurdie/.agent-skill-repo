# Standard Features Of HBOX Native Applications

A native HBOX application is a local web application whose launch and lifecycle are integrated with HBOX.

Its frontend should be available only on the local machine and accessed through HBOX.

If the application requires functionality that cannot be provided entirely within the browser, its backend should run as activity managed by an HBOX Session.

The application should provide a close button. Activating it should:

1. Gracefully shuts down any backend processes.
2. Make one best-effort attempt to close the current browser tab using `window.close()`.

Backend processes should exit with code 0 after a normal shutdown, allowing HBOX to close the associated Session.

If the backend has been closed but a browser tab remains open (either by failure of window.close() or due to duplicate tabs having been opened before the application was closed), the tab should display a message stating:
  That the application has been shut down.
  That the user can now close the tab.
This behavior should not require any detection or policing of successful window.close() actions.


When the backend has shut down, any application tabs that remain open should display a message explaining:

1. That the application has been shut down.
2. That the tab can now be closed.

For example: "<Application name> has been shut down. You can now close the tab."

This includes duplicate tabs that were opened before the application was shut down.

The application does not need to determine whether its call to window.close() succeeded. Remaining tabs should transition to the shutdown message based on the application or Session having ended, rather than by monitoring the result of the tab-close attempt.