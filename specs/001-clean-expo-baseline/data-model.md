# Navigation Model

This feature has no persisted domain data. Its only model is the user-visible navigation destination set.

## Navigation Destination

| Field | Description | Validation |
|---|---|---|
| Route | File-based destination path | Must correspond to an existing root route file. |
| Label | Text shown in native and web bottom navigation | Must match across both platform tab implementations. |
| Role | Home or placeholder | Exactly one Home destination; Home is first. |
| Content | Screen content purpose | Home has a heading and description; placeholders clearly indicate future functionality. |

## Required Destination Set

| Route | Label | Role |
|---|---|---|
| `/` | Home | Initial destination |
| `/features` | Features | Placeholder |
| `/settings` | Settings | Placeholder |

## Relationships and Transitions

- Every bottom-tab item maps to exactly one root route.
- Selecting a tab displays its matching route and marks that tab selected.
- Launch starts at the Home route.
