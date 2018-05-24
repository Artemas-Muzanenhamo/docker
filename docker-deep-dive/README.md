# Docker Deep Dive

## Architecture Big Picture

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40360546-748d9f4e-5dbe-11e8-8a8b-567e6c6ef254.png">
</p>

Firstly a container by definition is an _isolated area of an OS with resource usage limits applied_.

Now, to build them, we've leverage a bunch of Kernel low level stuff. In particular we use _Namespaces_ and _Control Groups_ as below.

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40361125-350c9dfa-5dc0-11e8-946d-9b615bee7e61.png">
</p>

The _Docker Engine_ is just like any other engine, it is modular.
We might interface with it through the CLI but under the covers it's just a bunch of smaller moving parts.

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40476138-31ca726c-5f3b-11e8-8fef-c5596a56a1ec.png">
</p>

So the workflow of the diagram above is as follow:

* We use a command line to create a container: `docker container run`
* The client takes the command and makes the appropriate API request to the _containers/create_ endpoint in the engine.
* The docker engine then pulls all of the required kernel stuff _(namespaces, groups)_ and _pop!_ Out comes a container.