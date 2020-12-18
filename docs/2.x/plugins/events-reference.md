# Events Reference

Craft provides several events that alert plugins when things are happening.

::: tip
See [Hooks and Events](hooks-and-events.md) for an explanation of how events work in Craft, and how they differ from hooks.
:::

## General Events

### onEditionChange

Raised by

:   <craft2:Craft\AppBehavior::setEdition()>

Raised after Craft’s edition changes.

#### Params:

- `edition` – The new edition (`0` for Solo, `2` for Pro)

### db.onBackup

Raised by

:   <craft2:Craft\DbConnection::backup()>

Raised after a database backup has been created.

#### Params:

- `filePath` – The path to the new backup file.

### email.onBeforeSendEmail

Raised by

:   <craft2:Craft\EmailService::sendEmail()>, <craft2:Craft\EmailService::sendEmailByKey()>

Raised right before an email is sent.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that is receiving the email.
- `emailModel` – The <craft2:Craft\EmailModel> defining the email to be sent.
- `variables` – Any variables that are going to be available to the email template.

::: tip
Event handlers can prevent the email from getting sent by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### email.onSendEmail

Raised by

:   <craft2:Craft\EmailService::sendEmail()>, <craft2:Craft\EmailService::sendEmailByKey()>

Raised when an email is sent.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that received the email.
- `emailModel` – The <craft2:Craft\EmailModel> defining the email that was sent.
- `variables` – Any variables that were available to the email template.

### email.onSendEmailError

Raised by

:   <craft2:Craft\EmailService::sendEmail()>, <craft2:Craft\EmailService::sendEmailByKey()>

Raised when an email fails to send.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that received the email.
- `emailModel` – The <craft2:Craft\EmailModel> defining the email that was sent.
- `variables` – Any variables that were available to the email template.
- `error` – The error defined by PHPMailer.

### i18n.onAddLocale

Raised by

:   <craft2:Craft\LocalizationService::addSiteLocale()>

Raised when a new locale is added to the site.

#### Params:

- `localeId` – The ID of the locale that was just added.

### i18n.onBeforeDeleteLocale

Raised by

:   <craft2:Craft\LocalizationService::deleteSiteLocale()>

Raised right before a locale is deleted.

#### Params:

- `localeId` – The ID of the locale that’s about to be deleted.
- `transferContentTo` – The ID of the locale that the deleted locale’s content should be transferred to, if any.

::: tip
Event handlers can prevent the locale from getting deleted by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### localization.onDeleteLocale

Raised by

:   <craft2:Craft\LocalizationService::deleteSiteLocale()>

Raised when a locale is deleted.

#### Params:

- `localeId` – The ID of the locale that was deleted.
- `transferContentTo` – The ID of the locale that the deleted locale’s content should have be transferred to, if any.

### plugins.onLoadPlugins

Raised by

:   <craft2:Craft\PluginsService::loadPlugins()>

Raised when Craft has finished loading all the plugins.

### updates.onBeginUpdate

Raised by

:   <craft2:Craft\UpdatesService::prepareUpdate()>

Raised when an update is beginning.

#### Params:

- `type` – A string either set to 'auto' or 'manual' indicating if the update is a manual update or auto-update.

### updates.onEndUpdate

Raised by

:   <craft2:Craft\UpdatesService::updateCleanUp()>

Raised when an update has ended.

#### Params:

- `success` – Set to true or false indicating if the update was successful or not.

## Element API Events

### content.onSaveContent

Raised by

:   <craft2:Craft\ContentService::saveContent()>

Raised when any element’s content is saved.

#### Params:

- `content` – A <craft2:Craft\ContentModel> object representing the saved content.
- `isNewContent` – A boolean indicating whether this was a new set of content.

### elements.onBeforeBuildElementsQuery

Raised by

:   <craft2:Craft\ElementsService::buildElementsQuery()>

Raised before Craft builds out an elements query, enabling plugins to modify the query or prevent it from actually happening.

#### Params:

- `criteria` – The <craft2:Craft\ElementCriteriaModel> object that defines the parameters for the query.
- `justIds` – `true` or `false` depending on whether the query should only be returning the IDs of the matched elements.
- `query` – The <craft2:Craft\DbCommand> object that is being built out.

