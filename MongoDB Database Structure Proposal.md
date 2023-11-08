# MongoDB Database Structure Proposal

This document outlines the proposed MongoDB database structure for the Action Logging System. The database will store logs of module activities and related model actions within individual requests.

## Collections

### `requests`

The `requests` collection will store information about each module request, along with model-specific logs for that request.

- `_id`: ObjectId (unique request identifier)
- `request_uuid`: String (unique request identifier)
- `module`: String (name of the module)
- `timestamp`: ISODate (timestamp of the request)
- `details`: Object
  - `request`: Object
    - `body`: Object (request body)
    - `headers`: Object (request headers)
    - `user_details`: Object (user information, if available)
  - `model_logs`: Array of model-specific logs

### `model_logs`

The `model_logs` collection is optional and can store detailed model-specific logs for reference.

- `_id`: ObjectId (unique model log identifier)
- `request_uuid`: String (corresponding request's unique identifier)
- `model_id`: String (unique model identifier)
- `model_class_name`: String (model's class name)
- `model_namespace`: String (model's namespace or context)
- `model`: String (name of the model)
- `action`: String (type of action, e.g., "Created," "Updated," "Deleted")
- `timestamp`: ISODate (timestamp of the model action)
- `model_details`: Object
  - `old_object`: Object (old object before the update)
  - `new_object`: Object (new object after the update, for "Updated" action)

## Example Usage

### Request Log

```json
{
  "_id": ObjectId("unique_request_id"),
  "request_uuid": "unique_request_id",
  "module": "module_name",
  "timestamp": ISODate("timestamp"),
  "details": {
    "request": {
      "body": { ... }, // Request body
      "headers": { ... }, // Request headers
      "user_details": { ... } // User information (if available)
    },
    "model_logs": [
      {
        "model_id": "unique_model_id",
        "model_class_name": "ModelClassName",
        "model_namespace": "ModelNamespace",
        "model": "model_name",
        "action": "action_type", // "Created," "Updated," "Deleted," etc.
        "timestamp": ISODate("timestamp"),
        "model_details": {
          "old_object": { ... }, // Old object before the update
          "new_object": { ... } // New object after the update (for "Updated" action)
        }
      },
      // Additional model logs for the same request
    ]
  }
}
```
