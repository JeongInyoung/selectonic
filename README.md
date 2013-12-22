# MultiSelectable
jQuery-plugin for making any list of items selectable by mouse and keyboard.   It could be usefull in webapp where are different widgets like menus, dropdowns with keyboard input, lists with multiple selection and so on. It maybe too bold for just a simple menu in one place.

See examples on [demo page](http://anovi.github.io/multiSelectable/ "MultiSelectable — jQuery plugin").

Author: Alexey Novichkov  
Version: 0.2.0

## Usage
```javascript
$(".itemsList").multiSelectable({
	multi: true,
    mouseMode: "select",
    keyboard: true,
    select: function(event, ui) {
        // do something cool, for expample enable action buttons
    }
	unselectAll: function(event, ui) {
        // …and disable action buttons
    }
});
```

## Options

Option | Description | Type | Default
--- | --- | --- | ---
**multi** | User can select many items in a list. | Boolean | true
**focusBlur** | List loses focus when user clicks outside of the list. | Boolean | false
**selectionBlur** | List clears selection when user clicks outside of the list. | Boolean | false
**keyboard** | Possibility to use keys **Up**, **Down** and **Home**, **End** to move cursor (focus), select range of items by holding **shift** and select all items by pressing **ctrl+A**. | Boolean | false
**autoScroll** | Scrollable element. When user moves cursor by keys **Up**, **Down**, **Home**, **End** – plugin calculate elements's scroll position in a way that focused item is always visible. Accept values: <br>`true` – list's element is scrollable; <br>`false` – no scrollable element; <br>`selector (String)` – custom element that is not list's element. | Boolean or String | true
**loop** | If `keyboard: true` and focused element is last in a list, pressing key **Down** set focus to first element of the list; oppositely with **Up** key, when focus on a first element. | Boolean | false
**preventInputs** | Prevent any reactions on keyboard input when focus is on a text input or textarea.  | Boolean | true |
**handle** | Matches child elemets, that will be served as clickable area inside each item to select/unselect these items. | String | null
**mouseMode** | `select` – mouse click selects target item and clear other selection in a list; to add/remove item to selection user need holding **ctrl** key (file manager style);<br>`toggle` – mouse click for select/unselect item (like group of checkboxes); | String | "select"
**event** | `mousedown` – item will be selected when user pushes on left mouse button. <br>`click` – item will be selected when user clicks (press and release) on left mouse button. <br>`hybrid` – unselected items will react on mousedown, but selected items will react on click (press and release). It can be usefull when there is another plugin, for example to dragging selected items. | string | "mousedown"
**filter** | Matches child elements, that can be selected. | String | "* >"
**listClass** | HTML class for selectable list. | String | "j-selectable"
**focusClass** | HTML class for focused element. | String | "j-focused"
**selectedClass** | HTML class for selected element. | String | "j-selected"
**disabledClass** | HTML class for selectable list which is disabled. | String | "j-disabled"
**before**, **select**, **unselect**, **unselectAll**, **focusLost**, **stop**, **create**, **destroy** | Callback functions, that described in the next section. | Function | null


## Callbacks (Events)
Callback functions except `create` and `destroy` may get arguments:
* `event` — event object from mouse or keyboard handlers.
* `ui` — object that contains properties with HTML elements:
	* `target` — link to targeted element, that has clicked or chosen by keyboard.
	* `focus` — link to focused element.
	* `items` — jquery collection of changed items – it could differ from event to event. Look at a table below.

Callback functions and arguments that they receive:
<table>
	<thead>
		<tr>
			<td>
				Callback
			</td>
			<td>event</td>
			<td>ui.target</td>
			<td>ui.focus</td>
			<td>ui.items</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
				<strong>create</strong> — calls after initialization.
			</td>
			<td>—</td>
			<td>—</td>
			<td>—</td>
			<td>—</td>
		</tr>
		<tr>
			<td>
				<strong>before</strong> — calls in every work cycle before any changes.
			</td>
			<td>event</td>
			<td>if exist</td>
			<td>if exist</td>
			<td>—</td>
		</tr>
		<tr>
			<td>
				<strong>select</strong> — when one or more elements has been selected.
			</td>
			<td>event</td>
			<td>Yes</td>
			<td>Yes</td>
			<td>jquery collection of selected elements</td>
		</tr>
		<tr>
			<td>
				<strong>unselect</strong> — one or more elements has been unselected.
			</td>
			<td>event</td>
			<td>if exist</td>
			<td>if exist</td>
			<td>jquery collection of unselected elements</td>
		</tr>
		<tr>
			<td>
				<strong>unselectAll</strong> — when all elements in the list has been unselected.
			</td>
			<td>event</td>
			<td>if exist</td>
			<td>if exist</td>
			<td>jquery collection of unselected elements</td>
		</tr>
		<tr>
			<td>
				<strong>focusLost</strong> — when the focus of the list is lost.
			</td>
			<td>event</td>
			<td>—</td>
			<td>Link on focused element. Link will be removed after this callback.</td>
			<td>—</td>
		</tr>
		<tr>
			<td>
				<strong>stop</strong> — calls in the end of every work cycle.
			</td>
			<td>event</td>
			<td>if exist</td>
			<td>if exist</td>
			<td>all changed items (if they were)</td>
		</tr>
		<tr>
			<td>
				<strong>destroy</strong> — calls before plugin's destroy.
			</td>
			<td>—</td>
			<td>—</td>
			<td>—</td>
			<td>—</td>
		</tr>
	</tbody>
</table>


Link `this` inside callbacks refer to list's initial element wrapped in jQuery, so you may call any method inside callbacks functions like `this.multiSelectable("disable");`.


## Methods
Calling a method:
```javascript
$(elem).multiSelectable("getSelected");
```


### isEnabled
Returns `true` if list is in enabled state.

### blur
Clears selection and focus in a list.

### getSelected
Get jQuery collection of selected elements.

### getSelectedId
Get array of id's of selected elements.

### getFocused
Get focused element or `null` if there is no one.

### disable
Disable selectable list, but keep data and save it state.

### enable
Turn on disabled plugin.

### refresh
Refresh plugin data. That is necessary when some items in a list has been removed.

### option
Get or set one or more options.

**Getter**:
```javascript
$(elem).multiSelectable("option", "mouseMode");
```
…or you can get whole options object:
```javascript
$(elem).multiSelectable("option");
```
**Setter**:
```javascript
$(elem).multiSelectable("option", "mouseMode", "toggle");
```
or
```javascript
$(elem).multiSelectable("option", {
    loop: true,
    mouseMode: "select"
});
```

### cancel
Cancel current changes in a list or prevent it, if called in *before* callback. Method can be called only inside callback functions:
```javascript
elem.multiSelectable({
    stop: function(event, ui) {
        if( condition ) {
            this.multiSelectable("cancel"); // inside callback
        }
    }
});
```

### destroy
Detach handlers, clear data and remove plugin's classes.

### Selecting items by API
If in any scenario you need auto-select items (focusing first item at the beginning) then you need pass elements to plugins interface:
```javascript
$(elem).multiSelectable( $(".someElements")  );
```
or selector:
```javascript
$(elem).multiSelectable(":even");
```
```javascript
$(elem).multiSelectable("li:eq(3)");
```


## Known issues
There is a problem with calculation of scroll position when lists container has a scrollbar and located inside (primary parent) element with `display: table-row;`.

## Compatibility
Requires jQuery 1.7+  
Tested in Firefox, Chrome, Safari, Opera, IE8+