::: tip
Event handlers can prevent the element query from being executed by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### elements.onBuildElementsQuery

Raised by

:   <craft2:Craft\ElementsService::buildElementsQuery()>

Raised after Craft has built out an elements query, enabling plugins to modify the query.

#### Params:

- `criteria` – The <craft2:Craft\ElementCriteriaModel> object that defines the parameters for the query.
- `justIds` – `true` or `false` depending on whether the query should only be returning the IDs of the matched elements.
- `query` – The <craft2:Craft\DbCommand> object that is being built out.

### elements.onBeforeDeleteElements

Raised by

:   <craft2:Craft\ElementsService::deleteElementById()>

Raised right before any elements are about to be deleted.

#### Params:

- `elementIds` – The element IDs that are about to be deleted.

### elements.onMergeElements

Raised by

:   <craft2:Craft\ElementsService::mergeElementsByIds()>

Raised when any element merged with another element.

#### Params:

- `mergedElementId` – The id of the element being merged.
- `prevailingElementId` – The id of the element that prevailed in the merge.

### structures.onBeforeMoveElement

Raised by

:   <craft2:Craft\StructuresService::prepend()>, <craft2:Craft\StructuresService::append()>, <craft2:Craft\StructuresService::prependToRoot()>, <craft2:Craft\StructuresService::appendToRoot()>, <craft2:Craft\StructuresService::moveBefore()>, <craft2:Craft\StructuresService::moveAfter()>

Raised right before an element is moved within a structure.

#### Params:

- `structureId` – The ID of the structure that the element is being moved within.
- `element` – A <craft2:Craft\BaseElementModel> object representing the element that is about to be moved.

::: tip
Event handlers can prevent the element from getting moved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### structures.onMoveElement

Raised by

:   <craft2:Craft\StructuresService::prepend()>, <craft2:Craft\StructuresService::append()>, <craft2:Craft\StructuresService::prependToRoot()>, <craft2:Craft\StructuresService::appendToRoot()>, <craft2:Craft\StructuresService::moveBefore()>, <craft2:Craft\StructuresService::moveAfter()>

Raised when an element is moved within a structure.

#### Params:

- `structureId` – The ID of the structure that the element was moved within.
- `element` – A <craft2:Craft\BaseElementModel> object representing the element that was moved.

### elements.onBeforePerformAction

Raised by

:   <craft2:Craft\ElementIndexController::actionPerformAction()>

Raised before a batch element action gets triggered.

#### Params:

- `action` – The element action class that is going to be performing the action.
- `criteria` – The <craft2:Craft\ElementCriteriaModel> object that defines which element(s.html) the user has chosen to perform the action on.

::: tip
Event handlers can prevent the element action from being triggered by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### elements.onPerformAction

Raised by

:   <craft2:Craft\ElementIndexController::actionPerformAction()>

Raised after a batch element action has been performed.

#### Params:

- `action` – The element action class that performed the action.
- `criteria` – The <craft2:Craft\ElementCriteriaModel> object that defines which element(s.html) the user had chosen to perform the action on.

### elements.onPopulateElement

Raised by

:   <craft2:Craft\ElementsService::findElements()>

Raised when any element model is populated from its database result.

#### Params:

- `element` – The populated element.
- `result` – The raw data representing the element from the database.

### elements.onPopulateElements

Raised by

:   <craft2:Craft\ElementsService::populateElements()>

Raised when all of the element models have been populated from an element query.

#### Params:

- `elements` – An array of the populated elements.
- `criteria` – The ElementCriteriaModel that was used to define the element query.

### elements.onBeforeSaveElement

Raised by

:   <craft2:Craft\ElementsService::saveElement()>

Raised right before an element is saved.

#### Params:

- `element` – A <craft2:Craft\BaseElementModel> object representing the element that is about to be saved.
- `isNewElement` – A boolean indicating whether this is a brand new element.

::: tip
Event handlers can prevent the element from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### elements.onSaveElement

Raised by

