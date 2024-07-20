<p align="center">
<img alt="kz logo" height="48" src="https://raw.githubusercontent.com/i11n/.github/main/svg/kz/color/kz.svg" />
<strong>observe</strong>
</p>

<p align="center">
kz is a collection of easy-to-use utility and feature libraries for creating anything you want with the <a href="https://deno.com">Deno</a> runtime.
</p>

<h1 align="center">@kz/observe</h1>

<p align="center">
The <code>@kz/observe</code> module provides types and features for implementing the observer pattern.
</p>

<p align="center">
<a href="https://jsr.io/@kz/observe">Overview</a> |
<a href="https://jsr.io/@kz/observe/doc">API Docs</a>
</p>

The observer pattern is a software design pattern in which an object, called
the subject or observable, maintains a list of its dependents, called
observers, and notifies them of state changes, usually by calling one of
their methods.

## Examples

```ts
import { TAbstractObserver, TBaseObservable } from './mod.ts';

interface ILocation {
  lat: number;
  lon: number;
}

class LocationReporter extends TBaseObservable<ILocation> {
  public publish(location: ILocation): void {
    try {
      if (location.lat < -90 || location.lat > 90) {
        throw new Error('Invalid latitude');
      }

      super.publish(location);
    } catch (error) {
      this.onError(error);
    }
  }
}

class LocationTracker extends TAbstractObserver<ILocation> {
  protected _locations: ILocation[] = [];

  public get locations(): ILocation[] {
    return this._locations;
  }

  public next(location: ILocation): void {
    if (this.subscription && this.subscription.isDisposed) return;

    this._locations.push(location);
  }

  public error(error: Error): void {
    console.log(error.message);
  }
}

const reporter = new LocationReporter();
const localTracker = new LocationTracker();
const remoteTracker = new LocationTracker();

const localSub = reporter.subscribe(localTracker);
const remoteSub = reporter.subscribe(remoteTracker);

reporter.publish({ lat: 37.7749, lon: -122.4194 });
reporter.publish({ lat: 40.7128, lon: -74.0060 });

console.log(localTracker.locations.length); // 2
console.log(remoteTracker.locations.length); // 2

localSub.dispose();

reporter.publish({ lat: 51.5074, lon: -0.1278 });

remoteSub.dispose();

console.log(localTracker.locations.length); // 2
console.log(remoteTracker.locations.length); // 3
```

## Contributing

Contributions are welcome! Take a look at our [contributing guidelines][contributing] to get started.

<p align="center">
<a href="https://github.com/i11n/.github/blob/main/.github/CODE_OF_CONDUCT.md">
  <img alt="Contributor Covenant" src="https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg?style=flat-square" />
</a>
</p>

## License

The MIT License (MIT) 2022 integereleven. Refer to [LICENSE][license] for details.

<p align="center">
<sub>Built with ❤ by integereleven</sub>
</p>

<p align="center">
<img
  alt="kz.io logo"
  height="64"
  src="https://raw.githubusercontent.com/i11n/.github/main/svg/brand/color/open-stroke.svg"
/>
</p>

<p align="center">
<a href="https://github.com/kz-io/observe/commits">
  <img alt="GitHub commit activity" src="https://img.shields.io/github/commit-activity/m/kz-io/observe?style=flat-square">
</a>
<a href="https://github.com/kz-io/observe/issues">
  <img alt="GitHub issues" src="https://img.shields.io/github/issues-raw/kz-io/observe?style=flat-square">
</a>
<a href="https://codecov.io/gh/kz-io/observe" >
  <img src="https://codecov.io/gh/kz-io/observe/graph/badge.svg?token=EK5CNEBUPG"/>
</a>
</p>

<p align="center">
<a href="https://jsr.io/@kz/observe">
  <img src="https://jsr.io/badges/@kz/observe" alt="" />
</a>
<a href="https://jsr.io/@kz/observe">
  <img src="https://jsr.io/badges/@kz/observe/score" alt="" />
</a>
</p>

[deno]: https://deno.dom "Deno homepage"
[jsr]: https://jsr.io "JSR homepage"
[branches]: https://github.com/kz-io/observe/branches "@kz/observe branches on GitHub"
[releases]: https://github.com/kz-io/observe/releases "@kz/observe releases on GitHub"
[contributing]: https://github.com/kz-io/observe/blob/main/CONTRIBUTING.md "@kz/observe contributing guidelines"
[license]: https://github.com/kz-io/observe/blob/main/LICENSE "@kz/observe license"
