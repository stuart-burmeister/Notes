# Work Resources

## Apollo
* Used for data management and storage.
* [Client-side reference](https://www.apollographql.com/docs/react/)
* [Full-stack tutorial](https://www.apollographql.com/docs/tutorial/introduction/)

## Material UI
* [Docs & API](https://material-ui.com/components/buttons/)
* [Styling blog post](https://codeburst.io/my-journey-to-make-styling-with-material-ui-right-6a44f7c68113)
* [Using themes/ overrides](https://blog.bitsrc.io/how-to-customize-material-ui-theme-v3-2-0-part-3-750db6981a33)
  - Use the overrides of the theme when trying to theme complex components.
  - Works in both jss syntax and css syntax.
  - Refer to the api to find the components to style (ie. root, input, etc.)
* [Use with storybook](https://medium.com/encode/setting-up-storybook-with-material-ui-and-styled-components-5bdacb6db866)

## Local/Session Storage
* Can store variables in the local/session storage.
* [Explanation blog post](https://www.robinwieruch.de/local-storage-react)

## CSS/JSS
* Pixels can be fractions, which sometimes results in small artifacts where containers use different pixel values.
  * reqiures further investigation...
* For scrolling, on div elements the scroll bar will squish the elements, while for table elements it won't. However, table elements are harder to deal with.
  * solution: detect the scrollbar and then add the padding.
  * [Explanation of how to detect](https://medium.com/@jbbpatel94/difference-between-offsetheight-clientheight-and-scrollheight-cfea5c196937)
* Trying to get elements to fit designated widths/heights can be time-consuming and frustrating.
  * Can be solved through [Box sizing!!!](https://www.w3schools.com/css/css3_box-sizing.asp)
  * This will incorporate the entire box model (border, padding, margin, content) into the sizing.
  * For more references on the box model, check out [here!](https://www.w3schools.com/css/css_boxmodel.asp)
  
 ## Javascript/ES6
 * var -> scoped either globally or locally, depending on the placement outside or inside the function/class. Can be redeclared.
 * let -> scoped only within the block it resides, cannot be redeclared.
 * const -> constant variable, cannot change value directly and cannot be redeclared.
