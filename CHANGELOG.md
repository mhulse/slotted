# Changelog

## v3.0.0
#### January 7, 2013

* Bumped version number to `v3.0.0`.
* `custom.rg.Slotted.cls`:
	* Changed what the class extends from ` (%Persistent, %Populate, %XML.Adaptor)` to `%RegisteredObject`.
		* Changed `extends` due to [info discussed here](https://groups.google.com/d/topic/dti-lightning/T3WcNsxY5A8/discussion).
	* Changed return type of class to `%ListOfObjects`.
	* Minor updates to comments.
	* Updated error message.
	* Fixed syntax error in copy/paste SQL comment.
* `custom.rg.GetStoriesRule.csr`:
	* Updated comments.
	* Removed unused line of code.
	* Removed `kill`s, because one was not even getting called and the other was removed for consistency's sake (the `new`s should do the job anyway).
	* Small formatting tweaks.
* `test.csp`:
	* Added closing slash to `<csp:object />` tags.

---

## v2.0.0
#### November 18, 2012

* Changed `stories()` query to return `Mapping` objects.
	* Usefull `Properties`, `Methods` and `ClassMethods`:
		1. `Property version As %Integer` (Staging Version.)
		2. `Property importTime As %TimeStamp` (When this `Mapping` was last inserted by the `Auto Populate` process.)		3. `Property localFullLayoutID As dt.cms.schema.LayoutFull` (The slotted `CMSStory`'s `FullLayout` object.)
		4. `Property localSummaryLayoutID As dt.cms.schema.LayoutSummary` (The slotted `CMSStory`'s `SummaryLayout` object.)
		5. `Relationship cmsStory As dt.cms.schema.CMSStory` (The slotted `CMSStory` object.)
		6. `Relationship slotReferenceID As dt.cms.schema.SlotReference` (Which `Slot` this `CMSStory` is being used in; can be `NULL` if the `CMSStory` is not assigned.)
		7. `Relationship sectionID As dt.cms.schema.Section` (Which `Section` this story belongs to; can be `NULL`, indicating that this `CMSStory` is not currently assigned to any `Section`.)
		8. `Relationship mappings As dt.cms.schema.Workspace` (Link to `CMSStory`s asssigned to this users workspace.)
		9. `Relationship ghostMappings As dt.cms.schema.GhostMapping` (Link to `ghostMappings` (used for 'ghost') `Sections`.)
		10. `Method getLink(sectionObject As dt.cms.schema.Section, fullLayoutObject As dt.cms.schema.LayoutFull) As %String` (Builds a link to this mapping/story.)
		11. `Method getCMSStory() As dt.cms.schema.CMSStory` (A `CMSStory` getter.)
		12. `Method getRank() As %Float` (Returns the `CMSStory`'s ranking.)
		13. `Method getStatus() As dt.cms.schema.Status` (Returns the `CMSStory`'s status object and will never return `NULL`.)
		14. `Method getOriginalStoryId() As %Integer` (This is the `%Id()` value of `dbo.story`.)
		15. `ClassMethod getMappingIDList(sectionName As %String, areaName As %String, publicationID As %String) As %ListOfDataTypes` (Returns a list of `MappingId`'s from a specific `Area` within a specific `Section` in a specific `Publication`.)
* Updated the code comments, demos and documention to reflect the above change.

---

## v1.0.1
#### November 12, 2012

* Bumped version number: `v1.0.1`.
* Cast the `version` parameter in ClassMethod to `%String`.
	* As an integer, the `$$$lister()` macro was failing.
* Added a default value of "0" for the `version` attribute in the RULE.
* Modified/added some comments to both ClassMethod and RULE.
* Updated `test.csp` to show how to call the ClassMethod directly.
* Updated `README.md`:
	* Updated docs.
	* Added "installation" section.
* Fixed link to GitHub repo in RULE.

---

## v1.0.0
#### November 9, 2012

* Initial public release on GitHub.

---

## vX.X.X
#### Mmmmm [D]D, YYYY

* ...

---