:   <craft2:Craft\ElementsService::saveElement()>

Raised when an element is saved.

#### Params:

- `element` – A <craft2:Craft\BaseElementModel> object representing the element that was just saved.
- `isNewElement` – A boolean indicating whether this is a brand new element.

### fields.onSaveFieldLayout

Raised by

:   <craft2:Craft\FieldsService::saveLayout()>

Raised when a field layout is saved.

#### Params:

- `layout` – A <craft2:Craft\FieldLayoutModel> object representing the field layout that was just saved.

## Entry API Events

### entries.onBeforeSaveEntry

Raised by

:   <craft2:Craft\EntriesService::saveEntry()>

Raised right before an entry is saved.

#### Params:

- `entry` – An <craft2:Craft\EntryModel> object representing the entry that is about to be saved.
- `isNewEntry` – A boolean indicating whether this is a brand new entry.

::: tip
Event handlers can prevent the entry from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### entries.onSaveEntry

Raised by

:   <craft2:Craft\EntriesService::saveEntry()>

Raised when an entry is saved.

#### Params:

- `entry` – An <craft2:Craft\EntryModel> object representing the entry that was just saved.
- `isNewEntry` – A boolean indicating whether this is a brand new entry.

### entries.onBeforeDeleteEntry

Raised by

:   <craft2:Craft\EntriesService::deleteEntry()>

Raised right before an entry is deleted.

#### Params:

- `entry` – An <craft2:Craft\EntryModel> object representing the entry that is about to be deleted.

::: tip
Event handlers can prevent the entry from being deleted by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### entries.onDeleteEntry

Raised by

:   <craft2:Craft\EntriesService::deleteEntry()>

Raised when an entry is deleted.

#### Params:

- `entry` – An <craft2:Craft\EntryModel> object representing the entry that was just deleted.

### entryRevisions.onSaveDraft

Raised by

:   <craft2:Craft\EntryRevisionsService::saveDraft()>

Raised right before a draft is saved.

#### Params:

- `draft` – An <craft2:Craft\EntryDraftModel> object representing the draft that was just saved.
- `isNewDraft` – A boolean indicating whether this is a brand new draft.

### entryRevisions.onPublishDraft

Raised by

:   <craft2:Craft\EntryRevisionsService::publishDraft()>

Raised when an draft is published.

#### Params:

- `draft` – An <craft2:Craft\EntryDraftModel> object representing the draft that was just published.

### entryRevisions.onBeforeDeleteDraft

Raised by

:   <craft2:Craft\EntryRevisionsService::deleteDraft()>

Raised right before a draft is deleted.

#### Params:

- `draft` – An <craft2:Craft\EntryDraftModel> object representing the draft that is able to be deleted.

::: tip
Event handlers can prevent the draft from getting deleted by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### entryRevisions.onDeleteDraft

Raised by

:   <craft2:Craft\EntryRevisionsService::deleteDraft()>

Raised right after a draft is deleted.

#### Params:

- `draft` – An <craft2:Craft\EntryDraftModel> object representing the draft that was just deleted.

### sections.onBeforeDeleteSection

Raised by

:   <craft2:Craft\SectionsService::deleteSectionById()>

Raised right before a section is deleted.

#### Params:

- `sectionId` – The ID of the section that is about to be deleted.

::: tip
Event handlers can prevent the section from being deleted by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### sections.onDeleteSection

Raised by

:   <craft2:Craft\SectionsService::deleteSectionById()>

Raised after a section is deleted.

#### Params:

- `sectionId` – The ID of the section that was just deleted.

### sections.onBeforeSaveEntryType

Raised by

:   <craft2:Craft\SectionsService::saveEntryType()>

Raised right before an entry type is saved.

#### Params:

- `entryType` – An <craft2:Craft\EntryTypeModel> object representing the entry type that is about to be saved.
- `isNewEntryType` – A boolean indicating whether this is a brand new entry type.

::: tip
Event handlers can prevent the entry type from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### sections.onSaveEntryType

Raised by

:   <craft2:Craft\SectionsService::saveEntryType()>

Raised when an entry type is saved.

#### Params:

