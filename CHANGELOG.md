# Changelog

Changes in this fork compared to the original [yourivw/LEClient](https://github.com/yourivw/LEClient) 1.3.0.

## 1.4.0

### Certificate chain

- **Full intermediate chain is returned** (`LEOrder::validateCertificateResponse()`).
  Let's Encrypt can deliver more than one intermediate certificate after the leaf
  (for example the long default chain that included the cross-signed ISRG Root X1).
  The original code only returned the first intermediate (`$matches[0][1]`) and silently
  dropped every further certificate, which produced incomplete chains on the web server.
  Now all certificates after the leaf are concatenated (newline separated) into
  `intermediate`; `leaf` is unchanged.
- The certificate response and the parsed certificates are logged at `LOG_STATUS`
  (or via the PSR logger) to make chain problems traceable.

### DNS challenge

- `LEFunctions::checkDNSChallenge()` uses PHP's `dns_get_record()` instead of
  querying `dns.google.com` via curl.

### PHP 8.x compatibility

- `LEOrder::__construct()`: `$notBefore` and `$notAfter` default to `''`
  (optional parameter before required parameter is deprecated since PHP 8.0).
- `LEException::__construct()`: explicit nullable types `?\Throwable` / `?array`
  (implicitly nullable parameters are deprecated since PHP 8.4).
- `LEClient`: property `$baseURL` is declared (dynamic properties are deprecated since PHP 8.2).
- `LEFunctions`: removed `openssl_pkey_free()` calls (no-op and deprecated since PHP 8.0).
- `LEAccount`: missing `contact` / `initialIp` fields in ACME account responses no
  longer raise warnings.

### Package

- Renamed to `nexxtmobile/leclient`; the `LEClient\` namespace is unchanged.
- Requires PHP >= 8.0.
