
You can generate unique encryption keys to be used to make secure API calls within your workflows.

Encryption keys protect sensitive data and should be updated regularly for continued protection.

Keys are per-workflow, so you can have different keys for different workflows. You can also have multiple keys for a single workflow, but only one key can be active at a time.

## How encryption keys work

When an POST request is made using the [kinde.secureFetch binding](/workflows/bindings/secure-fetch-binding/), the body is automatically encrypted with the active encryption key for the workflow. Use the same encryption key in your own code to decrypt the payload on arrival. This ensures secure transfer of the data.

## Add an encryption key to a workflow

1. Go to **Workflows** and select a workflow.
2. Select **Encryption keys** in the menu.
3. Select **Add encryption key**. A dialog appears, enabling you to copy the key. You need to copy it immediately, as it cannot be viewed again.
4. After you copy the key, select **Close**. If this is the first key you have added, it will automatically be made **active**.
5. Add the key to your code, to decrypt data sent from Kinde.

## Update an encryption key

1. Go to **Workflows** and select a workflow.
2. Select **Encryption keys** in the menu.
3. Select **Add encryption key**. A dialog opens with information about adding the key.
4. Select **Add**. A dialog appears showing the key value.
5. Copy the key value and store it somewhere you can access again.
6. Select **Close**.
7. When you are ready to update the key in your code, select the three dots menu on the new key, then select **Activate**. A dialog opens, confirming the action.
8. Select **Activate encryption key**. This makes the new key active and deactivates the previously active key.
9. Add the new key value to your code, to continue decrypting data sent from Kinde.

## Deactivate/activate an encryption key

You can change the active status of any key. Remember to update your code to use the active key.

1. Go to **Workflows** and select a workflow.
2. Select **Encryption keys** in the menu.
3. To deactivate an active key:
   1. Select the three dots menu on the active key.
   2. Select **Deactivate**. A confirmation window opens.
   3. Follow the prompts and select **Deactivate encryption key.**
4. To activate a deactivated key:
   1. Select the three dots menu on the inactive key. An inactive key shows no status.
   2. Select **Activate**. A confirmation window opens.
   3. Follow the prompts and select **Activate encryption key**. This makes the new key active and deactivates any previously active key.

## Delete used or unwanted encryption keys

Deleting used keys is good security hygiene. But deleting an active key can also break your code. Only delete inactive keys.

1. Go to **Workflows** and select a workflow.
2. Select **Encryption keys** in the menu.
3. Next to an inactive key, select the three dots menu.
4. Select **Delete key**. A confirmation window opens.
5. Follow the prompts and select **Delete encryption key**.
