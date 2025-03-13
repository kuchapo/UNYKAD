# Entra ID User Accounts

## Cloud-only Accounts

- Created in Entra ID.
- Without an on-premises account.
- Devices can also be managed.

## On-premises AD Accounts (synchronized)

- If there is already an on-premises infrastructure with Active Directory, Entra ID can store copies of the user identities in the cloud.
- **Sync to Entra ID** with **Microsoft Entra Connect**.
- **Password Hash Storage:** The domain stores the on-premises account or configuration of the connection. So when the user tries to sign in to a cloud application, their information gets passed back to the on-premises AD to be validated (**Passthrough**).
- You can configure the relationship between on-premises and Entra ID with **ADFS** (Hybrid).

## External Accounts (B2B)

- Stores identities of external users that are from outside the directory.
- Users are invited by an **Entra ID tenant** as a guest and their credentials are managed by their own provider.
