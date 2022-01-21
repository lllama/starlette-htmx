# Starlette_htmx

A set of extensions for using [HTMX](https://htmx.org) with [Starlette](http://starlette.io).

Based on [django-htmx](https://github.com/adamchainz/django-htmx) by [Adam Johnson](https://github.com/adamchainz)

## Middleware

Usage:

```python
from starlette.middleware import Middleware
from starlette_htmx.middleware import HtmxMiddleware

app = Starlette(debug=True, ..., middleware=[Middleware(HtmxMiddleware)])
```

The request objects will then have a `request.state.htmx` object which you can
use to test whether the request was made by htmx.


This class provides shortcuts for reading the htmx-specific `request headers <https://htmx.org/reference/#request_headers>`__.

### `__bool__(): bool`

`True` if the request was made with htmx, otherwise `False`.
This is based on the presence of the `HX-Request` header.

This allows you to switch behaviour for requests made with htmx like so:

```python

    def my_view(request):
        if request.htmx:
            template_name = "partial.html"
        else:
            template_name = "complete.html"
        return render(template_name, ...)
```

### `boosted: bool`

`True` if the request came from an element with the `hx-boost` attribute.
Based on the `HX-Boosted` header.

### `current_url: str | None`

The current URL of the browser, or `None` for non-htmx requests.
Based on the `HX-Current-URL` header.

### `history_restore_request: bool`

`True` if the request is for history restoration after a miss in the local history cache.
Based on the `HX-History-Restore-Request` header.

### `prompt: str | None`

The user response to `hx-prompt <https://htmx.org/attributes/hx-prompt/>`__ if it was used, or `None`.

### `target: str | None`

The `id` of the target element if it exists, or `None`.
Based on the `HX-Target` header.

### `trigger: str | None`

The `id` of the triggered element if it exists, or `None`.
Based on the `HX-Trigger` header.

### `trigger_name: str | None`

The `name` of the triggered element if it exists, or `None`.
Based on the `HX-Trigger-Name` header.

### `triggering_event: Any | None`

The deserialized JSON representtation of the event that triggered the request if it exists, or `None`.
This header is set by the `event-header htmx extension
<https://htmx.org/extensions/event-header/>`__, and contains details of the DOM
event that triggered the request.
