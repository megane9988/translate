---
title : 組織としての5.0への技術的な対応方法
E-title : Technical organisation post-5.0
megane : megane
update : 2018.11.15
moto : https://make.wordpress.org/core/2018/11/14/technical-organisation-post-5-0/
layout : default
---

Once WordPress 5.0 is released, the intention is to have a minor WordPress release twice a month. These releases will be focused on editor improvements and bug fixes. They can also include bug fixes beyond the editor.

> WordPress5.0がリリースされた後は、意図的に月に2回程度の小さなアップデートを行っていきます。このアップデートは主にエディターの改善やバグフィックスについてです。エディター以外のバグ修正などを含む場合もあります。


## Repositories リポジトリーについて

Work on the editor will continue on GitHub, while work on WordPress Core will continue in Trac. The JavaScript packages from GitHub, which represent more than just Gutenberg code, will be updated on a regular basis and merged into WordPress Core.

> 引き続きエディターについてはGitHubで開発を続けます。その間もコアはTracで開発を続けていきます。グーテンベルグだけでなく、JavaScriptのパッケージもGitHubで開発し、定期的にWordPressのコアにマージを行っていきます。

A typical Workflow for a Core bug fix involving a JavaScript package will follow this process:

> コアにおけるJavaScriptのパッケージ関連の修正には以下のような工程で追従していく予定です。

1. A Trac ticket is created to track the bug resolution.
1. If the issue is found to be related to a JavaScript package, a corresponding GitHub issue is created referencing the Trac ticket.
1. The issue is resolved in a GitHub pull request and merged to master.

> 1. Tracにおいて不具合の修正を追跡するTicketが作成されます
> 2. もしもその不具合がJavaScriptのパッケージに関連するのものの場合は、Githubにイシューを立て、Tracの該当するTicketを関連付けます。
> 3. イシューはプルリクエストによって解決され、masterブランチにマージされます。

Then, on a regular basis, the Editor Team will:

> その後は、エディターチームが以下の作業を定期的に行います。

1. Publish updates to the WordPress npm packages from the Gutenberg repository.
1. Update the packages in Core.
1. Close the Trac tickets fixed by the packages update.

> 1. グーテンベルグのリポジトリから、WordPress用のnpmのパッケージをアップデートします。
> 2. コアをアップデートします
> 3. TracのTicketはパッケージがアップデートされたことで解決として閉じます。

In addition to the editor improvements, prototypes, and implementation of Phase 2 work will also happen in the GitHub repository. We will use feature flags to avoid including the Phase 2 work in the WordPress Core package updates. This will allow us to continue using the Gutenberg plugin releases for beta testing.

> エディターの改善、フェーズ2に向けたプロトタイプの開発は引き続きGitHubのリポジトリーで行います。その際、feature_flagsを利用して、フェーズ2とWordPressのコアのパッケージアップデートに含まれないようにします。これで、グーテンベルグプラグインのリリースとβテストを続けることができます。
