API overview
============

## Contents

- [Available libraries](#available-libraries)
- [Notes](#notes)
- [Methods](#methods)
  - [Accounts](#accounts)
  - [Apps](#apps)
  - [Blocks](#blocks)
  - [Favourites](#favourites)
  - [Follow Requests](#follow-requests)
  - [Follows](#follows)
  - [Instances](#instances)
  - [Media](#media)
  - [Mutes](#mutes)
  - [Notifications](#notifications)
  - [Reports](#reports)
  - [Search](#search)
  - [Statuses](#statuses)
  - [Timelines](#timelines)
- [Entities](#entities)
  - [Account](#account)
  - [Application](#application)
  - [Attachment](#attachment)
  - [Card](#card)
  - [Context](#context)
  - [Error](#error)
  - [Instance](#instance)
  - [Mention](#mention)
  - [Notification](#notification)
  - [Relationship](#relationship)
  - [Results](#results)
  - [Status](#status)
  - [Tag](#tag)

___

## Available libraries

| Language             | Library                                                                        | Developer(s)                                                                   |
| -------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| Apex (Salesforce)    | [apex-mastodon](https://github.com/tzmfreedom/apex-mastodon)                   |                                                                                |
| C# (.NET Standard)   | [Mastodot](https://github.com/yamachu/Mastodot)                                |                                                                                |
| C# (.NET Standard)   | [Mastonet](https://github.com/glacasa/Mastonet)                                |                                                                                |
| C# (.NET)            | [mastodon-api-cs](https://github.com/pawotter/mastodon-api-cs)                 |                                                                                |
| C# (.NET)            | [Mastodon.Net](https://github.com/Tlaster/Mastodon.Net)                        |                                                                                |
| Crystal              | [mastodon.cr](https://github.com/decors/mastodon.cr)                           |                                                                                |
| Elixir               | [hunter](https://github.com/milmazz/hunter)                                    |                                                                                |
| Go                   | [go-mastodon](https://github.com/mattn/go-mastodon)                            |                                                                                |
| Go                   | [madon](https://github.com/McKael/madon)                                       |                                                                                |
| Haskell              | [hastodon](https://github.com/syucream/hastodon)                               |                                                                                |
| Java                 | [mastodon4j](https://github.com/sys1yagi/mastodon4j)                           |                                                                                |
| JavaScript           | [libodonjs](https://github.com/Zatnosk/libodonjs)                              |                                                                                |
| Javascript (Browser) | [mastodon.js](https://github.com/Kirschn/mastodon.js)                          |                                                                                |
| JavaScript (Node.js) | [node-mastodon](https://github.com/jessicahayley/node-mastodon)                |                                                                                |
| Perl                 | [Mastodon::Client](https://metacpan.org/pod/Mastodon::Client)                  | [@jjatria@mastodon.cloud](https://mastodon.cloud/@jjatria)                     |
| PHP                  | [Mastodon-api-php](https://github.com/yks118/Mastodon-api-php)                 |                                                                                |
| PHP                  | [MastodonOAuthPHP](https://github.com/TheCodingCompany/MastodonOAuthPHP)       |                                                                                |
| PHP                  | [Phediverse Mastodon REST Client](https://github.com/phediverse/mastodon-rest) |                                                                                |
| PHP                  | [TootoPHP](https://framagit.org/MaxKoder/TootoPHP)                             |                                                                                |
| Python               | [Mastodon.py](https://github.com/halcy/Mastodon.py)                            |                                                                                |
| R                    | [mastodon](https://github.com/ThomasChln/mastodon)                             |                                                                                |
| Ruby                 | [mastodon-api](https://github.com/tootsuite/mastodon-api)                      | [@Gargron@mastodon.social](https://mastodon.social/@Gargron)                   |
| Rust                 | [mammut](https://github.com/Aaronepower/mammut)                                |                                                                                |
| Scala                | [scaladon](https://github.com/schwitzerm/scaladon)                             |                                                                                |
| Swift                | [MastodonKit](https://github.com/ornithocoder/MastodonKit)                     | [@ornithocoder@mastodon.technology](https://mastodon.technology/@ornithocoder) |

___

## Notes

### Parameter types

When an array parameter is mentioned, the Rails convention of specifying array parameters in query strings is meant.
For example, a ruby array like `foo = [1, 2, 3]` can be encoded in the params as `foo[]=1&foo[]=2&foo[]=3`.
Square brackets can be indexed but can also be empty.

When a file parameter is mentioned, a form-encoded upload is expected.

### Selecting ranges

For most `GET` operations that return arrays, the query parameters `max_id` and `since_id` can be used to specify the range of IDs to return.
API methods that return collections of items can return a `Link` header containing URLs for the `next` and `prev` pages.
See the [Link header RFC](https://tools.ietf.org/html/rfc5988) for more information.

### Errors

If the request you make doesn't go through, Mastodon will usually respond with an [Error](#error).

___

## Methods

### Accounts

#### Fetching an account:

    GET /api/v1/accounts/:id

Returns an [Account](#account).

#### Getting the current user:

    GET /api/v1/accounts/verify_credentials

Returns the authenticated user's [Account](#account).

#### Updating the current user:

    PATCH /api/v1/accounts/update_credentials

Form data:

| Field          | Description                                                                                                                            | Optional   |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| `display_name` | The name to display in the user's profile                                                                                              | yes        |
| `note`         | A new biography for the user                                                                                                           | yes        |
| `avatar`       | A base64 encoded image to display as the user's avatar (e.g. `data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAADrCAYAAAA...`)       | yes        | 
| `header`       | A base64 encoded image to display as the user's header image (e.g. `data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAADrCAYAAAA...`) | yes        |

#### Getting an account's followers:

    GET /api/v1/accounts/:id/followers

Query parameters:

| Field      | Description                                                    | Optional   |
| ---------- | -------------------------------------------------------------- | ---------- |
| `max_id`   | Get a list of followers with ID less than or equal this value  | yes        |
| `since_id` | Get a list of followers with ID greater than this value        | yes        |
| `limit`    | Maximum number of followers to get (Default 40, Max 80)        | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns an array of [Accounts](#account).

#### Getting who account is following:

    GET /api/v1/accounts/:id/following

Query parameters:

| Field      | Description                                                    | Optional   |
| ---------- | -------------------------------------------------------------- | ---------- |
| `max_id`   | Get a list of followings with ID less than or equal this value | yes        |
| `since_id` | Get a list of followings with ID greater than this value       | yes        |
| `limit`    | Maximum number of followings to get (Default 40, Max 80)       | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns an array of [Accounts](#account).

#### Getting an account's statuses:

    GET /api/v1/accounts/:id/statuses

Query parameters:

| Field             | Description                                                  | Optional   |
| ----------------- | -----------------------------------------------------------  | ---------- |
| `only_media`      | Only return statuses that have media attachments             | yes        |
| `exclude_replies` | Skip statuses that reply to other statuses                   | yes        |
| `max_id`          | Get a list of statuses with ID less than or equal this value | yes        |
| `since_id`        | Get a list of statuses with ID greater than this value       | yes        |
| `limit`           | Maximum number of statuses to get (Default 20, Max 40)       | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns an array of [Statuses](#status).

#### Following/unfollowing an account:

    POST /api/v1/accounts/:id/follow
    POST /api/v1/accounts/:id/unfollow

Returns the target account's [Relationship](#relationship).

#### Blocking/unblocking an account:

    POST /api/v1/accounts/:id/block
    POST /api/v1/accounts/:id/unblock

Returns the target account's [Relationship](#relationship).

#### Muting/unmuting an account:

    POST /api/v1/accounts/:id/mute
    POST /api/v1/accounts/:id/unmute

Returns the target account's [Relationship](#relationship).

#### Getting an account's relationships:

    GET /api/v1/accounts/relationships

Query parameters:

| Field | Description                   | Optional   |
| ----- | ----------------------------  | ---------- |
| `id`  | Account IDs (can be an array) | no         |

Returns an array of [Relationships](#relationships) of the current user to a list of given accounts.

#### Searching for accounts:

    GET /api/v1/accounts/search

Query parameters:

| Field             | Description                                                   | Optional   |
| ----------------- | ------------------------------------------------------------- | ---------- |
| `q`               | What to search for                                            | no         |
| `limit`           | Maximum number of matching accounts to return (default: `40`) | yes        |

Returns an array of matching [Accounts](#accounts).

Will lookup an account remotely if the search term is in the `username@domain` format and not yet in the database.

### Apps

#### Registering an application:

    POST /api/v1/apps

Form data:

| Field             | Description                                                                                                                                                   | Optional   |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| `client_name`     | Name of your application                                                                                                                                      | no         |
| `redirect_uris`   | Where the user should be redirected after authorization (for no redirect, use `urn:ietf:wg:oauth:2.0:oob`)                                                    | no         |
| `scopes`          | This can be a space-separated list of the following items: "read", "write" and "follow" (see [this page](OAuth-details.md) for details on what the scopes do) | no         |
| `website`         | URL to the homepage of your app                                                                                                                               | yes        |

Creates a new OAuth app.

Returns `id`, `client_id` and `client_secret` which can be used with [OAuth authentication in your 3rd party app](Testing-with-cURL.md).

These values should be requested in the app itself from the API for each new app install + mastodon domain combo, and stored in the app for future requests.

### Blocks

#### Fetching a user's blocks:

    GET /api/v1/blocks

Query parameters:

| Field             | Description                                                   | Optional   |
| ----------------- | ------------------------------------------------------------- | ---------- |
| `max_id`          | Get a list of blocks with ID less than or equal this value    | yes        |
| `since_id`        | Get a list of blocks with ID greater than this value          | yes        |
| `limit`           | Maximum number of blocks to get (Default 40, Max 80)          | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns an array of [Accounts](#account) blocked by the authenticated user.

### Favourites

#### Fetching a user's favourites:

    GET /api/v1/favourites

Query parameters:

| Field             | Description                                                    | Optional   |
| ----------------- | -------------------------------------------------------------- | ---------- |
| `max_id`          | Get a list of favourites with ID less than or equal this value | yes        |
| `since_id`        | Get a list of favourites with ID greater than this value       | yes        |
| `limit`           | Maximum number of favourites to get (Default 20, Max 40)       | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns an array of [Statuses](#status) favourited by the authenticated user.

### Follow Requests

#### Fetching a list of follow requests:

    GET /api/v1/follow_requests

Query parameters:

| Field             | Description                                                         | Optional   |
| ----------------- | ------------------------------------------------------------------- | ---------- |
| `max_id`          | Get a list of follow requests with ID less than or equal this value | yes        |
| `since_id`        | Get a list of follow requests with ID greater than this value       | yes        |
| `limit`           | Maximum number of requests to get (Default 40, Max 80)              | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns an array of [Accounts](#account) which have requested to follow the authenticated user.

#### Authorizing or rejecting follow requests:

    POST /api/v1/follow_requests/:id/authorize
    POST /api/v1/follow_requests/:id/reject

Parameters:

| Field             | Description                                                         | Optional   |
| ----------------- | ------------------------------------------------------------------- | ---------- |
| `id`              | The id of the account to authorize or reject                        | no         |

Returns an empty object.

### Follows

#### Following a remote user:

    POST /api/v1/follows

Form data:

| Field             | Description                                                         | Optional   |
| ----------------- | ------------------------------------------------------------------- | ---------- |
| `uri`             | `username@domain` of the person you want to follow                  | no         |

Returns the local representation of the followed account, as an [Account](#account).

### Instances

#### Getting instance information:

    GET /api/v1/instance

Returns the current [Instance](#instance).

Does not require authentication.

### Media

#### Uploading a media attachment:

    POST /api/v1/media

Form data:

| Field             | Description                                                         | Optional   |
| ----------------- | ------------------------------------------------------------------- | ---------- |
| `file`            | Media to be uploaded                                                | no         |

Returns an [Attachment](#attachment) that can be used when creating a status.

### Mutes

#### Fetching a user's mutes:

    GET /api/v1/mutes

Query parameters:

| Field             | Description                                                         | Optional   |
| ----------------- | ------------------------------------------------------------------- | ---------- |
| `max_id`          | Get a list of mutes with ID less than or equal this value           | yes        |
| `since_id`        | Get a list of mutes with ID greater than this value                 | yes        |
| `limit`           | Maximum number of mutes to get (Default 40, Max 80)                 | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns an array of [Accounts](#account) muted by the authenticated user.

### Notifications

#### Fetching a user's notifications:

    GET /api/v1/notifications

Query parameters:

| Field             | Description                                                         | Optional   |
| ----------------- | ------------------------------------------------------------------- | ---------- |
| `max_id`          | Get a list of notifications with ID less than or equal this value   | yes        |
| `since_id`        | Get a list of notifications with ID greater than this value         | yes        |
| `limit`           | Maximum number of notifications to get (Default 15, Max 30)         | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns a list of [Notifications](#notification) for the authenticated user.

#### Getting a single notification:

    GET /api/v1/notifications/:id

Returns the [Notification](#notification).

#### Clearing notifications:

    POST /api/v1/notifications/clear

Deletes all notifications from the Mastodon server for the authenticated user.
Returns an empty object.

### Reports

#### Fetching a user's reports:

    GET /api/v1/reports

Returns a list of [Reports](#report) made by the authenticated user.

#### Reporting a user:

    POST /api/v1/reports

Form data:

| Field             | Description                                                         | Optional   |
| ----------------- | ------------------------------------------------------------------- | ---------- |
| `account_id`      | The ID of the account to report                                     | no         |
| `status_ids`      | The IDs of statuses to report (can be an array)                     | no         |
| `comment`         | A comment to associate with the report                              | no         |

Returns the finished [Report](#report).

### Search

#### Searching for content:

    GET /api/v1/search

Form data:

| Field             | Description                                                         | Optional   |
| ----------------- | ------------------------------------------------------------------- | ---------- |
| `q`               | The search query                                                    | no         |
| `resolve`         | Whether to resolve non-local accounts                               | no         |

Returns [Results](#results).

If `q` is a URL, Mastodon will attempt to fetch the provided account or status. Otherwise, it will do a local account and hashtag search.

Does not require authentication.

### Statuses

#### Fetching a status:

    GET /api/v1/statuses/:id

Returns a [Status](#status).

Does not require authentication.

#### Getting status context:

    GET /api/v1/statuses/:id/context

Returns a [Context](#context).

Does not require authentication.

#### Getting a card associated with a status:

    GET /api/v1/statuses/:id/card

Returns a [Card](#card).

Does not require authentication.

#### Getting who reblogged/favourited a status:

    GET /api/v1/statuses/:id/reblogged_by
    GET /api/v1/statuses/:id/favourited_by

Query parameters:

| Field             | Description                                                              | Optional   |
| ----------------- | ------------------------------------------------------------------------ | ---------- |
| `max_id`          | Get a list of reblogged/favourited with ID less than or equal this value | yes        |
| `since_id`        | Get a list of reblogged/favourited with ID greater than this value       | yes        |
| `limit`           | Maximum number of reblogged/favourited to get (Default 40, Max 80)       | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns an array of [Accounts](#account).

Does not require authentication.

#### Posting a new status:

    POST /api/v1/statuses

Form data:

| Field             | Description                                                              | Optional   |
| ----------------- | ------------------------------------------------------------------------ | ---------- |
| `status`          | The text of the status                                                   | no         |
| `in_reply_to_id`  | local ID of the status you want to reply to                              | yes        |
| `media_ids`       | Array of media IDs to attach to the status (maximum 4)                   | yes        |
| `sensitive`       | Set this to mark the media of the status as NSFW                         | yes        |
| `spoiler_text`    | Text to be shown as a warning before the actual content                  | yes        |
| `visibility`       | Either "direct", "private", "unlisted" or "public"                      | yes        |

Returns the new [Status](#status).

#### Deleting a status:

    DELETE /api/v1/statuses/:id

Returns an empty object.

#### Reblogging/unreblogging a status:

    POST /api/v1/statuses/:id/reblog
    POST /api/v1/statuses/:id/unreblog

Returns the target [Status](#status).

#### Favouriting/unfavouriting a status:

    POST /api/v1/statuses/:id/favourite
    POST /api/v1/statuses/:id/unfavourite

Returns the target [Status](#status).

### Timelines

#### Retrieving a timeline:

    GET /api/v1/timelines/home
    GET /api/v1/timelines/public
    GET /api/v1/timelines/tag/:hashtag

Query parameters:

| Field             | Description                                                                         | Optional   |
| ----------------- | ----------------------------------------------------------------------------------- | ---------- |
| `local`           | Only return statuses originating from this instance (public and tag timelines only) | yes        |
| `max_id`          | Get a list of timelines with ID less than or equal this value                       | yes        |
| `since_id`        | Get a list of timelines with ID greater than this value                             | yes        |
| `limit`           | Maximum number of statuses on the requested timeline to get (Default 20, Max 40)    | yes        |

`max_id` and `since_id` are usually get from the `Link` header.

Returns an array of [Statuses](#status), most recent ones first.

'public' and 'tag' timelines do not require authentication.

___

## Entities

### Account

| Attribute                | Description |
| ------------------------ | ----------- |
| `id`                     | The ID of the account |
| `username`               | The username of the account |
| `acct`                   | Equals `username` for local users, includes `@domain` for remote ones |
| `display_name`           | The account's display name |
| `locked`                 | Boolean for when the account cannot be followed without waiting for approval first |
| `created_at`             | The time the account was created |
| `followers_count`        | The number of followers for the account |
| `following_count`        | The number of accounts the given account is following |
| `statuses_count`         | The number of statuses the account has made |
| `note`                   | Biography of user |
| `url`                    | URL of the user's profile page (can be remote) |
| `avatar`                 | URL to the avatar image |
| `avatar_static`          | URL to the avatar static image (gif) |
| `header`                 | URL to the header image |
| `header_static`          | URL to the header static image (gif) |

### Application

| Attribute                | Description |
| ------------------------ | ----------- |
| `name`                   | Name of the app |
| `website`                | Homepage URL of the app |

### Attachment

| Attribute                | Description |
| ------------------------ | ----------- |
| `id`                     | ID of the attachment |
| `type`                   | One of: "image", "video", "gifv" |
| `url`                    | URL of the locally hosted version of the image |
| `remote_url`             | For remote images, the remote URL of the original image |
| `preview_url`            | URL of the preview image |
| `text_url`               | Shorter URL for the image, for insertion into text (only present on local images) |

### Card

| Attribute                | Description |
| ------------------------ | ----------- |
| `url`                    | The url associated with the card |
| `title`                  | The title of the card |
| `description`            | The card description |
| `image`                  | The image associated with the card, if any |

### Context

| Attribute                | Description |
| ------------------------ | ----------- |
| `ancestors`              | The ancestors of the status in the conversation, as a list of [Statuses](#status) |
| `descendants`            | The descendants of the status in the conversation, as a list of [Statuses](#status) |

### Error

| Attribute                | Description |
| ------------------------ | ----------- |
| `error`                  | A textual description of the error |

### Instance

| Attribute                | Description |
| ------------------------ | ----------- |
| `uri`                    | URI of the current instance |
| `title`                  | The instance's title |
| `description`            | A description for the instance |
| `email`                  | An email address which can be used to contact the instance administrator |

### Mention

| Attribute                | Description |
| ------------------------ | ----------- |
| `url`                    | URL of user's profile (can be remote) |
| `username`               | The username of the account |
| `acct`                   | Equals `username` for local users, includes `@domain` for remote ones |
| `id`                     | Account ID |

### Notification

| Attribute                | Description |
| ------------------------ | ----------- |
| `id`                     | The notification ID |
| `type`                   | One of: "mention", "reblog", "favourite", "follow" |
| `created_at`             | The time the notification was created |
| `account`                | The [Account](#account) sending the notification to the user |
| `status`                 | The [Status](#status) associated with the notification, if applicable |

### Relationship

| Attribute                | Description |
| ------------------------ | ----------- |
| `id`                     | Target account id |
| `following`              | Whether the user is currently following the account |
| `followed_by`            | Whether the user is currently being followed by the account |
| `blocking`               | Whether the user is currently blocking the account |
| `muting`                 | Whether the user is currently muting the account |
| `requested`              | Whether the user has requested to follow the account |

### Report

| Attribute                | Description |
| ------------------------ | ----------- |
| `id`                     | The ID of the report |
| `action_taken`           | The action taken in response to the report |

### Results

| Attribute                | Description |
| ------------------------ | ----------- |
| `accounts`               | An array of matched [Accounts](#account) |
| `statuses`               | An array of matchhed [Statuses](#status) |
| `hashtags`               | An array of matched hashtags, as strings |

### Status

| Attribute                | Description |
| ------------------------ | ----------- |
| `id`                     | The ID of the status |
| `uri`                    | A Fediverse-unique resource ID |
| `url`                    | URL to the status page (can be remote) |
| `account`                | The [Account](#account) which posted the status |
| `in_reply_to_id`         | `null` or the ID of the status it replies to |
| `in_reply_to_account_id` | `null` or the ID of the account it replies to |
| `reblog`                 | `null` or the reblogged [Status](#status) |
| `content`                | Body of the status; this will contain HTML (remote HTML already sanitized) |
| `created_at`             | The time the status was created |
| `reblogs_count`          | The number of reblogs for the status |
| `favourites_count`       | The number of favourites for the status |
| `reblogged`              | Whether the authenticated user has reblogged the status |
| `favourited`             | Whether the authenticated user has favourited the status |
| `sensitive`              | Whether media attachments should be hidden by default |
| `spoiler_text`           | If not empty, warning text that should be displayed before the actual content |
| `visibility`             | One of: `public`, `unlisted`, `private`, `direct` |
| `media_attachments`      | An array of [Attachments](#attachment) |
| `mentions`               | An array of [Mentions](#mention) |
| `tags`                   | An array of [Tags](#tag) |
| `application`            | [Application](#application) from which the status was posted |

### Tag

| Attribute                | Description |
| ------------------------ | ----------- |
| `name`                   | The hashtag, not including the preceding `#` |
| `url`                    | The URL of the hashtag |
