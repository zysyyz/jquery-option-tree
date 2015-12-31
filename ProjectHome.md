This jquery plugin converts passed JSON option tree into dynamically created `<select>` elements allowing you to choose one nested option from the tree.

It should be attached to a (most likely hidden) `<input>` element. It requires an option tree object. Object property names become labels of created select elements, each non-leaf node in the tree contains other nodes. Leaf nodes contain one value - it will be inserted into attached `<input>` element when chosen.

The plugin also supports loading additional levels via AJAX calls and preselecting given items at load.

# Usage #

```

var option_tree = {
   "Option 1": {"Suboption":200},
   "Option 2": {"Suboption 2": {"Subsub 1":201, "Subsub 2":202},
                "Suboption 3": {"Subsub 3":203, "Subsub 4":204, "Subsub 5":205}
               }
};

$('input[type=hidden].nested').optionTree(option_tree);

```

# Effect #
It will display a select element with three options: "Choose...", "Option 1" and "Option 2".
When client chooses one of the options from the list (e.g. "Option 1"), another select is displayed below with "Choose" and 2nd level options ("Suboption 2", "Suboption 3").
If there are other suboptions of 3rd, 4th etc. level, the process continues.

When client finally chooses a leaf option ("Suboption", "Subsub X" in the example), its value (represented by numbers here) is assigned to selected `<input>` element and its `change()` event is triggered.

# Configuration #
Plugin accepts config object as 2nd parameter. Config object may have the following properties (default values are listed below):
```
var config = { choose: 'Choose...',   // default label of the first (empty) option of each level. If empty will not add the empty option
               show_multiple: false,  // show multiple values in select (true - display all items, number (eg. 12) to show fixed number of items)
               preselect: {},         // values of options that will be automatically selected instead of 'Choose' option
               loading_image: '',     // ajax loading graphics URL to show while loading options via ajax (eg. /ajax-loader.gif)
               select_class: '',      // HTML class of created select elements 
               leaf_class: 'final',   // HTML class of leaf node <option> element
               empty_value: ''        // this value will be inserted int <input> element if client reverts from choosing an option by going up in the tree
               on_each_change: false, // URL to lazy load (JSON, 'id' parameter will be added) or function. See deafult_lazy_load
               set_value_on: 'leaf',  // leaf - sets input value only when choosing leaf node. 'each' - sets value on each level change.
                                      // makes sense only then indexed=true
               indexed: false,        // tree is indexed by option label (false) or option value (true) - see below
              };

$('input.nested').optionTree(option_tree, config);
```

To easy understand indexed=false/true option, see the examples of trees for both:
```

// Option LABELS are object property names. Property values are either a scalar value or an object with a subtree
// The whole tree can be loaded at once
var indexed_false = { 
   "Option 1": {"Suboption": 200},
   "Option 2": {"Suboption 2": {"Subsub 1":201, "Subsub 2":202},
                "Suboption 3": {"Subsub 3":203, "Subsub 4":204, "Subsub 5":205}
               }
};

// Option VALUES are property names. Property values become option names. 
// Additional levels have be loaded dynamically using 'on_each_change' option.
var indexed_true = {
   1: "Option 1",
   2: "Option 2" 
};

```

# Demo #
See a [short demonstration](http://kotowicz.net/jquery-option-tree/demo/demo.html) of plugin capabilities and [plugin posts](http://blog.kotowicz.net/search/label/option) on my blog.