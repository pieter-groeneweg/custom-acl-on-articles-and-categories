## Why and how it works.

As there is no blocking of editing content from other users, working in separate teams allows changes made by people you don't want to be able to change but still work on their own content.
This is not (yet) a standard feature but can be accomplished by making some changes using template overrides.

It is mainly replacing this:

`$canChange  = $user->authorise('core.edit.state', 'com_content.article.' . $item->id) && $canCheckin;`
					
with:
					
`if(!$canEdit && !$canEditOwn) {`
`$canChange  = $user->authorise('core.edit.state', 'com_content.article.'.$item->id) && ($item->created_by == $userId || $user->authorise('core.admin', 'com_content.article.'.$item->id));`
`}`
`else {`
`$canChange  = $user->authorise('core.edit.state', 'com_content.article.' . $item->id) && $canCheckin;`
`}`

I used it to make overrides in the ISIS admin template. I assume this to work in other templates also. Check template overrides in the Joomla docs. Overrides front end work same way as in admin end.

After copying the files in this repository to isis template html folder you follow next steps.
You will need to setup some ACL rules.

## ACL Settings
This is how I made this work. There maybe other configurations that suit you better.
In groups, create an new Author Group with parent "Registered". ie "MyAuthorGroup".
Then go to global settings, articles, tab Permissions. Select "MyAuthorGroup".

(I like my authors to work in the admin interface. So I allow them "Access Administration Interface" but that is my preferred choice.)

Allow "Create", "Edit State", "Edit Own". You may need to allow "Edit Custom Field Value" if required.
Add users to this group.

Save the changes.

Now you see with global ACL you can do a simple setup. With more indepth ACL configuration you can setup access and deny on category level and this way create Author Group managers..



