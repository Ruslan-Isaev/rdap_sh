# rdap

A minimal Bash CLI tool for querying [RDAP](https://en.wikipedia.org/wiki/Registration_Data_Access_Protocol) (Registration Data Access Protocol) information for IP addresses and domain names via [rdap.org](https://rdap.org).

## Features

- Automatically detects whether the input is an IP address or domain name
- Fetches structured RDAP data from `rdap.org`
- Outputs pretty-printed JSON via `jq`
- Handles HTTP errors and empty responses gracefully

## Requirements

- `bash`
- `curl`
- `jq`

## Installation

```bash
git clone https://github.com/Ruslan-Isaev/rdap.sh.git
cd rdap.sh
chmod +x rdap
```

Optionally, install system-wide:

```bash
sudo cp rdap /usr/local/bin
```

## Usage

```bash
./rdap <domain or IP>
```

### Examples

**Query a domain:**
```bash
./rdap example.com
```

**Query an IP address:**
```bash
./rdap 8.8.8.8
```

### Sample Output

```json
{
  "objectClassName": "ip network",
  "handle": "8.8.8.0/24",
  "startAddress": "8.8.8.0",
  "endAddress": "8.8.8.255",
  "ipVersion": "v4",
  "name": "GOGL",
  ...
}
```

## How It Works

1. If the argument matches an IPv4 pattern (`x.x.x.x`), the script queries `https://rdap.org/ip/<IP>`.
2. Otherwise, it treats the input as a domain and queries `https://rdap.org/domain/<domain>`.
3. The response is validated (HTTP status + non-empty body) and piped through `jq` for readable output.

## Error Handling

| Situation | Behavior |
|---|---|
| No argument provided | Prints usage and exits with code `1` |
| Non-200 HTTP response | Prints error with status code and exits with code `1` |
| Empty response body | Prints error and exits with code `1` |

