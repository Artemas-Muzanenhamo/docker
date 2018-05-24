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
    <img src="https://user-images.githubusercontent.com/29547780/40476005-e5f66ada-5f3a-11e8-8094-68be8164bc14.png">
</p>