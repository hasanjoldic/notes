# Refer policy

| `rel` value  | Description                                                                                   |
| ------------ | --------------------------------------------------------------------------------------------- |
| `nofollow`   | The current document's original author or publisher does not endorse the referenced document. |
| `noopener`   | Sets `window.opener` to `null`                                                                |
| `noreferrer` | No Referer header will be included. Additionally, has the same effect as noopener.            |

> The default refer policy is `no-referrer-when-downgrade`.

## Best practices

1. Always set `rel="noopener noreferrer"` on anchor tags. Very rarely we'd want access to `window.opener` or `document.referrer`, but probably only for our links.