- `entryType` – An <craft2:Craft\EntryTypeModel> object representing the entry type that was just saved.
- `isNewEntryType` – A boolean indicating whether this is a brand new entry type.

### sections.onBeforeSaveSection

Raised by

:   <craft2:Craft\SectionsService::saveSection()>

Raised right before a section is saved.

#### Params:

- `section` – An <craft2:Craft\SectionModel> object representing the section that is about to be saved.
- `isNewSection` – A boolean indicating whether this is a brand new section.

::: tip
Event handlers can prevent the section from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### sections.onSaveSection

Raised by

:   <craft2:Craft\SectionsService::saveSection()>

Raised when a section is saved.

#### Params:

- `section` – An <craft2:Craft\SectionModel> object representing the section that was just saved.
- `isNewSection` – A boolean indicating whether this is a brand new section.

## Category API Events

### categories.onBeforeSaveCategory

Raised by

:   <craft2:Craft\CategoriesService::saveCategory()>

Raised before any category is saved.

#### Params:

- `category` – A <craft2:Craft\CategoryModel> object representing the category about to be saved.
- `isNewCategory` – A boolean indicating whether this is a new category.

::: tip
Event handlers can prevent the category from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### categories.onSaveCategory

Raised by

:   <craft2:Craft\CategoriesService::saveCategory()>

Raised when any category is saved.

#### Params:

- `category` – A <craft2:Craft\CategoryModel> object representing the category being saved.
- `isNewCategory` – A boolean indicating whether this is a new category.

### categories.onBeforeDeleteCategory

Raised by

:   <craft2:Craft\CategoriesService::deleteCategory()>

Raised before any category is deleted.

#### Params:

- `category` – A <craft2:Craft\CategoryModel> object representing the category about to be deleted.

### categories.onDeleteCategory

Raised by

:   <craft2:Craft\CategoriesService::deleteCategory()>

Raised when any category is deleted.

#### Params:

- `category` – A <craft2:Craft\CategoryModel> object representing the category being deleted.

### categories.onBeforeDeleteGroup

Raised by

:   <craft2:Craft\CategoriesService::deleteGroupById()>

Raised before a category group is deleted.

#### Params:

- `groupId` – The ID of the category group that’s about to be deleted.

::: tip
Event handlers can prevent the category group from being deleted by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### categories.onDeleteGroup

Raised by

:   <craft2:Craft\CategoriesService::deleteGroupById()>

Raised after a category group is deleted.

#### Params:

- `groupId` – The ID of the category group that was just deleted.

## Tag API Events

### tags.onBeforeSaveTag

Raised by

:   <craft2:Craft\TagsService::saveTag()>

Raised when a tag is able to be saved.

#### Params:

- `tag` – A <craft2:Craft\TagModel> object representing the tag that is about to be saved.
- `isNewTag` – A boolean indicating whether this is a brand new tag.

::: tip
Event handlers can prevent the tag from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### tags.onSaveTag

Raised by

:   <craft2:Craft\TagsService::saveTag()>

Raised when a tag is saved.

#### Params:

- `tag` – A <craft2:Craft\TagModel> object representing the tag that was just saved.
- `isNewTag` – A boolean indicating whether this is a brand new tag.

## Asset API Events

### assets.onBeforeDeleteAsset

Raised by

:   <craft2:Craft\AssetsService::deleteFiles()>

Raised right before an asset is deleted.

#### Params:

- `asset` – An <craft2:Craft\AssetFileModel> object representing the asset about to be deleted.

### assets.onDeleteAsset

Raised by

:   <craft2:Craft\AssetsService::deleteFiles()>

Raised when an asset is deleted.

#### Params:

- `asset` – An <craft2:Craft\AssetFileModel> object representing the asset that was just deleted.

### assets.onBeforeReplaceFile

Raised by

:   <craft2:Craft\AssetsController::actionReplaceFile()>

Raised right before an asset’s file is replaced.

#### Params:

- `asset` – An <craft2:Craft\AssetFileModel> object representing the asset whose file is about to be replaced.

::: tip
Event handlers can prevent the file from getting replaced by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### assets.onReplaceFile

