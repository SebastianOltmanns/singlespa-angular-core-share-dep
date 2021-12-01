This project tries to share dependencies over multiple microfrontends. There are three projects: 
- `root`: the root config
- `p1`: microfrontend 1
- `p2`: microfrontend 2

The root config loads a systemJS importmap, containing the line
```
"@angular/core": "https://cdn.jsdelivr.net/npm/@esm-bundle/angular__core@12.2.0/system/es2015/ivy/angular-core.min.js",
```

P1 is getting bundled without `@angular/core`, due to the externals feature of webpack:
```
externals: {
  '@angular/core': '@angular/core',
}
```

P2 has the exact same content as P1, except that P2 is being bundled with `@angular/core`.

Sadly, when running everything and visiting `http://localhost:9000/p1`, the console reports error:
```
application 'p1' died in status SKIP_BECAUSE_BROKEN: StaticInjectorError(Platform: core)[N_]: 
  NullInjectorError: No provider for N_!
```

Visiting `http://localhost:9000/p2` works.

to run the root config: 
```
cd root
npm install
npm run start
```

to run project 1 (p1): 
```
cd p1
npm install
npm run serve:single-spa:p1
```

to run project 2 (p2): 
```
cd p2
npm install
npm run serve:single-spa:p2
```
