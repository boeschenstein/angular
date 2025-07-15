# Angular

## Replace global Error handler

<https://youtu.be/XJTc-Sy3JxQ>

- create a new error handler
- replace existing error handler using this:

```
(provide:ErrorHandler useClass:MyGlobalErrorhandlerService
```

- Add Stackify.js to get better log information 
- send everything to your backend and log it there

## base href - add url path

### Url Path

```
foo://example.com:8042/over/there?name=ferret#nose
\_/   \______________/\_________/ \_________/ \__/
 |           |            |            |        |
scheme    authority      path        query   fragment
```

Source: <https://angular.io/guide/router#html5-urls-and-the-base-href>

### Compile app to add path

Compile your ng app using `ng build --base-href <path>`

Example: `ng build --base-href lentilsAreTheBest`

Details see: <https://angular.io/cli/build>

The only change I see is in `index.html`:

```html
<head>
  <base href="/lentilsAreTheBest/">
</head>
```

### Usage

`http:\\myDns[:port]\<path>`

### Test base href locally 

That is the only webserver I found, which can provide href option: <https://www.npmjs.com/package/angular-http-server>

>Note: numbers only in base href (like `/1234`) are not working as it seems

Source: <https://dev.to/isthatcentered/testing-an-angular-build-with-base-href-locally-44af>

# Update to standalone components

## Step 1: use a tool

<https://angular.dev/reference/migrations/standalone>

> if you use nx, use `npx nx g @angular/core:standalone`

Run the migration in the order listed below, verifying that your code builds and runs between each step:
1. Run `ng g @angular/core:standalone` and select "Convert all components, directives and pipes to standalone"
2. Run `ng g @angular/core:standalone` and select "Remove unnecessary NgModule classes"
3. Run `ng g @angular/core:standalone` and select "Bootstrap the project using standalone APIs"
4. Run any linting and formatting checks, fix any failures, and commit the result

## Step 2: inject services

Details see: <https://angular.dev/errors/NG0201>

> if you get this error: `NullInjectorError: No provider for ...`

old:

```ts
@Injectable()
export class MyService { .. }
```

new: add `{ providedIn: 'root' }`

```ts
@Injectable({ providedIn: 'root' })
export class MyService { .. }
```

## Step 3: remove (Router) Module

Details see: <https://dev.to/ngrx/using-ngrx-packages-with-standalone-angular-features-53d8>

Remove this @NgModule class definition part in `app.routs.ts` in `main.ts`:

```ts
@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

That's it, because the 3 tools calls already added this `provideRouter(appRoutes)` : 

```ts
import { appRoutes } from './app/app.routes';

defineCustomElements();

bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(appRoutes, withEnabledBlockingInitialNavigation())
  ]
}).catch(err => console.error(err));
```

## Step 4: NgRx

> Error: The feature name "myFeature" does not exist in the state, therefore createFeatureSelector cannot access it.  Be sure it is imported in a loaded module using StoreModule.forRoot('myFeature', ...) or StoreModule.forFeature('myFeature', ...).  If the default state is intended to be undefined, as is the case with router state, this development-only warning message can be ignored.

todo

# Information

- Node Version Swicher: <https://github.com/jasongin/nvs>
