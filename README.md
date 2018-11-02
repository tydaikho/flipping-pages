# flipping-pages

React component for flipping book pages animation

[Demo](https://namannehra.github.io/flipping-pages/)

## Warning

Turning by swiping is only supported in browsers that support pointer events.
[Check support](https://caniuse.com/#feat=pointer)

## Installation

```
yarn add flipping-pages
```

or

```
npm install flipping-pages
```

## Usage

```javascript
import React, {PureComponent} from 'react'
import FlippingPages from 'flipping-pages'
import 'flipping-pages/FlippingPages.css' // IMPORTANT

class App extends PureComponent {
    constructor(props) {
        super(props)
        this.state = {
            selected: 0,
        }
        this.handleSelectedChange = this.handleSelectedChange.bind(this)
    }
    handleSelectedChange(selected) {
        this.setState({selected})
    }
    render() {
        const flippingPagesStyle = {
            height: 500,
            width: 500,
            perspective: 1000,
        }
        return (
            <FlippingPages
                direction="horizontal"
                style={flippingPagesStyle}
                selected={this.state.selected}
                onSelectedChange={this.handleSelectedChange}
            >
                <div>page 1</div>
                <div>page 2</div>
                <div>page 3</div>
            </FlippingPages>
        )
    }
}

export default App
```

## Props

| Name                | Type     | Default                    | Required |
|---------------------|----------|----------------------------|----------|
| `animationDuration` | `number` | `500`                      |          |
| `direction`         | `string` |                            | YES      |
| `onOverSwipe`       | `func`   | `overSwpie => swpie / 4`   |          |
| `onSelectedChange`  | `func`   |                            |          |
| `onSwipeStart`      | `func`   | `event => event.isPrimary` |          |
| `onTurn`            | `func`   |                            |          |
| `reverse`           | `bool`   | `false`                    |          |
| `rootRef`           | `object` |                            |          |
| `selected`          | `number` |                            | YES      |
| `shadowBackground`  | `string` | `rgba(0,0,0,0.25)`         |          |
| `swipeLength`       | `number` | `500`                      |          |
| `thresholdSpeed`    | `number` | `0.1`                      |          |

### `animationDuration`
Time in milliseconds for one page turn. Set to `0` to disable animation.

### `direction`
`horizontal` or `vertical`.

### `onOverSwipe(overSwpie) => swipe`
Called when the user swipes back on the first page or forword on the last page.
`overSwpie` is a number between `0` (not included) and `1`. Return value should
be a number between `0` and `1`. Turning is throttled based return value.

### `onSelectedChange(selected) => void`
Called when the user tries to turn the page by swiping. `selected` is the page
index user tried to turn to.

### `onSwipeStart(event) => shouldSwipe`
Called when the user taps. `event` is a `pointerdown` event. Turning starts only
if returned value is `true`. Can be used to disable swiping for certain pointer
types like mouse or completely disable swiping.
[Read more](https://developer.mozilla.org/en-US/docs/Web/API/PointerEvent)

### `onTurn(turn) => void`
Called everytime page turns. Can be used to check when turning animation is
complete. Animation will be complete when `turn` is equal to `selected` `prop`.

### `reverse`
Reverses the direction page turn. Can be used with horizontal direction for
right to left languages.

### `rootRef`
`ref` to the root element.

### `selected`
Index of the page to be displayed.

### `shadowBackground`
Pages have a shadow when they turn. Shadow has `0` opacity when page is resting
and `1` opacity half turned. This `prop` is the `CSS` `background` for the
shadow.

### `swipeLength`
The distance in pixels user must swipe to completely turn a page.

### `thresholdSpeed`
Minimum speed in pixels per milliseconds to register a swipe.

## Notes

* You should set CSS `user-select` property to `none` so text and images don't
  get selected when user drags with mouse.
  [Read more](https://developer.mozilla.org/en-US/docs/Web/CSS/user-select)

* You should set CSS `perspective` property for 3D effect when pages turn. Value
  double of element's height (width if `direction` is `horizontal`) is
  recomended.
  [Read more](https://developer.mozilla.org/en-US/docs/Web/CSS/perspective)

## Limitations

### No states or uncontrolled components
Web platform doesn't provide a way to bend an element in half. To achive this
effect, this component renders the page to be displayed twice. First time, the
last half of the page is hidden. Second time, the first half of the page is
hidden in one time and hind the second half other time. Then these halfs are
rotated independently. So if a page has state and that state is changed in first
render then there is no way to sync that changes to the second render. The same
is true for uncontrolled components. So this component can only be used for
static and stateless content.