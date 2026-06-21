---
name: mastodon-content-creator
description: Creates Mastodon posts with text and media, including automated image description using vision capabilities.
version: 1.0.0
platforms: [macos, linux]
metadata:
  hermes:
    tags: [mastodon, social-media, content-creation]
    category: writing
required_environment_variables:
  - name: MASTODON_INSTANCE
    prompt: Mastodon Instance URL
    help: The base URL of your Mastodon instance (e.g., https://mastodon.social)
    required_for: full functionality
  - name: MASTODON_ACCESS_TOKEN
    prompt: Mastodon Access Token
    help: Your Mastodon access token for the authenticated account
    required_for: full functionality
---

# Mastodon Content Creator

## When to Use
Activate this skill when the user requests to "post a status", "share something on Mastodon", or any request involving creating content (text, text with media) for a Mastodon instance.

## Procedure
1. **Status Search (if required)**: If the user wants to find a specific status but does not provide an ID, perform a search based on their intent:
    - **By User**: Use `account_statuses` with the target account's ID.
    - **By Content/Hashtag**: Use `search` with the appropriate query and `search_type`.
    - **If intent is unclear**: Suggest these search methods (by user, by content/hashtag) to the user.
2. **Media Analysis (if applicable)**: If the user provides media (image, video, or audio), use a vision-enabled model to generate a detailed description of the media content.
2. **Media Upload**: 
    - Use `media_post` with the absolute path to the media file.
    - Pass the generated description as the `description` parameter for accessibility/alt-text.
    - Capture and store the returned `id` (the `media_id`).
3. **Status Creation**: 
    - Use `status_post` with the user's provided text as the `content`.
    - If media was uploaded, include the `media_id` in the `media_ids` parameter.
    - Set visibility (e.g., 'public', 'unlisted') based on user preference or default to 'public'.
4. **Status Update**: 
    - If the user requests to modify an existing post, use `status_update` with the target `status_id`.
    - You can update the text content, spoiler text, or sensitivity settings.
5. **Status Deletion**: 
    - If the user requests to delete a post, you **MUST** first identify the status. If the ID is unknown, use the **Status Search** procedure to find it.
    - Once the status is identified, retrieve its details using `status_get` with the target `status_id`.
    - Present the content of the status to the user and ask for explicit confirmation (e.g., "Are you sure you want to delete the following post? [Post Content]").
    - Once confirmed, use `status_delete` with the target `status_id`.
6. **Final Output**: Return the direct URL of the status (for creation/update) or a confirmation message (for deletion) as a plain text string.

## Pitfalls
- **Media Upload Failure**: If `media_post` fails, inform the user and attempt to post text-only if appropriate.
- **Read-Only Mode**: If the server is in `READ_ONLY` mode, notify the user that posting is disabled.
- **Invalid Media Path**: Ensure the media file path provided is absolute and accessible to the server.

## Verification
- Confirm that `status_post` returns a successful status object containing the new status ID.
- Verify that the returned URL is correctly formatted and points to the intended post.