# Enabling Sending Emails from an Alias in Microsoft 365

By default, Microsoft 365 (formerly Office 365) does **not** allow sending emails from an alias (e.g., `it@abc.com`) such that the recipient sees the alias instead of the primary email (e.g., `xyz@abc.com`). This requires enabling a special setting via Exchange Online PowerShell.

---

## Prerequisites

- Exchange Administrator or Global Administrator permissions.
- Exchange Online PowerShell module installed.

---

## Step-by-Step Guide to Enable Sending from Alias

### 1. Install Exchange Online PowerShell Module (if needed)

Open PowerShell as Administrator and run:

```powershell
Install-Module -Name ExchangeOnlineManagement -Scope CurrentUser
````

If prompted to trust the repository, type **Y** and press **Enter**.

---

### 2. Connect to Exchange Online

Run:

```powershell
Connect-ExchangeOnline
```

Sign in with your admin credentials when prompted.

---

### 3. Enable SendFromAlias Feature

Run the following command:

```powershell
Set-OrganizationConfig -SendFromAliasEnabled $true
```

This enables sending emails from aliases across the organization.

---

### 4. Restart Email Clients

* Wait a few minutes for the change to take effect.
* Restart Outlook or refresh Outlook Web Access (OWA).
* When composing a new message, you can now type or select the alias in the **From** field.

---

## Verify Configuration

Check if the setting is enabled by running:

```powershell
Get-OrganizationConfig | Select-Object SendFromAliasEnabled
```

You should see:

```
SendFromAliasEnabled
---------------------
True
```

---

## Sending Email from an Alias: Options

### Option 1: Enable Sending from Alias (Outlook & OWA)

1. Enable the feature with PowerShell.

```powershell
Set-OrganizationConfig -SendFromAliasEnabled $true
```
   
3. Restart Outlook or OWA session.
4. When composing an email:

   * Click **From**.
   * Select **Other Email Address**.
   * Enter the alias email (e.g., `it@abc.com`).
5. Email will be sent from the alias address.

### Option 2: Use a Shared Mailbox

For team use or better separation:

1. Create a shared mailbox for the alias (e.g., `it@abc.com`).

2. Assign **Send As** permissions to the user:

   ```powershell
   Add-RecipientPermission -Identity it@abc.com -Trustee xyz@abc.com -AccessRights SendAs
   ```

3. User can send email from the shared mailbox via Outlook.

4. Recipients see emails coming from the alias address.

---

## Important Notes

* Adding an alias alone does **not** enable sending from it.
* Without enabling the setting, Microsoft 365 rewrites the **From** address to the primary email.
* Email clients must support the alias sending (**Outlook 365 does; older Outlook versions may not**).

---

## Recommendations

* Use **Option 1** for individual users sending occasionally from an alias.
* Use **Option 2** for shared or team mailboxes requiring full mailbox functionality.

---
