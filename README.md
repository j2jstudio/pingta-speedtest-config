# ssShift Speed Test Config

Public JSON configuration for speed test servers used by `ssShift`.

This repository is intended to host a static server list that can be fetched by clients at runtime. The configuration is designed to support:

- Remote config delivery
- Local cache fallback
- Download-only and upload-capable server separation
- Stable server IDs for client-side persistence

## Repository Layout

```text
.
├── README.md
└── speedtest-servers.v1.json
```

## Config Schema

Top-level fields:

- `schemaVersion`: JSON schema version
- `configVersion`: published config version
- `generatedAt`: ISO-8601 generation timestamp
- `ttlSeconds`: client cache lifetime in seconds
- `defaults.defaultServerId`: default selected server
- `defaults.defaultUploadSizeMB`: default upload payload size in MB
- `defaults.fallbackPolicy`: recommended fallback behavior
- `servers`: server definitions

Each server entry includes:

- `id`: stable unique identifier
- `name`: display name
- `provider`: provider or operator
- `regionCode`: region identifier
- `city`: city or routing description
- `countryCode`: country code or `GLOBAL`
- `priority`: lower number means higher priority
- `enabled`: whether clients should display the server
- `supportsDownload`: whether download testing is supported
- `supportsUpload`: whether upload testing is supported
- `download`: download endpoint definition when supported
- `upload`: upload endpoint definition when supported
- `notes`: short maintenance note
- `referenceURL`: official source or reference page

## Validation Rules

- `id` must remain stable after publication
- At least one of `supportsDownload` or `supportsUpload` must be `true`
- `download.url` is required when `supportsDownload` is `true`
- `upload.url` is required when `supportsUpload` is `true`
- Prefer `https` endpoints whenever possible
- Remove or disable endpoints that are no longer publicly reachable

## Publishing Guidance

- Keep the JSON file human-readable and versioned
- Update `configVersion` whenever the server list changes
- Update `generatedAt` on every published change
- Prefer official provider endpoints over community-maintained mirrors
- Avoid adding legacy or unstable endpoints as defaults

## Suggested File Name

Use `speedtest-servers.v1.json` as the published file name in the GitHub repository.

## Recommended Repository Name

Use `ssshift-speedtest-config`.

## Source References Used For The Initial Draft

- Cloudflare: <https://github.com/cloudflare/speedtest>
- Leaseweb: <https://kb.leaseweb.com/kb/network/network-link-speeds/>
- Hetzner: <https://fsn1-speed.hetzner.com/>
- OVHcloud: <https://proof.ovh.net/files/>
