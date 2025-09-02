
We know it’s really important that you can easily get your data out of Kinde when you need to. It’s equally important that your data - especially password data - is safe and cannot be easily accessed.

**Important note**: For security reasons, only team members who are **Owners** can export data.

You can download most of your Kinde data in a few steps, but if the export includes user passwords, this triggers an owner approval process to ensure that only authorized people can access them. See the fairly long but secure process outlined below.

Big disclaimer of course: Once you have downloaded your data, you are entirely responsible for protecting it.

## Initiate a data export

1. Go to **Settings > Business > Details** to the **Business Information** page in Kinde.
2. In the **Export data** section, select **Export**. Note, this section is only visible if you are an **Owner**.
3. In the window that appears, select one of the export options.
4. If you select **All data (except passwords)**:
   1. Select **Next**. You will be sent an email with a confirmation code.
   2. Enter the code and select **Next**. The download file is generated.
   3. Download the file. Note that this is the only opportunity to download. To get the file again, you need to go back to step 2.
5. If you select **All data**:
   1. Select **Next**. A request is sent to all business owners to approve the export.
   2. Read about the process below.

## The export and approval process (when passwords are included)

There are a number of checks and validations done to enable password export. Unfortunately there is no way to avoid this being long and somewhat notification-heavy, as the aim is to prevent unauthorized access to passwords and other data.

<Aside>

If a business has a sole owner, this might seem like a more complex process than it needs to be. But we implemented this process to maximize security. At some stage in future we may optimize it for the solo owner experience, but for now, this applies to all businesses.

</Aside>

Here’s how it works:

1. An owner initiates a data export (including passwords) in Kinde. See above.
2. All Kinde owners are notified by email of the export request (including any additional owners). They then have 24 hours to review it and ensure that it is a legitimate request. Note that this email is still sent if there is only one owner.
3. If the request seems suspect, any owner can reject it immediately:
   1. Select **Review** in the email to open the request in Kinde.
   2. Select **Reject** in the far column of the data export table. This ends the export process.
4. If the request is fine to approve, the notified owners have to wait 24 hours from the initial request for a new email which will allow them to approve the request in Kinde.
5. The request can be approved by whichever notified owner responds first:
   1. Select **View** in the email.
   2. Select **Approve** in the right column of the data export table.
   3. Enter a one-time code to verify your identity and complete the approval.
6. Approval triggers an email to the original requestor, who receives instructions on how to download the data. To download:
   1. In the email, select **Download**. This opens Kinde.
   2. Select the **Download** button in the export area. A confirmation window opens.
   3. Enter the one-time verification code sent to your email and select **Next**. The data starts to be prepared.
   4. After the data is generated, a window appears showing an encrypted .dat file for download, as well as a **Key** and an **Initialization Vector** for decrypting the file.
   5. Copy the **Key** and the **Initialization Vector** somewhere you can access it again later.
   6. Download the file. Note that this is the only opportunity to download. To get the file again, you need to request the data export again.
   7. An email is sent to the owner, advising them you have downloaded the data.
   8. Next, decrypt the downloaded file (see below).

We know this is a long process, especially since you are likely both the requestor and the owner, but we have made password security a top priority.

## Decrypt your data export file

To decrypt the .dat file, you need to run a decryption command. You can use a tool like OpenSSL or a native command prompt.

1. Open a command prompt window.
2. Paste the following command. `openssl aes-256-ctr -d -e -in /path/to/kinde_export.dat -out /path/to/kinde_export.zip -nosalt -p -K YOUR_ENCRYPTION_KEY -iv YOUR_ENCRYPTION_IV`
3. Replace `YOUR_ENCRYPTION_KEY` with the **Key** you copied above.
4. Replace `YOUR_ENCRYPTION_IV` with the **Initialization Vector** you copied above.
5. Replace the **-in** path with the .dat file location (e.g. `-in ~/Downloads/kinde_export.dat` ), and update the **-out** path to where you would like the decrypted zip file to be generated (e.g. `-out ~/Desktop/kinde_export.zip` )

   Example of how the command might look:

   ```text
   openssl aes-256-ctr -d -e -in /Users/DriveName/Downloads/kinde_export.dat -out /Users/Drivename/Desktop/kinde_export.zip -nosalt -p -K 5f2xxxxxxx6b51ca282745852b0caxxxxxxxxxxxcd5832ecb97500956f3 -iv 4d4axxxxxxxxd2bd1994xxxxc698d3
   ```

