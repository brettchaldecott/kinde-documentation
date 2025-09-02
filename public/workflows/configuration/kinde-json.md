
The `kinde.json` file controls how Kinde runs your workflow code. Place this file in your project's root directory so Kinde can discover your configuration during sync.

## Configuration options

The `kinde.json` file lets you:

- Specify the root directory for your custom code
- Define the runtime environment version
- Enable integrations you need

When you sync your project to Kinde, it reads this file to understand your workflow's requirements and set up the appropriate runtime environment.

## Example

```json
{
  "rootDir": "kindeSrc",
  "version": "2024-12-09",
  "integrations": {"npm": {"enabled": true}}
}
```

## API reference

### `rootDir` (string)

Tells Kinde where to look for your custom code.

- **Required:** No
- **Default:** `kindeSrc`

### `version` (string)

The version of the Kinde runtime environment. The date reflects the last breaking change.

- **Required:** Yes

### `integrations` (object)

The integrations you wish to use. Currently only `npm` is supported.

- **Required:** No

#### `npm` (object)

The npm integration allows you to lock down package versions. For this to take effect, the `kinde.json` file must live in the same directory as the `package.json` responsible for defining the versions.

- **Required:** No

Example:

```json
{"integrations": {"npm": {"enabled": true}}}
```
