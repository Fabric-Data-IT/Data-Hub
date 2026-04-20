# Nexus Metadata

Nexus Metadata is a centralized product that consolidates enriched metadata from multiple sources into a unified, structured dataset. It provides normalized and cross-referenced content information — including release dates, regional availability, and additional metadata layers — designed to support analytical and operational workflows across the entertainment industry.

Data is delivered in **JSONL format** via S3 buckets, with each dataset stored under a dedicated subfolder within the `Sources` directory.

---

## Details of the S3 Buckets

Each client receives a dedicated S3 bucket containing the Nexus data. The general structure is:

```
s3://bucket-client/Sources/Nexus/
├── ExternalIds/
│   └── latest/
│       └── external_ids.jsonl
├── ReleaseDate/
│   └── latest/
│       └── release_date.jsonl
└── [future datasets]/
    └── latest/
        └── ...
```

The `latest` subfolder contains the most recent snapshot of the complete dataset.

### Example

To access the demo data, use the following AWS CLI command:

```bash
aws s3 ls s3://bb-media-data/nexus/ --endpoint https://nyc3.digitaloceanspaces.com
```

### Update frequency and scope

The update of data in the S3 Bucket is **weekly**. The scope is defined according to the needs of each customer.

---

## File Description

### ExternalIds (BBIDs)

Maps universal BB identifiers to external provider IDs. This dataset enables cross-referencing content across multiple platforms and databases using standardized identifiers.

**File:** `external_ids.jsonl`
**Location:** `Sources/Nexus/ExternalIds/latest/`

#### ExternalIds Schema

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| UID | string | The ID created by BB which identifies a group of external IDs. | `"bb-12345"` |
| ExternalIds | array | List of external provider identifiers associated with this content. | See below |

#### ExternalIds (nested)

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| UID | string | The ID created by BB which identifies a group of external IDs. | `"bb-12345"` |
| Provider | string | Identifies the source of the ID. Enum: `imdb`, `tmdb`, `tvdb`, `eidr`, `douban`, `filmweb`. | `"imdb"` |
| ID | string | The ID from the external provider. | `"tt1234567"` |
| Type | string | The type assigned by the external provider. Enum: `Movie`, `Tv Show`, `Episode`. | `"Movie"` |
| CreatedAt | string (date-time) | The time when the ID was inserted in the database. | `"2017-07-21T17:32:28Z"` |
| UpdatedAt | string (date-time) | The time when the ID was last updated in the database. | `"2017-07-21T17:32:28Z"` |

---

### ReleaseDate

Contains the original release date and country-specific release dates for audiovisual content. This dataset enables tracking of release windows across different markets.

**File:** `release_date.jsonl`
**Location:** `Sources/Nexus/ReleaseDate/latest/`

#### ReleaseDate Schema

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| UID | string | The unique identifier for the content. | `"asdadad"` |
| OriginalReleaseDate | string (date) | The original release date of the content in `YYYY-MM-DD` format. | `"2024-05-15"` |
| ReleaseDates | array | List of release dates by country. | See below |

#### ReleaseDates (nested)

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| Country | string (ISO 3166-1 alpha-2) | The country code in ISO 3166-1 alpha-2 format. | `"US"` |
| Date | string (date) | The release date in the specified country in `YYYY-MM-DD` format. | `"2024-06-01"` |
| Type | string | The type of release event as reported by the source (e.g., theatrical, streaming, TV premiere). | `"Show_Release"` |

---

## Available Datasets

| Dataset | Description | Status |
|---------|-------------|--------|
| External Ids | Universal BB identifiers mapped to external provider IDs (IMDb, TMDb, TVDb, EIDR, Douban, Filmweb). | Available |
| Release Date | Original and country-specific release dates for audiovisual content. | Available |

> Additional datasets will be incorporated into Nexus Metadata progressively. This page will be updated as new schemas are released.
