# Adonis Manager
> The build (Manager) pattern base implementation

## DEPRECIATED (Use https://github.com/poppinss/manager instead)

[![travis-image]][travis-url]
[![appveyor-image]][appveyor-url]
[![coveralls-image]][coveralls-url]
[![npm-image]][npm-url]
![](https://img.shields.io/badge/Uses-Typescript-294E80.svg?style=flat-square&colorA=ddd)

The `Manager` pattern is used by AdonisJs all over the place to expose `driver` based API using fluent syntax.

**The implementation is in Typescript and require you to write in Typescript too**.

You are also free to make use of it inside your own packages (if required).

## Installation

```shell
npm i @adonisjs/manager

# Yarn
yarn add @adonisjs/manager
```

## Usage
The class exposed by this module is an abstract class. You will have to create the actual implementation which extends this class (as shown below).

Let's say we are building a `Mailer` which allows users to switch drivers or add new drivers at runtime.

```ts
import { Manager } from '@adonisjs/manager'

interface IMailDriver {
  send (view: string, callback: (message) => void)
}

class Mail extends Manager<IMailDriver> {
  protected _cacheDrivers = true

  constructor (proctected app) {
    super(app)
  }

  protected getDefaultDriver () {
    return 'smtp'
  }

  public createSmtp (): IMailDriver {
    return // SMTP driver instance
  }
}
```

1. The `_cacheDrivers` property will cache the instances of mail drivers and will not create a new one everytime.
2. The `getDefaultDriver` method returns the default driver to be used. You can fetch this information from the `Config` too.
3. The `app` variable in the constructor is the reference to the IoC container object. This is passed to the extended drivers callback.

```ts
const ioc = // get it from somewhere
const mail = new Mail(ioc)

mail.driver('smtp').send()
```

### Extending drivers
New drivers can be added using the `extend` method implemented within the `Manager` class.

```ts
Mail.extend('mailgun', (app) => {
  return new Mailgun(app.use('Config'))
})
```

## Change log

The change log can be found in the [CHANGELOG.md](CHANGELOG.md) file.

## Contributing

Everyone is welcome to contribute. Please go through the following guides, before getting started.

1. [Contributing](https://adonisjs.com/contributing)
2. [Code of conduct](https://adonisjs.com/code-of-conduct)


## Authors & License
[Harminder Virk](https://github.com/thetutlage) and [contributors](https://github.com/adonisjs/adonis-manager/graphs/contributors).

MIT License, see the included [MIT](LICENSE.md) file.

[travis-image]: https://img.shields.io/travis/adonisjs/adonis-manager/master.svg?style=flat-square&logo=travis
[travis-url]: https://travis-ci.org/adonisjs/adonis-manager "travis"

[appveyor-image]: https://img.shields.io/appveyor/ci/thetutlage/adonis-manager/master.svg?style=flat-square&logo=appveyor
[appveyor-url]: https://ci.appveyor.com/project/thetutlage/adonis-manager "appveyor"

[coveralls-image]: https://img.shields.io/coveralls/adonisjs/adonis-manager/master.svg?style=flat-square
[coveralls-url]: https://coveralls.io/github/adonisjs/adonis-manager "coveralls"

[npm-image]: https://img.shields.io/npm/v/@adonisjs/manager.svg?style=flat-square&logo=npm
[npm-url]: https://npmjs.org/package/@adonisjs/manager "npm"