Raised by

:   <craft2:Craft\AssetsController::actionReplaceFile()>

Raised when any asset’s file is replaced.

#### Params:

- `asset` – An <craft2:Craft\AssetFileModel> object representing the asset whose file was just replaced.

### assets.onBeforeSaveAsset

Raised by

:   <craft2:Craft\AssetsService::storeFile()>

Raised right before an asset is saved.

#### Params:

- `asset` – An <craft2:Craft\AssetFileModel> object representing the asset about to be saved.
- `isNewAsset` – A boolean indicating whether this is a new asset.

::: tip
Event handlers can prevent the asset from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### assets.onSaveAsset

Raised by

:   <craft2:Craft\AssetsService::storeFile()>

Raised when any asset is saved.

#### Params:

- `asset` – An <craft2:Craft\AssetFileModel> object representing the asset being saved.
- `isNewAsset` – A boolean indicating whether this is a new asset.

### assets.onBeforeUploadAsset

Raised by

:   <craft2:Craft\BaseAssetSourceType::insertFileByPath()>

Raised right before an asset is uploaded to its source.

#### Params:

- `path` – The path to the temporary file on the server.
- `folder` – An <craft2:Craft\AssetFolderModel> object representing the asset folder that the file is going to be saved to.
- `filename` – The filename of the file.

::: tip
Event handlers can prevent the asset from getting uploaded by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

## Global Set API Events

### globals.onBeforeSaveGlobalContent

Raised by

:   <craft2:Craft\GlobalsService::saveContent()>

Raised right before a Global Set’s content is saved.

#### Params:

- `globalSet` – A <craft2:Craft\GlobalSetModel> object representing the Global Set whose content is about to be saved.

::: tip
Event handlers can prevent the global set from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### globals.onSaveGlobalContent

Raised by

:   <craft2:Craft\GlobalsService::saveContent()>

Raised when a Global Set’s content is saved.

#### Params:

- `globalSet` – A <craft2:Craft\GlobalSetModel> object representing the Global Set whose content was just saved.

## User API Events

### userSession.onBeforeLogin

Raised by

:   <craft2:Craft\UserSessionService::login()>

Raised right before a user is logged in.

#### Params:

- `username` – A string of the username that is about to log in.

::: tip
Event handlers can prevent the user from getting logged in by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### userSession.onLogin

Raised by

:   <craft2:Craft\UserSessionService::login()>

Raised when a user has logged in.

#### Params:

- `username` – A string of the username that has just logged in.

### userSession.onBeforeLogout

Raised by

:   <craft2:Craft\UserSessionService::beforeLogout()>

Raised right before a user is logged out.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that is getting logged out.

::: tip
Event handlers can prevent the user from getting logged out by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### userSession.onLogout

Raised by

:   <craft2:Craft\UserSessionService::afterLogout()>

Raised when a user is logged out.

### users.onBeforeActivateUser

Raised by

:   <craft2:Craft\UsersService::activateUser()>

Raised right before a user is activated.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that’s about to be activated.

::: tip
Event handlers can prevent the user from getting activated by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### users.onActivateUser

Raised by

:   <craft2:Craft\UsersService::activateUser()>

Raised when a user is activated.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that has just been activated.

### userGroups.onBeforeAssignUserToGroups

Raised by

:   <craft2:Craft\UserGroupsService::assignUserToGroups>

Raised right before a user’s group assignments are updated. Note that this could be called even if the group assignments haven’t changed.

#### Params:

- `userId` – The ID of the user whose group assignments are about to be updated.
- `groupIds` – The user’s new group IDs (if any).

::: tip
Event handlers can prevent the user’s new group assignments from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### userGroups.onAssignUserToGroups

Raised by

:   <craft2:Craft\UserGroupsService::assignUserToGroups>

Raised right after a user’s group assignments are updated.

#### Params:

- `userId` – The ID of the user whose group assignments were updated.
- `groupIds` – The user’s new group IDs (if any).

### users.onBeforeDeleteUser

Raised by

:   <craft2:Craft\UsersService::deleteUser()>

