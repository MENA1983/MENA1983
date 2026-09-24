# External Video Runner

This directory is the production-side renderer that stays outside the private AI Viral Content Factory.

## Pipeline

Private Factory -> HTTPS manifest -> External Runner -> MP4 -> public GitHub release asset -> later platform publication

The runner contains no private factory source, OAuth credentials, AI keys, or platform tokens.

## Manifest contract

Schema: `external-video/v1`.

Each scene contains narration and one or more HTTPS image URLs. A scene may optionally provide an HTTPS `audio_url`; otherwise eSpeak-ng is used.

Limits are deliberately bounded: 1-20 scenes, at most 40 images, at most 6 images per scene, and at most 180 seconds of final video.

## Why external

The private factory keeps orchestration, policy, rights, publication, reconciliation, and learning. Heavy media rendering happens on a public GitHub Actions runner so it does not consume the private repository's Actions quota.

GitHub currently documents standard GitHub-hosted runners as free and unlimited for public repositories. This is an execution advantage, not a guarantee of unlimited production: asset hosting, workflow concurrency, platform limits, rights, and policies still apply.

## Security

Only the public manifest and declared public assets cross the boundary. Never place secrets, OAuth tokens, cookies, private URLs, or private factory source in the manifest.

`workflow_dispatch` is manual-first here. GitHub requires the workflow file to be on the default branch for the Run workflow button.