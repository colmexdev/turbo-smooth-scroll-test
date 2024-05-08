# turbo-smooth-scroll-test
Minimal reproduction of issue with smooth scrolling on anchor navigation with Turbo v8.0.4 (as for now, only happening on Firefox). Empty hash (`<a href="#">`) and visible elements (on viewport) seem to comply with smooth scrolling; otherwise, scroll happens to be abrupt.

While `data-turbo="false"` seems to solve the problem, it seems that something else is going on. After testing on Chrome, it works as expected without the need of disabling Turbo on the element.

Not sure if this happens in other browsers, I can't test on any other unfortunately.

## Update
Smooth scroll seems to be respected now if the following modifications are made:
- On `class View`, line _1449_:
```javascript
focusElement(element) {
  if (element instanceof HTMLElement) {
    if (element.hasAttribute("tabindex")) {
      element.focus(); // Here I added {preventScroll: true}, considering that previous action is scrollIntoView();
    } else {
      element.setAttribute("tabindex", "-1");
      element.focus(); //Same here, added {preventScroll: true}
      element.removeAttribute("tabindex");
    }
  }
}
``` 
- On `class Navigator`, line _3994_:
```javascript
locationWithActionIsSamePage(location, action) {
  const anchor = getAnchor(location);
  const currentAnchor = getAnchor(this.view.lastRenderedLocation);
  const isRestorationToTop = action === "restore" && typeof anchor === "undefined";
  return (
    action !== "replace" &&
    getRequestURL(location) === getRequestURL(this.view.lastRenderedLocation) &&
    (isRestorationToTop || (anchor != null && anchor !== currentAnchor)) // Here I removed condition 'anchor !== currentAnchor'
  )
}
``` 
Tested on Firefox and Chrome. Can't test on other browsers unfortunately.
