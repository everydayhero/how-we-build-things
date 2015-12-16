# User Interfaces

Hmm.. Plural? How many do we have, then..

- everydayhero NFP
    - The code is [here](https://github.com/everydayhero/heroix/tree/master/iron)
    - Uses [react-router](https://github.com/rackt/react-router)
    - Static content, served by nginx
    - First page load sends HTML, only JSON thereafter

- NFP profiles
    - Universal app.
    - First page load sends HTML, only JSON thereafter
    - Uses [react-router](https://github.com/rackt/react-router)
    - The code is [here](https://github.com/everydayhero/nfp-profile)

- Supporter
    - Mostly server-sent HTML, via Rails.
    - Some scattered React / Ember usage.
    - No JS routers

- Professional services and virtual-event microsites

- Legacy UIs
    - Heroix, Admin, Core etc
    - Server-sent HTML, via Rails (1 or 2)
    - JS, if any, is a nasty rats nest of jQuery or Prototype
