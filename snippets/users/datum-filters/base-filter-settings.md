| Service Name       | A unique ID for the filter, to be referenced by other components. |
| Service Group      | An optional service group name to assign. |
| Source ID          | A case-insensitive [pattern][regex] to match the input source ID(s) to filter. If omitted then datum for _all_ source ID values will be filtered, otherwise only datum with _matching_ source ID values will be filtered. |
| Required Mode      | If configured, an [operational mode][opmodes] that must be active for this filter to be applied. |
| Required Tag       | Only apply the filter on datum with the given tag. A tag may be prefixed with `!` to invert the logic so that the filter only applies to datum **without** the given tag. Multiple tags can be defined using a `,` delimiter, in which case **at least one** of the configured tags must match to apply the filter. |
