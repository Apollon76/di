# Independency
Independency is a DI container library. Unlike many other Python DI containers Independency operates in the local scope. It's inspired by [punq](https://github.com/bobthemighty/punq), so the API is very similar.

Independency supports generics and other specific typings.


## Installation

```bash
pip install independency
```

## Quick start
Independency avoids global state, so you must explicitly create a container in the entrypoint of your application:

```
import independency

builder = independency.ContainerBuilder()
# register application dependencies
container = builder.build()
```

## Examples
```python3
import requests

from independency import Container, ContainerBuilder


class Config:
    def __init__(self, url: str):
        self.url = url


class Getter:
    def __init__(self, config: Config):
        self.config = config

    def get(self):
        return requests.get(self.config.url)


def create_container() -> Container:
    builder = ContainerBuilder()
    builder.singleton(Config, Config, url='http://example.com')
    builder.singleton(Getter, Getter)
    return builder.build()


def main():
    container = create_container()
    getter: Getter = container.resolve(Getter)
    print(getter.get().status_code)


if __name__ == '__main__':
    main()
```
