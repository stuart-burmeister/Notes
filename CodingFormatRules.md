# Style Rules

## File structure
* Each component should have it own file and directory
* The directory and file names should be the same as the component name
* Sub components are also managed as above (check for examples)
* All components should have an index.js file exporting the component
* All the components should also be exported from the components/index.js file as well.

## Props & Creating components
* All components start with props
* Decompose the props into local variables for use (ie no props.variable)
* Do not specify props if none are used.

## Exporting
* Export at the end of the file, not at the function definition.

## Dialog
* Implement separate from the components
* Use callbacks rather than handlers for passing functionality.
* Use the basic structure of Title, Content, then Action.

## Layout/Screen Drawing
* Use Box and grid for layouts.
* When applicable, start the component with Box
* Use the Grid container and Grid item to divide the screen repeatedly.

## CSS
* Use material-ui styles as well as Hook API
* Do not use inline styling.
* For merging styles use clsx
* The first element of the component should be labelled as the 'root'
* Commonly used style (for merging) is written above the route.
* Declare any variables used in styling directly above useStyles.
* Name each style in the same order at the code, and with combinations of '__' (2 underlines) and words (lowercase)

## Common Loading Screen
* 
