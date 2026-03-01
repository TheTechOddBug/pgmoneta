# Migration

## From 0.20.x to 0.21.0

### Vault Encryption

The key derivation for vault file encryption has been upgraded to
`PKCS5_PBKDF2_HMAC` (SHA-256, random 16-byte salt, 600,000 iterations).

This is a **breaking change**. Existing vault files encrypted with the
old method cannot be decrypted by version 0.21.0.

**Action required:**

1. Stop pgmoneta
2. Delete the existing user files:
   - `pgmoneta_users.conf`
   - `pgmoneta_admins.conf`
   - Vault users file (if applicable)
3. Delete the existing master key:
   ```
   rm ~/.pgmoneta/master.key
   ```
4. Regenerate the master key:
   ```
   pgmoneta-admin master-key
   ```
5. Re-add all users:
   ```
   pgmoneta-admin user add -f <users_file>
   ```
6. Restart pgmoneta
