## Everything Wrong with Firestore

> nuances, missing features, strange API, things to watchout, and wishlist

- `2025-08-28` Firestore cannot Set() field by JSON path (e.g. `"product.gtin": ...` will be stored as a field `product.gtin` in root, instead do `"product": map[string]any{"gtin": ...}`)
- `2025-08-26` Firestore cannot Update() field by JSON path with array in it (e.g. `$.receipt.products[0].category` is not possible to udpate)
- `2025-08-12` Firestore iterator has no backoff mechanism
- `2025-08-12` Firestore iterator may return `Unavailable` gRPC status, yet never terminate
- `2025-08-12` Firestore iterator may never terminate
- `2025-05-03` Firestore Update() silently fails when some fields are not mutated (e.g. JSON path mistake)
- `2025-05-03` Firestore cannot Update() field by JSON path, it has to be through Set() with MergeAll
- `2025-04-21` Firestore cannot Update() field multiple times in the same operation (e.g. cannot ArrayRemove and ArrayUnion in the same operation)
- `2025-01-15` Firestore does not support uint64 [doc](https://cloud.google.com/go/docs/reference/cloud.google.com/go/firestore/latest)
- `2024-11-08` Firestore struct tags encoding do not work with custom types, it requires marshalling to JSON and unmarshalling to `map[string]any`