Raised right before a user is deleted.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that’s about to be deleted.
- `transferContentTo` – A <craft2:Craft\UserModel> object representing the user that the deleted user’s content should be transferred to, if any.

::: tip
Event handlers can prevent the user from getting deleted by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### users.onDeleteUser

Raised by

:   <craft2:Craft\UsersService::deleteUser()>

Raised when a user is deleted.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that was just deleted.
- `transferContentTo` – A <craft2:Craft\UserModel> object representing the user that the deleted user’s content should have been transferred to, if any.

### users.onBeforeSaveUser

Raised by

:   <craft2:Craft\UsersService::saveUser()>

Raised right before a user is saved.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that’s about to be saved.
- `isNewUser` – A boolean indicating whether this is a brand new user account.

::: tip
Event handlers can prevent the user from getting saved by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### users.onSaveUser

Raised by

:   <craft2:Craft\UsersService::saveUser()>

Raised when a user is saved.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that was just saved.
- `isNewUser` – A boolean indicating whether this is a brand new user account.

### users.onBeforeSetPassword

Raised by

:   <craft2:Craft\UsersService::saveUser()>, <craft2:Craft\UsersService::changePassword()>

Raised right before a user’s password is changed.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user whose password is about to be changed.
- **password** – The user’s new password.

::: tip
Event handlers can prevent the user’s password from getting changed by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### users.onSetPassword

Raised by

:   <craft2:Craft\UsersService::saveUser()>, <craft2:Craft\UsersService::changePassword()>

Raised when a user’s password is changed.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user whose password was changed.

### users.onBeforeSuspendUser

Raised by

:   <craft2:Craft\UsersService::suspendUser()>

Raised right before a user is suspended.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that’s about to be suspended.

::: tip
Event handlers can prevent the user from getting suspended by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### users.onSuspendUser

Raised by

:   <craft2:Craft\UsersService::suspendUser()>

Raised when a user is suspended.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that was just suspended.

### users.onLockUser

Raised by

:   <craft2:Craft\UsersService::lockUser()>

Raised when a user is locked.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that was just locked.

### users.onBeforeUnlockUser

Raised by

:   <craft2:Craft\UsersService::unlockUser()>

Raised right before a user is unlocked.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that’s about to be unlocked.

::: tip
Event handlers can prevent the user from getting unlocked by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### users.onUnlockUser

Raised by

:   <craft2:Craft\UsersService::unlockUser()>

Raised when a user is unlocked.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that was just unlocked.

### users.onBeforeUnsuspendUser

Raised by

:   <craft2:Craft\UsersService::unsuspendUser()>

Raised right before a user is unsuspended.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that’s about to be unsuspended.

::: tip
Event handlers can prevent the user from getting unsuspended by setting [$event->performAction](craft2:Craft\Event::performAction) to `false`.
:::

### users.onUnsuspendUser

Raised by

:   <craft2:Craft\UsersService::unsuspendUser()>

Raised when a user is unsuspended.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that was just unsuspended.

### users.onBeforeVerifyUser

Raised by

:   <craft2:Craft\UsersController::actionSetPassword()>, <craft2:Craft\UsersController::actionVerifyEmail()>

Raised right before a user’s email is verified.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that’s about to be verified.

### users.onVerifyUser

Raised by

:   <craft2:Craft\UsersController::actionSetPassword()>, <craft2:Craft\UsersController::actionVerifyEmail()>

Raised when a user’s email is verified.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that was just verified.

### userGroups.onBeforeAssignUserToDefaultGroup

Raised by

:   <craft2:Craft\UserGroupsService::assignUserToDefaultGroup()>

Raised towards the end of a public user registration request before a user is assigned to a default user group.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that was just registered.
- `defaultGroupId` – The id of the user group that the user is about to be assigned to.

### userGroups.onAssignUserToDefaultGroup

Raised by

:   <craft2:Craft\UserGroupsService::assignUserToDefaultGroup()>

Raised towards the end of a public user registration request after a user is assigned to a default user group.

#### Params:

- `user` – A <craft2:Craft\UserModel> object representing the user that was just registered.
- `defaultGroupId` – The id of the user group that the user was just assigned to.
