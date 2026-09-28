# Bottom Navigation Contract

## Purpose

Define the consistent user-visible navigation surface for native and web implementations.

## Destinations

| Order | Route | Label | Screen expectation |
|---|---|---|---|
| 1 | `/` | Home | Displays a heading and short description. |
| 2 | `/features` | Features | Displays a concise future-feature placeholder. |
| 3 | `/settings` | Settings | Displays a concise future-feature placeholder. |

## Behavioral Rules

- Home is selected when the application initially opens.
- Each visible tab must navigate to an existing matching route.
- Native and web tabs must present the same routes, labels, and order.
- Placeholder routes have no business behavior, persistence, or external integration.

## Compatibility Rules

- The contract is implemented through the existing file-based Router setup.
- Changing a route requires updating its counterpart tab trigger in both native and web navigation before removing the prior route.
