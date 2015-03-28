# Slotted

#### Get slotted stories (from the `dt.cms.schema.Mapping` object) using `<custom:rg:get:stories>` tag and/or `##class(custom.rg.GetStoriesRule).stories()` ClassMethod.

Tested and optimized for use on DTI's [ContentPublisher](http://www.dtint.com/our-solutions/content-publisher/) `v7.7.3` system running [Caché](http://www.intersystems.com/cache/index.html) for Windows (`x86-64`) `2009.1.5` (Build `901_0_11112U`).

Based on DTI's `dt.cms.support.TopSlotStories`.

**Many thanks goes to Joy, Eric and others at [DTI](http://www.dtint.com/) for the [pro help and inspiration](https://groups.google.com/forum/#!topic/dti-lightning/5avEj-4twNE/discussion).**

---

### FEATURES

* `Mapping` object is returned for easy access to more than just the `CMSStory` object.
* One, or multiple, `Area`s (from a single `Section`).
* `Slot`(s) inclusion.
* `Slot`(s) exclusion.
* Ability to look at one, or multiple, `Staging Versions`.
* A counter while iterating over `Mapping` objects.
* A "`total`" number of returned `Mapping` objects (so you can check for "first" or "last" items).
* Customizable `ORDER BY` (useful when looking at multiple `Areas` and/or `Staging Versions`).
* Applicable attribute have been setup to handle `#()#` runtime expressions as values.
* Optimized/optimal garbage collection of local variables.
* Defauts to `gPublication.getName()` if a `Publication` name is not defined.
* Defaults to `gSection.getName()` if a `Section` name is not defined.
* Lots more DTI-approved error handling goodies.
* All comma delimited strings are trimmed before being converted into a `$list` for use with the `SQL` statement; no more white space woes! **Woot!** :D
* Based on the awesome feedback from **Joy** and **Eric**, I've updated the `query` to use `%ResultSet.SQL` with smart parameter handling (thanks guys!!!!).

---

#### DETAILS

## `<custom:rg:get:stories>` tag

### `custom.rg.GetStoriesRule` attributes:

1. `publication`: `Publication` name; `gPublication.GetName()` used by default.
2. `section`: `Section` name; `gSection.GetName()` used by default.
3. `layout`: `Page Layout` name (required).
4. `grid`: `Grid` name (required).
5. `area`: `Area`(s) name (required).
6. `items`: Number of `Mapping` objects to return; the default number is '`5`'.
7. `include`: Include only these `Slots` (comma delimited list).
8. `exclude`: Exclude only these `Slots` (comma delimited list).
9. `version`: `Staging Version`(s) to pull from; '`0`' (live) used by default.
10. `order`: Custom `ORDER BY`, used in `SQL` statement.
11. `direction`: Direction of iteration; either forward (default) or backward.
12. `count`: Loop counter name; the default name is '`count`'.
13. `value`: `Mapping` object name; the default name is '`value`'.
14. `obj`: Local `%ListOfObjects` object variable name; the default name is '`obj`'.
15. `total`: Total number of `Mapping` objects returned.

### Example call:

```
<ol>
	
	 <custom:rg:get:stories layout="sports" grid="Default" area="Top Stories, Stories" items="10" include="" exclude="" version="0" order="" direction="forward" count="count" value="value" obj="obj" total="total">
		
		#[ new story set story = value.getCMSStory() ]#
		
		<li>
			<b>Value:</b> #(value)#
			<ul>
				<li><b>CMSStory</b> #(story)#</li>
				<li><b>Slug:</b> <span style="color:blue">#(story.getName())#</span></li>
				<li><b>publishedToWebDate:</b> #(story.publishedToWebDate)#</li>
				<li>
					<b>FullLayout:</b> #(value.localFullLayoutID)#
					<ul>

						<li><b>FullLayout name:</b> #(value.localFullLayoutID.name)#</li>
					</ul>
				</li>
				<li><b>Total:</b> #(total)#</li>
				<li><b>Count:</b> #(count)#</li>
				<csp:if condition=(count=1)>
					<li><b style="color:red">First</b></li>
				</csp:if>
				<csp:if condition=(count=total)>
					<li><b style="color:red">Last</b></li>
				</csp:if>
				<li><b>Obj:</b> #(obj)#</li>
			</ul>
		</li>
		
		#[ kill story ]#
		
	</custom:rg:get:stories>
	
</ol>
```

### `html` output:

> <h1>sports</h1>
> 
> <ol>
> 	<li>
> 		<b>Value:</b> 13@dt.cms.schema.Mapping
> 		<ul>
> 			<li><b>CMSStory</b> 12@dt.cms.schema.CMSStory</li>
> 			<li><b>Slug:</b> <span style="color:blue">cd.test01.1030</span></li>
> 			<li><b>publishedToWebDate:</b> 2012-11-16 17:39:29</li>
> 			<li>
> 				<b>FullLayout:</b> 23@dt.cms.schema.LayoutFull
> 				<ul>
> 					<li><b>FullLayout name:</b> Default</li>
> 				</ul>
> 			</li>
> 			<li><b>Total:</b> 8</li>
> 			<li><b>Count:</b> 1</li>
> 			<li><b style="color:red">First</b></li>
> 			<li><b>Obj:</b> 11@%Library.ListOfObjects</li>
> 		</ul>
> 	</li>
> 	<li>
> 		<b>Value:</b> 14@dt.cms.schema.Mapping
> 		<ul>
> 			<li><b>CMSStory</b> 24@dt.cms.schema.CMSStory</li>
> 			<li><b>Slug:</b> <span style="color:blue">c1.sp.uofootball.0923</span></li>
> 			<li><b>publishedToWebDate:</b> 2012-10-30 11:57:07</li>
> 			<li>
> 				<b>FullLayout:</b> 23@dt.cms.schema.LayoutFull
> 				<ul>
> 					<li><b>FullLayout name:</b> Default</li>
> 				</ul>
> 			</li>
> 			<li><b>Total:</b> 8</li>
> 			<li><b>Count:</b> 2</li>
> 			<li><b>Obj:</b> 11@%Library.ListOfObjects</li>
> 		</ul>
> 	</li>
> 	<li>
> 		<b>Value:</b> 15@dt.cms.schema.Mapping
> 		<ul>
> 			<li><b>CMSStory</b> 27@dt.cms.schema.CMSStory</li>
> 			<li><b>Slug:</b> <span style="color:blue">c1.sp.osufootball.0923</span></li>
> 			<li><b>publishedToWebDate:</b> 2012-11-16 17:44:58</li>
> 			<li>
> 				<b>FullLayout:</b> 30@dt.cms.schema.LayoutFull
> 				<ul>
> 					<li><b>FullLayout name:</b> Test</li>
> 				</ul>
> 			</li>
> 			<li><b>Total:</b> 8</li>
> 			<li><b>Count:</b> 3</li>
> 			<li><b>Obj:</b> 11@%Library.ListOfObjects</li>
> 		</ul>
> 	</li>
> 	<li>
> 		<b>Value:</b> 16@dt.cms.schema.Mapping
> 		<ul>
> 			<li><b>CMSStory</b> 31@dt.cms.schema.CMSStory</li>
> 			<li><b>Slug:</b> <span style="color:blue">c2.sp.regional.0923</span></li>
> 			<li><b>publishedToWebDate:</b> 2012-09-28 16:55:27</li>
> 			<li>
> 				<b>FullLayout:</b> 23@dt.cms.schema.LayoutFull
> 				<ul>
> 					<li><b>FullLayout name:</b> Default</li>
> 				</ul>
> 			</li>
> 			<li><b>Total:</b> 8</li>
> 			<li><b>Count:</b> 4</li>
> 			<li><b>Obj:</b> 11@%Library.ListOfObjects</li>
> 		</ul>
> 	</li>
> 	<li>
> 		<b>Value:</b> 17@dt.cms.schema.Mapping
> 		<ul>
> 			<li><b>CMSStory</b> 34@dt.cms.schema.CMSStory</li>
> 			<li><b>Slug:</b> <span style="color:blue">c3.sp.preps.0923</span></li>
> 			<li><b>publishedToWebDate:</b> 2012-09-28 16:55:27</li>
> 			<li>
> 				<b>FullLayout:</b> 23@dt.cms.schema.LayoutFull
> 				<ul>
> 					<li><b>FullLayout name:</b> Default</li>
> 				</ul>
> 			</li>
> 			<li><b>Total:</b> 8</li>
> 			<li><b>Count:</b> 5</li>
> 			<li><b>Obj:</b> 11@%Library.ListOfObjects</li>
> 		</ul>
> 	</li>
> 	<li>
> 		<b>Value:</b> 18@dt.cms.schema.Mapping
> 		<ul>
> 			<li><b>CMSStory</b> 37@dt.cms.schema.CMSStory</li>
> 			<li><b>Slug:</b> <span style="color:blue">c3.sp.timbers.0923</span></li>
> 			<li><b>publishedToWebDate:</b> 2012-09-28 16:55:27</li>
> 			<li>
> 				<b>FullLayout:</b> 23@dt.cms.schema.LayoutFull
> 				<ul>
> 					<li><b>FullLayout name:</b> Default</li>
> 				</ul>
> 			</li>
> 			<li><b>Total:</b> 8</li>
> 			<li><b>Count:</b> 6</li>
> 			<li><b>Obj:</b> 11@%Library.ListOfObjects</li>
> 		</ul>
> 	</li>
> 	<li>
> 		<b>Value:</b> 19@dt.cms.schema.Mapping
> 		<ul>
> 			<li><b>CMSStory</b> 40@dt.cms.schema.CMSStory</li>
> 			<li><b>Slug:</b> <span style="color:blue">c5.sp.fbc_smalls.0923</span></li>
> 			<li><b>publishedToWebDate:</b> 2012-09-28 16:55:27</li>
> 			<li>
> 				<b>FullLayout:</b> 23@dt.cms.schema.LayoutFull
> 				<ul>
> 					<li><b>FullLayout name:</b> Default</li>
> 				</ul>
> 			</li>
> 			<li><b>Total:</b> 8</li>
> 			<li><b>Count:</b> 7</li>
> 			<li><b>Obj:</b> 11@%Library.ListOfObjects</li>
> 		</ul>
> 	</li>
> 	<li>
> 		<b>Value:</b> 20@dt.cms.schema.Mapping
> 		<ul>
> 			<li><b>CMSStory</b> 43@dt.cms.schema.CMSStory</li>
> 			<li><b>Slug:</b> <span style="color:blue">c6.sp.fbc-pac12.0923</span></li>
> 			<li><b>publishedToWebDate:</b> 2012-09-28 16:55:27</li>
> 			<li>
> 				<b>FullLayout:</b> 23@dt.cms.schema.LayoutFull
> 				<ul>
> 					<li><b>FullLayout name:</b> Default</li>
> 				</ul>
> 			</li>
> 			<li><b>Total:</b> 8</li>
> 			<li><b>Count:</b> 8</li>
> 			<li><b style="color:red">Last</b></li>
> 			<li><b>Obj:</b> 11@%Library.ListOfObjects</li>
> 		</ul>
> 	</li>
> </ol>

---

## `##class(custom.rg.GetStoriesRule).stories()` ClassMethod

### `##class(custom.rg.GetStoriesRule).stories()` parameters:

1. `publication`: `Publication` name (required).
2. `section`: `Section` name (required).
3. `layout`: `Page Layout` name (required).
4. `grid`: `Grid` name (required).
5. `area`: `Area`(s) name (required).
6. `items`: Number of `Mapping` objects to return; the default number is '`5`'.
7. `include`: Include only these `Slots` (comma delimited list).
8. `exclude`: Exclude only these `Slots` (comma delimited list).
9. `version`: `Staging Version`(s) to pull from; '0' (live) used by default.
10. `order`: Custom `ORDER BY`, used in `SQL` statement.

### `COS` example call:

```
#[ new slotted set slotted = ##class(custom.rg.Slotted).stories("rg", "sports", "sports", "Default", "Top Stories, Stories", 10) ]#

<ol>
	<csp:loop counter="x" from="1" to="#(slotted.Count())#">
		<li>#(slotted.GetAt(x))#</li>
	</csp:loop>
</ol>

#[ kill slotted ]#
```

### `html` output:

> <ol>
> 	<li>13@dt.cms.schema.Mapping</li>
> 	<li>14@dt.cms.schema.Mapping</li>
> 	<li>15@dt.cms.schema.Mapping</li>
> 	<li>16@dt.cms.schema.Mapping</li>
> 	<li>17@dt.cms.schema.Mapping</li>
> 	<li>18@dt.cms.schema.Mapping</li>
> 	<li>19@dt.cms.schema.Mapping</li>
> 	<li>20@dt.cms.schema.Mapping</li>
> </ol>

---

#### DEMO

Check out [`slotted/test.csp`](https://github.com/mhulse/slotted/blob/master/slotted/test.csp) for a full code example.

---

#### TIP(S)

Dynamically get the **current** `Page` (`dt.cms.schema.Page`):

```
set page = ##class(dt.cms.support.Utilities).getCurrentPage(gSection)
```

Dynamically get the **current** `Page Layout` (`dt.cms.schema.PageLayout`):

```
set pagelayout = ##class(dt.cms.support.Utilities).getCurrentPage(gSection).pageLayoutID
```

Dynamically get the **current** `Grid` (`dt.cms.schema.Grid`):

```
set grid = ##class(dt.cms.support.Utilities).getCurrentGrid(gSection)
```

The above methods are useful if you're not sure what `PageLayout` and/or `Grid` a `Section` will be using.

Example:

```
<custom:rg:get:stories layout="#(##class(dt.cms.support.Utilities).getCurrentPage(gSection).pageLayoutID.name)#" grid="#(##class(dt.cms.support.Utilities).getCurrentGrid(gSection).name)#" area="Updates" items="10">
	
	<dti:story:use storyobj="#(value.getCMSStory())#">
		
		<dti:story:element:exist field="Headline">
			<h1><dti:story:link><dti:story:element field="Headline" extract="textonly" /></dti:story:link></h1>
		</dti:story:element:exist>
		
	</dti:story:use>
	
</custom:rg:get:stories>
```

Ahhh, that's much better than having to hard code the attribute values for `layout` and `grid`. :)

---

#### INSTALLATION

There's a couple ways (that I can think of) to install this code:

### Copy/paste:

1. Open Studio.
2. Change to the `CMS` namespace.
3. "File" >> "New..." and choose "Caché Class Definition" from "General" tab.
4. Copy/paste the **RAW** contents `custom.rg.Slotted.cls` into this new file.
5. Save this file as `custom.rg.Slotted.cls` to your `custom` package, in a sub package called `rg`.
6. Compile.
7. "File" >> "New..." and choose "Caché Server Page" from "CSP File" tab.
8. Copy/paste the **RAW** contents of `custom.rg.GetStoriesRule.csr` into this new file.
9. Save this file as `custom.rg.GetStoriesRule.csr` to the "CSP Files" >> `/csp/cms/customrules` package/folder/location.
10. Compile.

### Import local:

1. Download and unzip this repo to your local machine.
2. Open Studio.
3. Change to the `CMS` namespace.
4. "Tools" >> "Import Local...".
5. Import `custom.rg.Slotted.xml`, `custom.rg.GetStoriesRule.csr` and check the compile box.

---

#### LEGAL

Copyright © 2012 [Micky Hulse](http://mky.io)

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this work except in compliance with the License. You may obtain a copy of the License in the LICENSE file, or at:

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
