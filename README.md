# Everything Wrong with Firestore

_nuances, missing features, strange API, things to watchout, and wishlist_

## Types

- Firestore does not work with custom types, it requires marshalling to JSON and unmarshalling to `map[string]any` before writing to Firestore
- Firestore [does not support](https://cloud.google.com/go/docs/reference/cloud.google.com/go/firestore/latest#cloud_google_com_go_firestore_DocumentRef_Create) `uint64`
- Firestore needs manual setting into `map[string]any` `time.Time` values after decoding before write to Firestore so that Firestore recognizes `time.Time` type
- `firestore.Increment(1)` on new field will have `float64` type, not `int64`

## Iterator

- Firestore iterator has no backoff mechanism
- Firestore iterator may return `Unavailable` gRPC status, yet never terminate
- Firestore iterator may never terminate
- Firestore iterator cannot iterate over whole collection, it requires reading docs in batches (index, and order_by does not help)

## Update()

- Firestore Update() silently fails when some fields are not mutated (e.g. JSON path mistake)
- Firestore Update() cannot ovewrite field of array type
- Firestore cannot Update() field multiple times in the same operation (e.g. cannot ArrayRemove and ArrayUnion in the same operation)
- Firestore cannot Update() n-th element in array in field (e.g. `$.receipt.products[0].category` is not possible to udpate)

## JSON Path

- Firestore cannot Set() field by JSON path (e.g. `"product.gtin": ...` will be stored as a field `product.gtin` in root, instead do `"product": map[string]any{"gtin": ...}`)
- Firestore cannot Update() field by JSON path, it has to be through Set() with MergeAll

## Monitoring

- Firestore `option.WithTelemetryDisabled()` does not disable telemetry. [bug](https://github.com/googleapis/google-cloud-go/issues/13095) is reported and not fixed for 5+ months. as of 2026-02-22 still not fixed.
- Firestore sets trace span names with id in them, resulting in polluting traces with garbage and high cardinality and cost. you would get something like `/my_collection/_doc/asdf1234` traces of kind `client`. https://github.com/googleapis/google-cloud-go/issues/12973