6. Press **Enter**. The file decrypted kinde_export.zip file should appear in the specified -out location.

## Cancel a request

At any time, the person who made the export request can cancel it by going to the **Business Information** page in Kinde and selecting **Cancel request**. This ends both the export and approval process.

## What the exported data looks like

Data is exported in NDJSON format, with separate data files for users and organizations.

Here’s an example of `users.ndjson`.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "description": "Unique identifier for the object"
    },
    "email": {
      "type": ["string", "null"],
      "format": "email",
      "description": "Email address of the user"
    },
    "phone": {
      "type": ["string", "null"],
      "description": "Phone number of the user, if available"
    },
    "username": {
      "type": ["string", "null"],
      "description": "Username of the user, if available"
    },
    "last_name": {
      "type": ["string", "null"],
      "description": "Last name of the user, if available"
    },
    "created_on": {
      "type": "string",
      "format": "date-time",
      "description": "Timestamp when the user was created"
    },
    "first_name": {
      "type": ["string", "null"],
      "description": "First name of the user, if available"
    },
    "identities": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "type": {
            "type": "string",
            "description": "Type of identity (e.g., email)"
          },
          "identity": {
            "type": "string",
            "description": "Identity value (e.g., email address)"
          },
          "provider": {
            "type": ["string", "null"],
            "description": "Provider associated with the identity, if any"
          }
        },
        "required": ["type", "identity"]
      },
      "description": "List of identities associated with the user"
    },
    "external_id": {
      "type": ["string", "null"],
      "description": "External identifier for the user, if available"
    },
    "business_code": {
      "type": "string",
      "description": "Code representing the associated business"
    },
    "organizations": {
      "type": "array",
      "items": {
        "type": "string"
      },
      "description": "List of organizations the user belongs to"
    },
    "email_verified": {
      "type": "boolean",
      "description": "Indicates if the email address is verified"
    },
    "password": {
      "type": "object",
      "properties": {
        "hashing_config": { "type": "object" },
        "hashed_password": { "type": "string" },
        "hashing_algorithm": { "type": ["string", "null"] }
      },
      "required": ["hashing_config", "hashed_password"]
    }
  },
  "required": ["id", "email", "created_on", "identities", "business_code", "organizations", "email_verified"],
  "additionalProperties": false
}
```

Here's an example of `organizations.ndjson`.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "description": "Name of the organization"
    },
    "created_on": {
      "type": "string",
      "format": "date-time",
      "description": "Timestamp when the organization was created"
    },
    "business_code": {
      "type": "string",
      "description": "Code representing the associated business"
    },
    "organization_code": {
      "type": "string",
      "description": "Code representing the organization"
    }
  },
  "required": ["name", "created_on", "business_code", "organization_code"],
  "additionalProperties": false
}
```

## How passwords are exported

As above, when passwords are exported, they appear as a hashed password, a hashing algorithm (e.g. bcrypt), and other hashing configuration details (e.g. salt and salt location). With this data you can authenticate against these credentials in another system using the same algorithm. Plain text passwords are never stored by Kinde for security purposes.

## Considerations if switching to another provider

Kinde provides all your data in standard JSON files in a simple format. You will need to refer to the documentation of your new provider to establish what format the data needs to be in for importing.

## Data security

Once your user data is exported and downloaded, it becomes your business’s responsibility to protect.

When we export the file containing passwords, it is encrypted using AES-256-CTR. The file can only be decrypted if you have the unique Key and Initialization Vector (IV) provided during download.

The encryption of the export file is designed to keep passwords secure during export, but once downloaded and decrypted, they become vulnerable.
