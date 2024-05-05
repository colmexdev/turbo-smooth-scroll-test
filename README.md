# turbo-smooth-scroll-test
Minimal reproduction of issue with smooth scrolling on anchor navigation with Turbo v8.0.4 (as for now, only happening on Firefox). Empty hash (`<a href="#">`) and visible elements (on viewport) seem to comply with smooth scrolling; otherwise, scroll happens to be abrupt.

While `data-turbo="false"` seems to solve the problem, it seems that something else is going on. After testing on Chrome, it works as expected without the need of disabling Turbo on the element.

Not sure if this happens in other browsers, I can't test on any other unfortunately.
