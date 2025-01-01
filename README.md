# yandex_practicum_architecture
This is Yandex Practicum Software Architecture course projects repo

### Task 1 - splitting into microfrontends

During project analysis I split it into the following modules using vertical decomposition or DDD:
- `Signup`. It's responsible for user registration.
- `Login`. It's responsible for user signin and returning JSON Web Token (JWT).
- `Profile`. It's responsible for changing user profile info and it's avatar.
- `Cards`. It's responsible for adding new photos, removing existing ones and change like status.
- `Host`. It's responsible for aggregation of all microfrontends, validating JWT
- `Common`. It contains reusable components to be used in other microfrontends - PopupWithForm

Design schema represented here:
![alt text](https://github.com/voometin/yandex_practicum_architecture/blob/sprint_1/microfrontends.png?raw=true)

I chose to use Webpack Module Federation framework, because all the project uses the same frontend library React and at that moment doesn't seem to be to complex and doesn't contain a lot of interactive components. Using multiple technologies makes the project more complex for supporting. Single SPA is more appropriate for multiple technologies when it's justified. Now it's seemed that using of multiple frameworks for separate microfrontends is not justified. Using WMF it seems to be easy to implement sharing `common` component between multiple microfrontends.

`Signup` and `Login` modules are separeted for service fault-tolerance. If there would be some DoS attack on 1 microfrontend or some high load on 1 of the microfronted - it doesn't affect other module. Also it's easier to scale different microfrontends independently.

Currently `common` module implemented as microfrontend and multiple microfrontends (`Cards`, `Profile`, `Host`) call it inside itself. It has some drawbacks - it's a bottleneck for other microfrontends. If `common` module will be affected by high load of some microfrontends, then it can affect other modules. It's better to implement it as a package and add it to Node Package Registry. After that each microfrontend can use it as module independently and scale easily (but i didnt dive deep how to do it).

<ins>The project was not tested if it works</ins>

