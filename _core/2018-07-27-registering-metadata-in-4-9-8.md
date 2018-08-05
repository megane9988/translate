---
title : 4.9.8 に Metadata の登録が含まれました。【翻訳中】
E-title : Registering Metadata in 4.9.8
megane : megane
update : 2018.08.03
moto : https://make.wordpress.org/core/2018/07/27/registering-metadata-in-4-9-8/
layout : default
---


With WordPress 4.9.8, the `register_meta()` function supports registration of metadata not only for an entire object type (posts, terms, comments, users), but also for a specific object subtype (such as a specific post type or taxonomy).

WordPress 4.9.8では `register_meta()` によるメタデータの登録について、オブジェクトタイプ（投稿、分類、コメント、ユーザー）全体だけでなく、特定のオブジェクトのサブタイプ(カスタム投稿タイプやタクソノミー)にも対応するようになりました。


Since [the enhanced way of metadata registration was introduced in WordPress 4.6](https://make.wordpress.org/core/2016/07/20/additional-register_meta-changes-in-4-6/) developers could register their meta keys for an entire object type (post, term, commentor user). 

「WordPress4.6におけるメタデータ登録の拡張方法の紹介」において、開発者は、それらのメタキーをオブジェクトタイプ（投稿、分類、コメント、ユーザー）全体に登録で切るようにしました。

Post meta is the most popular use-case for register_meta, so most current uses of register_meta specify the object type post. While a given post meta key is often specific to only one post type, meta registration did not account for object subtypes, causing any registered post meta keys that specify "show_in_rest" => true to appear in the REST API responses for any and all post types.

As long as registered meta keys were appropriately prefixed, this produced only a bit of unnecessary metadata being present in some endpoints. However when two meta keys of the same name were registered by different plugins for use with different post types this could introduce conflicts and possible data inaccuracy, as well as potential capability and thus security implications.

In WordPress 4.9.8, meta keys specific to only one or a few post types may now be properly registered without impacting meta keys for unrelated post types. This also applies to term metadata specific to only one or a few taxonomies.

## How it works

Two new utility functions have been introduced to register metadata for posts and terms respectively. These functions are now the recommended way to register post and term meta:

To register post metadata, use `register_post_meta( $post_type, $meta_key, $args )`.
To register term metadata, use `register_term_meta( $taxonomy, $meta_key, $args )`.
The first parameter for each new method is the name of the post type or taxonomy for which to register the meta key, and the third parameter works exactly like the third parameter of the existing register_meta() function.

By using these functions, the registered metadata is only associated with the specified post type/taxonomy (also more broadly called the object subtype). Its sanitization and authentication callbacks only affect that object subtype, and the registered key is only exposed in that specific object subtype’s REST API endpoint when show_in_rest is set to true.

Internally these two functions wrap register_meta(), which now supports a new object_subtype argument. register_post_meta and register_term_meta simply provide a more convenient way to specify this new parameter.

Example using register_post_meta():

```
register_post_meta( 'my_cpt', 'my_key', array(
    'show_in_rest'      =&gt; true,
    'sanitize_callback' =&gt; 'absint',
) );
Achieving exactly the same result using register_meta():

register_meta( 'post', 'my_key', array(
    'object_subtype'    =&gt; 'my_cpt',
    'show_in_rest'      =&gt; true,
    'sanitize_callback' =&gt; 'absint',
) );
```

It is possible to use register_post_meta() or register_term_meta() to call register_meta() just as before, without an object subtype, by passing an empty string as the first parameter. Going forward this is not recommended, as meta keys should be specific to only the object subtypes that they are needed for.

## Backward compatibility

All old code using register_meta() as before will continue to work. However, if you have been using the function with the intention to actually register meta keys for only a certain object subtype, it is recommended to adjust the code accordingly.

The changes have been implemented in a way that meta keys registered for a certain object subtype take precedence over those registered for an entire object type. An example:

Let’s say a site has three post types: “post”, “page”, and “attachment”.
Someone registers the meta key “my_key” for all posts of all post types, using register_meta() like before.
Someone else registers the meta key “my_key” only for the post type “page”, using register_post_meta( 'page', ... ).
The result is that for pages the arguments from the second registration are taken into account, while for posts and attachments the arguments from the first registration are taken into account.
What about comments and users?
Comments and users are object types that at this point do not support object subtypes. While there is technically a comment_type field in the comments database table, comment types are not clearly scoped in core (for example there is no register_comment_type()), so it was decided to leave them out for now.

For this reason, there are no register_comment_meta() or register_user_meta() functions at this point.

If you are interested in more about the history of these changes, you can read the initial background post for these efforts, or look into the related ticket #38323.

One last thing: Keep in mind, that, even though meta keys can now be handled per object subtype, it is still recommended to choose specific names that are appropriately prefixed.