# OneDrive JS SDK

[![ts](https://badgen.net/badge/Built%20With/TypeScript/blue)](https://github.com/microsoft/TypeScript)
[![npm version](https://badge.fury.io/js/@harrisoff%2Fonedrive-api.svg)](https://www.npmjs.com/package/@harrisoff/onedrive-api)
![license](https://img.shields.io/npm/l/@harrisoff/onedrive-api)

This project wraps a small part of OneDrive's APIs, only for uploading files and creating sharing links.

This is the core lib of [OneDrive Image Hosting](https://github.com/harrisoff/onedrive).

[中文文档](./README.zh-cn.md)

## Setting up Application

In [App registrations](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) page click *new registration* button, then you'll need to fill the following fields:

- Name

- Supported account types

   `Personal Microsoft accounts only` is enough for personal usage

- Redirect URI

   For example, `https://localhost:3000/`

After registration, click *Authentication* on the left, check `Access tokens (used for implicit flows)` and save.

`Application (client) ID` is generated once the application is registered. But you'll need to verify the application before using it. Just click *Branding* on the left and follow the instructions.

## Usage

### Authentication

Here we use [token flow](https://docs.microsoft.com/en-us/onedrive/developer/rest-api/getting-started/graph-oauth?view=odsp-graph-online#token-flow) in authentication. Use helper function `generateAuthUrl` to generate an auth url and open it.

```ts
import { generateAuthUrl } from '@harrisoff/onedrive-api'

const authUrl = generateAuthUrl('your-client-id', 'your-redirect-uri')
```

> Actually there's another field `scope` is required in the auth url.
> This value is set to `openid https://graph.microsoft.com/Files.ReadWrite.All`
> and is unnecessary to be changed.

After redirecting back to your site, `access_token` or error messages will be presented in the hash depending on whether authentication is successful.

### API calls

There are two ways to call APIs.

The original APIs are exposed so you can use them directly:

```ts
import { uploadSmall, createUploadSession, uploadLargeChunk, share, getShareUrl } from '@harrisoff/onedrive-api'
```

Or you can use constructor to create a client instance, which wraps the original APIs:

```ts
import OneDriveAPI, { getShareUrl } from '@harrisoff/onedrive-api'

const client = new OneDriveApi({ accessToken })
const { id: fileId } = await client.upload(file, filePath)
const { shareId } = await client.share(fileId)
const sharingLink = getShareUrl(shareId)
```

## TODO List

- [x] more details about setting up application
- [ ] progress callback
- [ ] test suite
- [ ] standardize error code

## Development

For more details, see:

- [Official OneDrive API Document](https://docs.microsoft.com/en-us/onedrive/developer/)
- [some notes for my personal reference](./NOTES.md)
