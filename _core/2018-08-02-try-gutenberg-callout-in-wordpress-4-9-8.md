---
title : “グーテンベルグを試してみよう” という呼びかけが WordPress 4.9.8 に含まれました。
E-title : “Try Gutenberg” Callout in WordPress 4.9.8
megane : megane
update : 2018.08.03
moto : https://make.wordpress.org/core/2018/08/02/try-gutenberg-callout-in-wordpress-4-9-8/
layout : default
---


WordPress 4.9.8 will contain the “Try Gutenberg” callout, encouraging site owners to install the Gutenberg plugin, to test how their existing content and plugins works with the block editor. It also presents the option of installing the Classic Editor plugin, should they feel that they need more time to prepare for switching over to the block editor.

> WordPress 4.9.8では既存のコンテンツやプラグインがブロックエディターと一緒に動くかどうかをテストするために、グーテンベルグプラグインのインストールを促す、“グーテンベルグを試してみよう” という呼びかけが表示されるようになります。また、ブロックエディターに変更するのは時間が必要だと感じる人のために、オプションとしてクラシックエディタープラグインがあるよという内容も表示されます。

Screenshot of the “Try Gutenberg” callout in place on the Dashboard.

> “グーテンベルグを試してみよう” という呼びかけがダッシュボードに表示されているスクリーンショット

In WordPress 4.9.8, the callout will be shown to the following users:

> WordPress 4.9.8ではこの呼びかけが以下のユーザーに表示されます。



- If Gutenberg is not installed or activated, the callout will be shown to Admin users on single sites, and Super Admin users on multisites. (Based on the install_plugins capability.)

> まだグーテンベルグプラグインをインストールしてないもしくは、有効化していない場合は、呼びかけが、シングルサイトの場合は管理者、マルチサイトの場合は特権管理者のみに表示されます。(基本的はプラグインのインストールが可能なユーザー)

- If Gutenberg is installed and activated, the callout will be shown to Contributor users and above. (Based on the edit_posts capability.)

> グーテンベルグプラグインがインストールしてあり、有効化されている場合は、呼びかけが寄稿者以上の権限ユーザーに表示されます。(基本的は投稿の編集が可能なユーザー)

- If the Classic Editor plugin is installed and activated, the callout will be hidden for all users.

> もしクラシックエディターがインストールされ、有効化されている場合は呼びかけは、すべてのユーザー権限において非表示となります。


## Actions and Filters

> アクションとフィルターについて

The callout is attached to the try_gutenberg_panel action. If you would like to remove it on sites that you administer, you can do so with this snippet:

> 呼びかけは `try_gutenberg_panel` というアクションに登録されています。もしも、管理者権限でも呼びかけを削除したい場合は、以下のスニペットのようにすると、削除が可能です。


```
remove_action( 'try_gutenberg_panel', 'wp_try_gutenberg_panel' );
```

The “Learn more about Gutenberg” link currently directs to https://wordpress.org/gutenberg (or your localised version). However, particularly for hosts, you may have special instructions for your customers to install Gutenberg. In that case, the try_gutenberg_learn_more_link filter allows you to change this link, like so:

> ”グーテンベルグエディターを更に学ぶ” のリンクは基本的には https://wordpress.org/gutenberg (もしくは、翻訳された該当のページ)となります。しかし、特にホスティングなどをされている場合で、貴方が顧客に対して、グーテンベルグのインストールの内容について個別な方法を伝える必要があるかもしれません。そんなときは、 `try_gutenberg_learn_more_link filter` を利用し、以下のようにすることで、リンク先を変更することが可能です。

```
function my_host_learn_more_link( $link ) {
    return '<a href="https://support.my.host/gutenberg">Learn more about Gutenberg at My Host</a>';
}
 
add_filter( 'try_gutenberg_learn_more_link', 'my_host_learn_more_link' ); 
```
