# todobox

## Project setup
Install Vue CLI
```
npm install -g @vue/cli
```
Install required packages
```
pnpm install
```
Compile and run in development
```
pnpm run serve
```

*(This project does not use any third-party libraries.)*

## Technical implementation and short description of features
The main functionality of the app is implemented in the **TodoList** component. On page load, there are two todo lists shown:
1. A list of to-dos that need to be done
2. A list of to-dos that are considered done

Both of these rendered lists use the same `data` object that's shared via props. The differentiation of the two lists is done via the `listType` prop. Using that differentiator enables me to conditionally render markup inside the component. There is a `LIST_TYPE.js` javascript file in the constants folder that's used to avoid *magic values*.

Apart from the `listType` prop, there are events that the parent component is listening to (`@tick-todo-item, @add-todo-item`...)

Because the two lists interact with the same state, the state is *lifted* up to the nearest parent, which in this case is `TodoListContainer`.

The drag and drop functionality is achieved using the native Drag and Drop API provided by the browser. First, the item we want to drag gets the attribute `draggable` which is then set to true. After tagging the necessary item, we listen to the `@dragstart` event by passing in a function that can process the dragged element. In the `startDrag` function we set a payload that consists of the `itemId` as well as the `dragStartListType` property from the dragged list. The second property we pass (`dragStartListType`) is necessary because we have to disable dragging and dropping into the same list.

After setting up the *dragging* portion, we define a *drop zone*. We then set up `@drop`, so the list can listen to any incoming drop events. In order to override default behavior we must do `@dragenter.prevent` and `@dragover.prevent`.

In the parent component, we define the drop *handler* which as the name suggests, handles the correct behavior of dropping to-do items. It checks the `listType` of the list where the *drag* event happened, and based on that determines where to put the item. In short:
- If it gets dropped into the to-do list, that means the dragged item is from the done list
- If it gets dropped into the done list, that means the dragged item is from the to-do list

Besides the drag and drop feature, there is an option to delete *all* done items using the delete icon above. Another way to delete items would be using the ellipsis (three dot) button. Hover over an item, click it and then select the option 'Delete' in the modal menu.

Clicking on the + sign, a new item is added into the to-do list.

The functionality of editing a singular item inside the to-do list (and not the done list) is achieved using two-way binding with `v-model="item.value"`.